+++
title = "Creating Enrichments"
description = "Enrich entity data using SCOT with Airflow"
weight = 3
+++

## SCOT enrichments
SCOT entity enrichments provide the capability to incorporate data enrichment or other actions on flaired entities. These actions occur automatically when an entity is flaired. Actions can be anything from gathering data associated with the entity for display within the entity's flair pane in SCOT, or other types of action that can be taken outside of SCOT. SCOT works with the open-source workflow management platform, **Apache Airflow**, to launch actions based on the entity's type. 

## The enrichment workflow
- In the SCOT API configuration file, specify the following required fields:
    - enrichmentApiJobEndpoint (must be in this format WITH PLACEHOLDER INCLUDED): "/api/v1/dags/scot_entity_[ENTITY_TYPE_PLACEHOLDER]_enrichment/dagRuns"
    - enrichmentHost (your deployed airflow instance url): "https://airflow.example.com"
    - enrichmentUsername: "scot4-enrichment-account-name"
    - enrichmentPassword: "scot4-enrichment-account-password"
    - enrichmentTypes (semicolon-delimited list of entity types): "ipaddr;domain;email;etc"
- In Airflow, jobs are defined as Python scripts called a DAG. Each DAG you create corresponds to the workflow of enrichment(s) you want performed on a given entity type. 
- when an entity is newly created and flaired, SCOT automatically checks the entity type and, if it matches any of the types specified in the enrichment configuration, it will send a request to your Airflow server with the entity information and a callback URL to capture results
- Airflow uses the endpoint specified in the request (scot4_entity_[ENTITY]_enrichment) to match against the names of the DAGs you've created (NOTE: the name you give your created DAGs must match this same scheme) and will trigger the DAG.
- Once the DAG completes, Airflow will make a POST request back to SCOT's /entity/enrichment endpoint, including any collected data as directed by the DAG (exmaple below).
- SCOT will add the enrichment data to the entity.
- In SCOT, you can click on the flaired entity to open the flair pane. In the flair pane, you will see the enrichment name(s) appear as tabs. Click on each enrichment tab to see the latest enrichment data for this entity. From this view you can also view previous enrichment data results, and now send a new enrichment request for potentially updated data.

## Deploying Apache Airflow
Directions for deploying an Airflow instance can be found at https://airflow.apache.org/

## Creating a new DAG in Airflow
You can review the docs for making DAG workflows at https://airflow.apache.org/docs/apache-airflow/stable/index.html.
SCOT enrichments are designed such that one DAG corresponds to one entity type. Within that one DAG, you can have one or more tasks (python functions with an @Task decorator) defined for that enrichment type, for example:

A DAG to enrich an IPv4 entity can have:
1. A task to collect enrichment data from an external 3rd party source
2. A task to collect enrichment data from an internal source
3. A task to add the IP to an internal blocklist if the enrichment data meets certain criteria
4. A task that returns new entity class IDs to the /entity/{entity_id}/entity_class endpoint in SCOT based on enrichment data

You are limited only by what you can do programmatically in python and using APIs

## Creating your first DAG
Below is a simple DAG workflow you can use as a starting place. It's made for enrichment of an IPv4 address (but could be modified for any entity type). Based on how you set up your Airflow instance and define your DAG endpoints, you should name this script to match the endpoint. In this case, where we define the endpoint (see workflow above) as `/api/v1/dags/scot_entity_[ENTITY_TYPE_PLACEHOLDER]_enrichment/dagRuns`, and assuming you have an entity type named `ipaddr`, your DAG should be named: `scot_entity_ipaddr_enrichment`


### MaxMind DB
This enrichment takes a given IPv4 and performs a lookup in a database file from MaxMind that can be stored locally on the same server where your Airflow instance is running. You can create an account under MaxMind's GeoLite2 service (https://www.maxmind.com/en/geolite2/signup) which will allow access to their db files which you can down load and use. In this particular example, the database file we expect to use is 'GeoLite2-City.mmdb'. Once you have this file, place it somewhere on your airlow server and note the file path for later.


### Sample DAG code

See the Airflow documentation for creating a new DAG. Copy the following code and save as described in the section above "Creating your first DAG"

