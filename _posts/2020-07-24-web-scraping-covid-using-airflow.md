---
layout: post
title:  "Apache Airflow as Job Orchestration for Web Scraping of Covid-19"
author: audhi
categories: [ airflow,python ]
tags: [programming]
image: assets/images/12-0.jpg
---

### Overview
Airflow is a platform to programmatically author, schedule, and monitor workflows. Use airflow to author workflows as directed acyclic graphs (DAGs) of tasks. The airflow scheduler executes our tasks on an array of workers while following the specified dependencies. Basically, it help us to automate the script. Meanwhile, the COVID‑19 pandemic, also known as the coronavirus pandemic, is an ongoing global pandemic of coronavirus disease 2019 (COVID‑19), caused by severe acute respiratory syndrome coronavirus 2 (SARS‑CoV‑2). The outbreak was first identified in Wuhan, China, in December 2019. We need to monitor the data in Indonesia, daily. Further, Kompas news is one of the platform which updates the data in dashboard at [__here__]('https://www.kompas.com/covid-19')

### Prerequisites
Before talking more intensively, please read and set up following tools properly
1. Install Apache Airflow [__read here__]('https://audhiaprilliant.github.io/airflow-documentation/')
2. Install the module dependencies
   - __requests__ for web scraping
   - __bs4__ for web scraping using BeautifulSoup
   - __pandas__ for data manipulation
   - __re__ for regular expression
   - __os__ for file management
   - __datetime__ for timing
   - __json__ for reading JSON file
3. Telegram
   - Telegram token [__read here__]('https://www.freecodecamp.org/news/telegram-push-notifications-58477e71b2c2/')
   - Telegram chat ID [__read here__]('https://www.freecodecamp.org/news/telegram-push-notifications-58477e71b2c2/')
4. Email
   - Apps password that contains 16 digits of characters [__read here__]('https://helptechcommunity.wordpress.com/2020/04/04/airflow-email-configuration/')
   - set up `airflow.cfg` file to synchronize with our email
5. set up file and directory in Airflow
   - Save the DAG Python file in directory `dags`
   - Save Telegram chat ID in directory `config`
   - Create directory `data/covid19` in Airflow to store `summary_covid19.txt` and `daily_update_covid.csv`. Please [__click here__]({{ baseurl }}/assets/images/12-4.png) to look up the detail of recommended directory.

