---
layout: post
title:  "Apache Airflow as Workflow Management System"
author: audhi
categories: [ airflow, python ]
tags: [programming]
image: assets/images/10-0.jpg
---

### Overview
Airflow is a platform to programmatically author, schedule and monitor workflows. Use Airflow to author workflows as Directed Acyclic Graphs (DAGs) of tasks. The Airflow scheduler executes your tasks on an array of workers while following the specified dependencies. Rich command line utilities make performing complex surgeries on DAGs a snap. The rich user interface makes it easy to visualize pipelines running in production, monitor progress, and troubleshoot issues when needed.

When workflows are defined as code, they become more maintainable, versionable, testable, and collaborative.

### Principles
- **Dynamic**: Airflow pipelines are configuration as code (Python), allowing for dynamic pipeline generation. This allows for writing code that instantiates pipelines dynamically.
- **Extensible**: Easily define your own operators, executors and extend the library so that it fits the level of abstraction that suits your environment.
- **Elegant**: Airflow pipelines are lean and explicit. Parameterizing your scripts is built into the core of Airflow using the powerful Jinja templating engine.
- **Scalable**: Airflow has a modular architecture and uses a message queue to orchestrate an arbitrary number of workers. Airflow is ready to scale to infinity.

### Beyond the Horizon
Airflow is not a data streaming solution. Tasks do not move data from one to the other (though tasks can exchange metadata!). Airflow is not in the Spark Streaming or Storm space, it is more comparable to Oozie or Azkaban.

Workflows are expected to be mostly static or slowly changing. You can think of the structure of the tasks in your workflow as slightly more dynamic than a database structure would be. Airflow workflows are expected to look similar from a run to the next, this allows for clarity around unit of work and continuity.

### Installation
#### Getting Airflow
The easiest way to install the latest stable version of Airflow is with pip:
```bash
pip install apache-airflow
```
You can also install Airflow with support for extra features like `gcp` or `postgres`:
```bash
pip install apache-airflow[gcp,postgres]
```

#### Initiating Airflow Database
Airflow requires a database to be initiated before you can run tasks. If you’re just experimenting and learning Airflow, you can stick with the default SQLite option. If you don’t want to use SQLite, then take a look at Initializing a Database Backend to setup a different database.

After configuration, you’ll need to initialize the database before you can run tasks:
```bash
airflow initdb
```

### It’s a DAG Definition File
One thing to wrap your head around (it may not be very intuitive for everyone at first) is that this Airflow Python script is really just a configuration file specifying the DAG’s structure as code. The actual tasks defined here will run in a different context from the context of this script. Different tasks run on different workers at different points in time, which means that this script cannot be used to cross communicate between tasks. Note that for this purpose we have a more advanced feature called `XCom`.

People sometimes think of the DAG definition file as a place where they can do some actual data processing - that is not the case at all! The script’s purpose is to define a DAG object. It needs to evaluate quickly (seconds, not minutes) since the scheduler will execute it periodically to reflect the changes if any.

In Airflow all workflows are DAGs. A Dag consists of operators. An operator defines an individual task that needs to be performed. There are different types of operators available( As given on Airflow Website):
- `BashOperator` - executes a bash command
- `PythonOperator` - calls an arbitrary Python function
- `EmailOperator` - sends an email
- `SimpleHttpOperator` - sends an HTTP request
- `MySqlOperator`, `SqliteOperator`, `PostgresOperator`, `MsSqlOperator`, `OracleOperator`, `JdbcOperator`, etc. - executes a SQL command
- `Sensor` - waits for a certain time, file, database row, S3 key, etc

### How to Run Airflow and Scheduler
It's pretty easy to run the Apache Airflow and Scheduler. First, open your terminal and follow below commands!
```bash
# airflow needs a home, ~/airflow is the default,
# but you can lay foundation somewhere else if you prefer
# (optional)
export AIRFLOW_HOME=~/airflow

# initialize the database
airflow initdb

# start the web server, default port is 8080
airflow webserver -p 8080

# start the scheduler
airflow scheduler

# visit localhost:8080 in the browser and enable the example dag in the home page
```
Upon running these commands, Airflow will create the `$AIRFLOW_HOME` folder and lay an "airflow.cfg" file with defaults that get you going fast. You can inspect the file either in `$AIRFLOW_HOME/airflow.cfg`, or through the UI in the `Admin->Configuration` menu. The PID file for the webserver will be stored in `$AIRFLOW_HOME/airflow-webserver.pid` or in `/run/airflow/webserver.pid` if started by systemd.

For the next post, I will do explain how could I define my pipeline for Covid-19 data using web scraping over Kompas news. Please stay tune!

### Sources
<a target="_blank" href="https://airflow.readthedocs.io/en/stable/i" class="btn btn-danger">Airflow Documentation</a> <a target="_blank" href="https://towardsdatascience.com/getting-started-with-apache-airflow-df1aa77d7b1b#:~:text=Airflow%20is%20a%20platform%20to,while%20following%20the%20specified%20dependencies." class="btn btn-warning">Towards Data Science</a>