```
from airflow.utils.task_group import TaskGroup
import json
import pendulum
from airflow.decorators import task, dag, task_group
from airflow.models import Variable
from airflow.operators.python import get_current_context
from airflow.models.log import Log
from airflow.utils.db import create_session
from airflow.timetables.trigger import CronTriggerTimetable
import logging
import os 

@dag(
    timetable=None, ## Add your schedule interval here, any valid cron notation (https://en.wikipedia.org/wiki/Cron) works here. Use CronTriggerTimeTable like this example: CronTriggerTimetable("0 */4 * * *", timezone="UTC") 
    start_date=pendulum.datetime(2021, 1, 1, tz="UTC"),
    catchup=False,
    tags=['author:example', 'example_from_template'],
    doc_md=open(f"{os.path.dirname(os.path.realpath(__file__))}/README.md").read(),
    ## params is where you can add other parameters to each triggered DAG's configuration
    params={'entity_id': None, 'entity_value': '192.168.1.1', 'db_path': None, 'callback_url':None} 
)

def scot4_simple_example():
    @task
    def enrichment_task():
        import pandas as pd
        # Use logger to get create info/debug/error messages for the DAG instead of the print function
        logger = logging.getLogger(__name__) 
        # get the IP address from the Airflow context
        context = get_current_context()
        ip_addr = context['params'].get('entity_value')
        # query enrichment data for IP from 3rd party/API service
        enr_data = get_enr_data(ip_addr)
        logger.debug(f'enr_data results: {enr_data}')
        # convert any results to markdown so SCOT can parse it
        markdown = pd.DataFrame.from_records([enr_data]).T.to_markdown()
        # use this schema for SCOT parsing (title value = tab name in the flair pane)
        enrichment_data = { 'title': 'Geo IP Summary',
                            'enrichment_class': 'markdown',
                            'description': 'enrichment via MaxMind direct query',
                            'data': {'markdown': markdown,}
                            }
        return {'enrichment_data': [enrichment_data]}
    
    
    def get_enr_data(ip_addr):
        import geoip2.database
        logger = logging.getLogger(__name__) # use for debugging
        context = get_current_context()
        ## This should be the file path to the GeoLite2-City.mmdb file on your airflow server
        db_path = context['params'].get('db_path')
        reader = geoip2.database.Reader(db_path)
        try:
            response = reader.city(ip_addr)
            # logger.debug(f'response content: {response}') # uncomment for debugging
            # use response.raw if running an older version of geoip2
            result = response.to_dict
            # result = response.raw
            reader.close()
            return result
        except:
            logger.error(f'failed to perform lookup')
            reader.close()
            # Throw an error and Airflow will stop and return nothing
            raise Exception(f"failed to read enrichment data")
            ## you could optionally return an error to populate in the IP's enrichment pane of the flair modal 
            # return {"data": "unable to find IP in database"}

        
    @task
    def add_enrichment_to_scot(results):
        import requests
        context = get_current_context()
        callback_url = context['params'].get('callback_url')
        api_key = None
        ## Airflow can store secrets, use that to store an API key for your SCOT instance and retrieve it using Variable.get()
        ## if you have a second/multiple SCOT instances you can store multiple API keys and retrieve the correct one based on which callback URL is being given
        if callback_url is not None and "scot4-prod" in callback_url:
            api_key = Variable.get('scot4-api-key')
        elif callback_url is not None and "scot4-test" in callback_url: 
            api_key = Variable.get('scot4-test-api-key')
            
        if callback_url is not None and results.get('enrichment_data') is not None and len(results['enrichment_data']) > 0:
            for enrichment_data in results['enrichment_data']:
                context = get_current_context()
                entity_id = context['params'].get('entity_id')
                url = f"{callback_url}/entity/{entity_id}/enrichment"
                res = requests.post(url, data=json.dumps(enrichment_data), headers={
                    'Content-Type':'application/json',
                    'Authorization': f'apikey {api_key}',
                })
                if not res.ok:
                    raise Exception(f"Request to {url} failed: {res.status_code} {res.reason}: {res.text}")
                print(f"Request to {url} succeeded: {res.status_code} {res.reason}: {res.text}")


    ## This is where we define the flow of the DAG. 
    ## See https://airflow.apache.org/docs/apache-airflow/stable/concepts/dags.html 
    ## for more information about defining the control flow of DAGs.

    task_results = enrichment_task()
    add_enrichment_to_scot(task_results)


dag = scot4_simple_example()
```


### Testing your DAG
See the Airflow documentation for how to trigger a DAG from the Airflow UI. When you tigger a DAG, you're asked to supply values for the params that are specified in the code. You should already have the entity you want to test with registered/flaired in SCOT so that it has an entity ID.

- 'entity_id': The ID of the entity you're testing
- 'entity_value': The value 0f the entity (try the IP for maxmind.com)
- 'db_path': /the/file/path/on/your/airflow/server/to/GeoLite2-City.mmdb 
- 'callback_url': the full URL to your SCOT API as defined in your SCOT-API config settings (ex. https://scot4-test.domain.com/api/v1)

Once you enter these fields and run your DAG, you'll see the progress and whether the perations completed successfully (green) or not (red). If the DAG failed, click on the red box indicating the portion of the DAG that failed and then select the log tab on the right to check for any errors. See the Airflow docs for details.