### How to Scrape Data from Kompas News
##### 1 Install and import the module dependencies
As mentioned on above prerequisites, first we must install all the module dependencies. These would have helped us to write our Python program properly. Easily, we can write out all the modules in `requirements.txt` file.
```txt
bs4
pandas
```
Furthermore, `requests`,`re`,`os`,`datetime`, and `json` are the built-in modules, so we don't need to install. After installing the modules that needed, import it as follows.
```python
# Modules for web scraping
import requests
from bs4 import BeautifulSoup
# Module for data manipulation
import pandas as pd
# Module for regular expression
import re
# Module for file management
import os
# Module for timing
from datetime import datetime
# Module for reading JSON file
import json
```
##### 2 Get the URL of Kompas news page for daily monitoring Covid-19
This function is created to read the HTML page on Python. We use BeautifulSoup for wrangling the HTML elements.
```python
def get_url():
    # URL
    url = 'https://www.kompas.com/covid-19'
    page = requests.get(url)
    # Wrangling HTML with BeautifulSoup
    soup = BeautifulSoup(page.content,'html.parser')
    return(soup)
```
##### 3 Get current date
The updated time can be a metadata to track the up to date data for daily monitoring and store it into our local directory. The time is scraped and save it in original Indonesia Western Time (WIB). We do some transformation to make it in standard form of `YY/MM/DD`.
<p align="center">
<img src="{{ site.baseurl }}/assets/images/12-1.png" alt="drawing" width="700"/>
</p>
```python
dict_month = {'Januari':'01','Februari':'02','Maret':'03','April':'04','Mei':'05','Juni':'06','Juli':'07',
              'Agustus':'08','September':'09','Oktober':'10','November':'11','Desember':'12'}
def get_current_date(**kwargs):
    date_scrape = soup.find('span',class_='covid__date').text
    date_scrape = re.findall(r'Update terakhir: (\S+.+WIB)',date_scrape)[0].replace(', ',',')
    date = date_scrape.split(',')[0]
    time = date_scrape.split(',')[1]
    # Date manipulation
    date_format = re.findall(r'\w+',date)[0]
    month_format = re.findall(r'\w+',date)[1]
    year_format = re.findall(r'\w+',date)[2]
    # If condition
    if len(date_format) == 1:
        date_format = '0' + date_format
    else:
        date_format = date_format
    # New date format
    date = year_format+'/'+dict_month.get(month_format)+'/'+date_format
    return(date,time)
```
##### 4 Get the aggregated data
The daily aggregated consists of four information namely __confirmed__, __treated__, __deaths__, and __recovered__. 
<p align="center">
<img src="{{ site.baseurl }}/assets/images/12-2.png" alt="drawing" width="700"/>
</p>
```python
def get_daily_summary(**kwargs):
    soup = get_url()
    date,time = get_current_date()
    # Get summary
    # Regular expression pattern
    pattern_summary = re.compile(r'\d[^\s]+')
    for job_elem in soup.find_all('div',class_='covid__box'):
        # Each job_elem is a new BeautifulSoup object.
        terkonfirmasi_elem = job_elem.find('div',class_='covid__box2 -cases')
        dirawat_elem = job_elem.find('div',class_='covid__box2 -odp')
        meninggal_elem = job_elem.find('div',class_='covid__box2 -gone')
        sembuh_elem = job_elem.find('div',class_='covid__box2 -health')
        # Daily update
        a = pattern_summary.findall(terkonfirmasi_elem.text)[0].replace(',','')
        b = pattern_summary.findall(dirawat_elem.text)[0].replace(',','')
        c = pattern_summary.findall(meninggal_elem.text)[0].replace(',','')
        d = pattern_summary.findall(sembuh_elem.text)[0].replace(',','')
        daily_update = ','.join([date,time,a,b,c,d])
    return(daily_update)
```
##### 5 Save the aggregated data
This aggregated will be saved in extension of `txt`. Mostly, the Government does press conference at 16.00 WIB, it might be faster or slower. And Kompas team must update the current data to the dashboard. It can not be predict properly. So to make sure we will not get the duplicated data, the data must be checked with previous period.
```python
def summary_save_txt(**context):
    value = context['task_instance'].xcom_pull(task_ids = 'summary_scraping_data')
    with open(dir_path+'/data/covid19/summary_covid19.txt','r') as f:
        lines = f.read().splitlines()
        last_line = lines[-1]
        if last_line == value:
            notif = 'Last update:',re.findall(r'^(.+?),',last_line)[0]
        else:
            with open(dir_path+'/data/covid19/summary_covid19.txt','a+') as ff:
                ff.write('{}\n'.format(value))
                ff.close()
            notif = 'Up to date data:', re.findall(r'^(.+?),',value)[0]
    return(notif)
```
##### 6 Get the provinces data
This data contains the accumulated data for all provinces in Indonesia. So, we can conduct several analysis for selected province as we want and compare it to another.
<p align="center">
<img src="{{ site.baseurl }}/assets/images/12-3.png" alt="drawing" width="700"/>
</p>
```python
def get_daily_summary_provinces(**kwargs):
    soup = get_url()
    date,time = get_current_date()
    # Get summary - provinsi
    # Regular expression pattern
    pattern_prov = re.compile(r'\d+')
    provinsi = []
    terkonfirmasi_prov = []
    meninggal_prov = []
    sembuh_prov = []

    for elem in soup.find_all('div',class_='covid__row'):
        provinsi_elem = elem.find('div',class_='covid__prov')
        terkonfirmasi_elem = elem.find('span',class_='-odp')
        meninggal_elem = elem.find('span',class_='-gone')
        sembuh_elem = elem.find('span',class_='-health')
        # Append to list
        provinsi.append(provinsi_elem.text)
        terkonfirmasi_prov.append(pattern_prov.findall(terkonfirmasi_elem.text)[0])
        meninggal_prov.append(pattern_prov.findall(meninggal_elem.text)[0])
        sembuh_prov.append(pattern_prov.findall(sembuh_elem.text)[0])

    # Create dataframe
    dic_data = {'date':[date]*len(provinsi),'time':[time]*len(provinsi),'provinces':provinsi,
                'confirmed':terkonfirmasi_prov,'deaths':meninggal_prov,'recovered':sembuh_prov}
    df = pd.DataFrame(data = dic_data)
    return(df)
```
##### 7 Save the provinces data
```python
def provinces_save_csv(**context):
    value = context['task_instance'].xcom_pull(task_ids = 'provinces_scraping_data')
    with open(dir_path+'/data/covid19/daily_update_covid.csv','r') as f:
        lines = f.read().splitlines()
        last_line = lines[-1]
        if re.findall(r'^(.+?),',last_line)[0] == value['date'].unique().tolist()[0]:
            notif = 'Last update:',re.findall(r'^(.+?),',last_line)[0]
        else:
            with open(dir_path+'/data/covid19/daily_update_covid.csv','a') as ff:
                value.to_csv(ff,header=False,index=False)
                ff.close()
            notif = 'Up to date data:', value['date'].unique().tolist()[0]
    return(notif)
```
##### 8 Define DAG and set schedule
Before defining the DAG, we must set up default argument for tasks.
   - `owner` refers to the creator or owner. We can put our name or division
   - `depends_on_past` is assigned to `True` if the next task will be running when previous one is success and vice versa
   - `start_date` defines the start date of our airflow job
   - `email` must be assigned with our email. If any task is failed, the Airflow directly inform us
   - `retries` is the number of repetition if our task is failed before sending us an alert by email
   - `retry_delay` refers to the interval between two repetition

