+++
title = "Design"
description = "Notes on the design of SCOT"
weight = 3
+++

## Goals
- SCOT should require minimal training to use and understand
- SCOT should improve the effectiveness and efficiency of the IR analyst
- SCOT should reward the user for using it by helping solve tedious problems
- SCOT should be easy to maintain and resilient.
- SCOT should be available to Windows, MacOS, and Linux users.
- SCOT should not add burdens to the analyst if at all possible.

## Architecture

SCOT 4 has been designed from the start to be run in a collection of containers. We recommend Kubernetes to orchestrate this collection. 

## Tech Stack

### Database
SCOT supports using the following databases:
* MySQL
* Postgres
* SQLite
* MS-SQL 

### Web Server
* Apache 
* Uvicorn 

### Web Framework
* FastApi

### UI Framework
* VueJS

### Other Components
* Airflow
* Mojolicious / Minion

