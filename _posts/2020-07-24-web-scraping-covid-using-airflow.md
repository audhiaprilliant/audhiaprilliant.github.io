---
layout: post
title:  "Apache Airflow as Job Orchestration for Web Scraping of Covid-19"
author: audhi
categories: [ airflow,python ]
tags: [programming]
image: assets/images/12-0.jpg
---

### Overview
Airflow is a platform to programmatically author, schedule, and monitor workflows. Use airflow to author workflows as directed acyclic graphs (DAGs) of tasks. The airflow scheduler executes our tasks on an array of workers while following the specified dependencies. Basically, it help us to automate the script. Meanwhile, the COVID‑19 pandemic, also known as the coronavirus pandemic, is an ongoing global pandemic of coronavirus disease 2019 (COVID‑19), caused by severe acute respiratory syndrome coronavirus 2 (SARS‑CoV‑2). The outbreak was first identified in Wuhan, China, in December 2019. We need to monitor the data in Indonesia, daily. Further, Kompas news is one of the platform which updates the data in dashboard at [__here__](https://www.kompas.com/covid-19)

### Prerequisites
Before talking more intensively, please read and set up following tools properly
1. Install Apache Airflow [__read here__](https://audhiaprilliant.github.io/airflow-documentation/)
2. Install the module dependencies
   - __requests__ for web scraping
   - __bs4__ for web scraping using BeautifulSoup
   - __pandas__ for data manipulation
   - __re__ for regular expression
   - __os__ for file management
   - __datetime__ for timing
   - __json__ for reading JSON file
3. Telegram
   - Telegram token [__read here__](https://www.freecodecamp.org/news/telegram-push-notifications-58477e71b2c2/)
   - Telegram chat ID [__read here__](https://www.freecodecamp.org/news/telegram-push-notifications-58477e71b2c2/)
4. Email
   - Apps password that contains 16 digits of characters [__read here__](https://helptechcommunity.wordpress.com/2020/04/04/airflow-email-configuration/)
   - set up `airflow.cfg` file to synchronize with our email
5. set up file and directory in Airflow
   - Save the DAG Python file in directory `dags`
   - Save Telegram chat ID in directory `config`
   - Create directory `data/covid19` in Airflow to store `summary_covid19.txt` and `daily_update_covid.csv`. Please [__click here__]({{ baseurl }}/assets/images/12-4.png) to look up the detail of recommended directory.

### Full article
You can read full article at [__Medium__](https://medium.com/analytics-vidhya/apache-airflow-as-job-orchestration-e207ba5b4ac5)
