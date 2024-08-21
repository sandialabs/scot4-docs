+++
title = "Security Orchestration Automation Response (SOAR)"
description = "How to get SCOT to SOAR"
weight = 8
+++

SOAR stands for Security Orchestration, Automation, and Response.  It's a collection of capabilities that allow a team to define automation to perform when certain conditions merit.  SOAR can help your team be more effective by eliminating tasks that are repetitive and time consuming.

The developers of SCOT decided to use [Apache Airflow](https://airflow.apache.org) to help provide this capability.  You can set up your own instance of Airflow and use it to enrich Entities as they are discovered, query internal systems or logs, kick off jobs, alter defensive system configurations, or just about anything else you can think of doing via a python program.  There are also registries of shared DAGs that may allow you to use plugins without developing your own.  

In this section, we will show how to configure SCOT to work with Airflow and provide some example integrations that may spark additional ideas for your environment.  

## Install Airflow

Several options exist for installing Airflow.  Please see [Airflow Install Docs](https://airflow.apache.org/docs/apache-airflow/stable/installation/index.html) for instructions.

