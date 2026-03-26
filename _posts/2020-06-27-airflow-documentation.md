---
layout: post
title:  "Intro to Apache Airflow"
subtitle: Pipelines made simple with Airflow
author: audhi
categories: [ article ]
tags: [ programming ]
---
Airflow is a platform to create, schedule, and monitor workflows. You can use Airflow to design workflows as Directed Acyclic Graphs (DAGs) of tasks. The Airflow scheduler runs your tasks on many workers and follows the order you set. Airflow has useful tools in the command line and a user interface to help you see pipelines, monitor progress, and fix problems.

When workflows are written as code, they are easier to maintain, test, version, and share with others.

### Principles
- **Dynamic**: Airflow pipelines are written in Python code. You can create pipelines automatically with code.
- **Extensible**: You can create your own operators, executors, and extend Airflow to match your needs.
- **Clear**: Airflow pipelines are simple and easy to understand. You can add parameters using the Jinja template engine.
- **Scalable**: Airflow can work with many workers and grow as your system grows.

### Important notes
Airflow is not for streaming data. Tasks do not move data between each other (they can only exchange metadata). Airflow is more like Oozie or Azkaban than Spark Streaming or Storm.

Workflows are usually static or change slowly. The task structure should look similar from one run to another. This makes it easy to understand and manage.

### Installation
#### Installing Airflow
The easiest way to install Airflow is with pip:
```bash
pip install apache-airflow
```
You can also add extra features, like gcp or postgres:
```bash
pip install apache-airflow[gcp,postgres]
```

#### Initialize the database
Airflow needs a database before running tasks. If you are just learning, use the default SQLite. For other databases, see Airflow documentation.

After setup, initialize the database:
```bash
airflow initdb
```

### DAG definition file
In Airflow, a Python script defines a DAG. It does not run tasks directly. Tasks run on different workers at different times.

The DAG file should run quickly (seconds), because the scheduler runs it often. You can use XCom to share data between tasks.

Airflow tasks are created with operators. Some common operators:
- `BashOperator` – run a bash command
- `PythonOperator` – run a Python function
- `EmailOperator` – send an email
- `SimpleHttpOperator` – send an HTTP request
- `MySqlOperator`, `PostgresOperator`, `SqliteOperator`, etc. – run SQL commands
- `Sensor` – wait for a file, database row, S3 key, or time

### Running Airflow
To run Airflow and the scheduler:
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
Airflow will create the `$AIRFLOW_HOME` folder and lay an "airflow.cfg" file with defaults that get you going fast. You can inspect the file either in `$AIRFLOW_HOME/airflow.cfg`, or through the UI in the `Admin->Configuration` menu. The PID file for the webserver will be stored in `$AIRFLOW_HOME/airflow-webserver.pid` or in `/run/airflow/webserver.pid` if started by systemd.

For the next post, I will do explain how could I define my pipeline for Covid-19 data using web scraping over Kompas news. Please stay tune!