At last, it is required to determine our schedule in UTC, when our job will be running. Confused with the configuration of cron? Don't worry, click [__here__]('https://crontab.guru/'). It need to remember that __dag_id is unique in Airflow database__. For details of our DAG, you can look at following codes:
```python
# Set default args
default_args = {
    'owner': 'airflow',
    'depends_on_past': False,
    'start_date': datetime(2020, 5, 20),
    'email': ['your_email@gmail.com'],
    'email_on_failure': True,
    'email_on_retry': False,
    'retries': 3,
    'retry_delay': timedelta(minutes=2)
}

# Set Schedule: Run pipeline once a day.
# Use cron to define exact time (UTC). Eg. 8:15 AM would be '15 08 * * *'
schedule_interval = '30 09 * * *'

# Define DAG: Set ID and assign default args and schedule interval
dag = DAG(
    dag_id = 'scraping_data_covid19',
    default_args = default_args,
    schedule_interval = schedule_interval
)
```
##### 9 Create tasks
For this web scraping, we create six main tasks. Each task call the function we have define above. This is how we create task in Apache Airflow. Formally, we need to create tasks to show us beginning and ending of a job (`echo task start` and `echo task end`). Furthermore, an operator represents a single, ideally idempotent, task. Operators determine what actually executes when our DAG runs. We will use three operators:
   - `BashOperator` is used to execute commands in a Bash shell
   - `PythonOperator` for executing commands in Python
   - `EmailOperator` connects with our configuration in `airflow.cfg`, so we can send data or information to private email

```python
# Echo task start
task_start = BashOperator(
    task_id = 'start_task',
    bash_command = 'echo start',
    dag = dag
)

# Task 1: scraping daily summary data
summary_scraping = PythonOperator(
    task_id = 'summary_scraping_data',
    python_callable = get_daily_summary,
    dag = dag
)

# Task 2: save the daily summary data
summary_save = PythonOperator(
    task_id = 'summary_save_data',
    python_callable = summary_save_txt,
    provide_context = True,
    dag = dag
)

# Task 3: scraping daily provinces data
provinces_scraping = PythonOperator(
    task_id = 'provinces_scraping_data',
    python_callable = get_daily_summary_provinces,
    dag = dag
)

# Task 4: save the daily summary data
provinces_save = PythonOperator(
    task_id = 'provinces_save_data',
    python_callable = provinces_save_csv,
    provide_context = True,
    dag = dag
)

# Task 5: send norification email
send_email = EmailOperator(
    task_id = 'send_email',
    to = ['your_email@gmail.com'],
    subject = 'Covid19 data ',
    html_content = '''
    {date} Covid19 data has been scraped!
    '''.format(date = current_date),
    dag = dag
)

# Task 6: send notification Telegram
send_telegram = PythonOperator(
    task_id = 'send_telegram',
    python_callable = telegram_bot,
    provide_context = True,
    dag = dag
)

# Echo task finish
finish_start = BashOperator(
    task_id = 'finish_task',
    bash_command = 'echo finish',
    dag = dag
)
```
##### 10 set up task dependencies
```python
# Set up the dependencies
task_start >> summary_scraping >> summary_save >> provinces_scraping
provinces_scraping >> provinces_save >> send_email >> send_telegram >> finish_start
```
The task in Apache Airflow webserver
<p align="center">
<img src="{{ site.baseurl }}/assets/images/12-5.png" alt="drawing" width="700"/>
</p>

### Recap
To execute and run our web scraping job, we must create a `.py` file and save it in dags directory in Airflow. Then, running the webserver and scheduler, activate the job.
```python
# Modules for airflow
from airflow import DAG
from datetime import timedelta, datetime
from airflow.utils.dates import days_ago
from airflow.operators.bash_operator import BashOperator
from airflow.operators.python_operator import PythonOperator
from airflow.operators.email_operator import EmailOperator
# Modules for web scraping
import requests
from bs4 import BeautifulSoup
# Module for data manipulation
import pandas as pd
# Module for regular expression
import re
# Module for file management
import os
# Module for timing
from datetime import datetime
# Module for reading JSON file
import json

# Setting for datetime and directory
dir_path = '/home/audhi/airflow'
current_date = pd.to_datetime('today').strftime('%Y-%m-%d')
# Setting for date transformation
dict_month = {'Januari':'01','Februari':'02','Maret':'03','April':'04','Mei':'05','Juni':'06','Juli':'07',
              'Agustus':'08','September':'09','Oktober':'10','November':'11','Desember':'12'}
# Read JSON object as a dictionary 
with open(dir_path+'/config/telegram.json') as tele_data:
    telegram_bot = json.load(tele_data)
telegram_chatid = telegram_bot['result'][0]['message']['chat']['id']
telegram_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'

def get_url():
    # URL
    url = 'https://www.kompas.com/covid-19'
    page = requests.get(url)
    # Wrangling HTML with BeautifulSoup
    soup = BeautifulSoup(page.content,'html.parser')
    return(soup)

def get_current_date(**kwargs):
    date_scrape = soup.find('span',class_='covid__date').text
    date_scrape = re.findall(r'Update terakhir: (\S+.+WIB)',date_scrape)[0].replace(', ',',')
    date = date_scrape.split(',')[0]
    time = date_scrape.split(',')[1]
    # Date manipulation
    date_format = re.findall(r'\w+',date)[0]
    month_format = re.findall(r'\w+',date)[1]
    year_format = re.findall(r'\w+',date)[2]
    # If condition
    if len(date_format) == 1:
        date_format = '0' + date_format
    else:
        date_format = date_format
    # New date format
    date = year_format+'/'+dict_month.get(month_format)+'/'+date_format
    return(date,time)

def get_daily_summary(**kwargs):
    soup = get_url()
    date,time = get_current_date()
    # Get summary
    # Regular expression pattern
    pattern_summary = re.compile(r'\d[^\s]+')
    for job_elem in soup.find_all('div',class_='covid__box'):
        # Each job_elem is a new BeautifulSoup object.
        terkonfirmasi_elem = job_elem.find('div',class_='covid__box2 -cases')
        dirawat_elem = job_elem.find('div',class_='covid__box2 -odp')
        meninggal_elem = job_elem.find('div',class_='covid__box2 -gone')
        sembuh_elem = job_elem.find('div',class_='covid__box2 -health')
        # Daily update
        a = pattern_summary.findall(terkonfirmasi_elem.text)[0].replace(',','')
        b = pattern_summary.findall(dirawat_elem.text)[0].replace(',','')
        c = pattern_summary.findall(meninggal_elem.text)[0].replace(',','')
        d = pattern_summary.findall(sembuh_elem.text)[0].replace(',','')
        daily_update = ','.join([date,time,a,b,c,d])
    return(daily_update)

def get_daily_summary_provinces(**kwargs):
    soup = get_url()
    date,time = get_current_date()
    # Get summary - provinsi
    # Regular expression pattern
    pattern_prov = re.compile(r'\d+')
    provinsi = []
    terkonfirmasi_prov = []
    meninggal_prov = []
    sembuh_prov = []

    for elem in soup.find_all('div',class_='covid__row'):
        provinsi_elem = elem.find('div',class_='covid__prov')
        terkonfirmasi_elem = elem.find('span',class_='-odp')
        meninggal_elem = elem.find('span',class_='-gone')
        sembuh_elem = elem.find('span',class_='-health')
        # Append to list
        provinsi.append(provinsi_elem.text)
        terkonfirmasi_prov.append(pattern_prov.findall(terkonfirmasi_elem.text)[0])
        meninggal_prov.append(pattern_prov.findall(meninggal_elem.text)[0])
        sembuh_prov.append(pattern_prov.findall(sembuh_elem.text)[0])

    # Create dataframe
    dic_data = {'date':[date]*len(provinsi),'time':[time]*len(provinsi),'provinces':provinsi,
                'confirmed':terkonfirmasi_prov,'deaths':meninggal_prov,'recovered':sembuh_prov}
    df = pd.DataFrame(data = dic_data)
    return(df)

def summary_save_txt(**context):
    value = context['task_instance'].xcom_pull(task_ids = 'summary_scraping_data')
    with open(dir_path+'/data/covid19/summary_covid19.txt','r') as f:
        lines = f.read().splitlines()
        last_line = lines[-1]
        if last_line == value:
            notif = 'Last update:',re.findall(r'^(.+?),',last_line)[0]
        else:
            with open(dir_path+'/data/covid19/summary_covid19.txt','a+') as ff:
                ff.write('{}\n'.format(value))
                ff.close()
            notif = 'Up to date data:', re.findall(r'^(.+?),',value)[0]
    return(notif)

def provinces_save_csv(**context):
    value = context['task_instance'].xcom_pull(task_ids = 'provinces_scraping_data')
    with open(dir_path+'/data/covid19/daily_update_covid.csv','r') as f:
        lines = f.read().splitlines()
        last_line = lines[-1]
        if re.findall(r'^(.+?),',last_line)[0] == value['date'].unique().tolist()[0]:
            notif = 'Last update:',re.findall(r'^(.+?),',last_line)[0]
        else:
            with open(dir_path+'/data/covid19/daily_update_covid.csv','a') as ff:
                value.to_csv(ff,header=False,index=False)
                ff.close()
            notif = 'Up to date data:', value['date'].unique().tolist()[0]
    return(notif)

def telegram_bot(**context):
    value_daily = list(context['task_instance'].xcom_pull(task_ids = 'summary_scraping_data').split(','))
    value_date = context['task_instance'].xcom_pull(task_ids = 'provinces_save_data')
    value_render = ' '.join([str(elem) for elem in list(value_date)])
    message = '''
    COVID-19 DATA - {update_date} {time}
    Kompas News

    Confirmed: {confirmed}
    Treated: {treated} 
    Deaths: {deaths}
    Recovered: {recover}
    Scraped on: {scraped_date} WIB

    Developed by Audhi Aprilliant
    '''.format(update_date = value_render,time = value_daily[1],confirmed = value_daily[2],treated = value_daily[3],\
        deaths = value_daily[4],recover = value_daily[5],scraped_date = pd.to_datetime('today').strftime('%m-%d-%Y %H:%M:%S'))
    
	# Send text message
    bot_token = telegram_token
    bot_chatID = str(telegram_chatid)
    send_text = 'https://api.telegram.org/bot'+bot_token+'/sendMessage?chat_id='+bot_chatID+'&parse_mode=Markdown&text='+message
    requests.get(send_text)

# Set default args
default_args = {
    'owner': 'airflow',
    'depends_on_past': False,
    'start_date': datetime(2020, 5, 20),
    'email': ['your_email@gmail.com'],
    'email_on_failure': True,
    'email_on_retry': False,
    'retries': 3,
    'retry_delay': timedelta(minutes=2)
}

# Set Schedule: Run pipeline once a day.
# Use cron to define exact time (UTC). Eg. 8:15 AM would be '15 08 * * *'
schedule_interval = '30 09 * * *'

# Define DAG: Set ID and assign default args and schedule interval
dag = DAG(
    dag_id = 'scraping_data_covid19',
    default_args = default_args,
    schedule_interval = schedule_interval
)

# Echo task start
task_start = BashOperator(
    task_id = 'start_task',
    bash_command = 'echo start',
    dag = dag
)

# Task 1: scraping daily summary data
summary_scraping = PythonOperator(
    task_id = 'summary_scraping_data',
    python_callable = get_daily_summary,
    dag = dag
)

# Task 2: save the daily summary data
summary_save = PythonOperator(
    task_id = 'summary_save_data',
    python_callable = summary_save_txt,
    provide_context = True,
    dag = dag
)

# Task 3: scraping daily provinces data
provinces_scraping = PythonOperator(
    task_id = 'provinces_scraping_data',
    python_callable = get_daily_summary_provinces,
    dag = dag
)

# Task 4: save the daily summary data
provinces_save = PythonOperator(
    task_id = 'provinces_save_data',
    python_callable = provinces_save_csv,
    provide_context = True,
    dag = dag
)

# Task 5: send norification email
send_email = EmailOperator(
    task_id = 'send_email',
    to = ['your_email@gmail.com'],
    subject = 'Covid19 data ',
    html_content = '''
    {date} Covid19 data has been scraped!
    '''.format(date = current_date),
    dag = dag
)

# Task 6: send notification Telegram
send_telegram = PythonOperator(
    task_id = 'send_telegram',
    python_callable = telegram_bot,
    provide_context = True,
    dag = dag
)

# Echo task finish
finish_start = BashOperator(
    task_id = 'finish_task',
    bash_command = 'echo finish',
    dag = dag
)

# Set up the dependencies
task_start >> summary_scraping >> summary_save >> provinces_scraping
provinces_scraping >> provinces_save >> send_email >> send_telegram >> finish_start
```

### Sources
<a target="_blank" href="https://github.com/audhiaprilliant/Web-Scraping-Covid19-Kompas-News" class="btn btn-danger">GitHub</a>
