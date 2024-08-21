+++
title = 'Requirements'
description = 'Requirements to run SCOT4'
weight = 1
+++

SCOT4 runs as a collection of container-ized applications.  As such, it can run on a virtualized or bare-metal hardware.  

## Size

Sizing the resources of your installation depends on several factors:

* Expected number of concurrent users
* Database location (on same system or remote)
* Object Storage (on same system, remote, or local filesystem)

Assuming that you select to use an external database, and either an external object store or a local filesystem, the following table will provide an estimate of system configurations.  We recommend testing with your local usage patterns to appropriately size you system.

| Concurrent Users | CPU Cores | RAM |
| ---------------- | --------  | --- |
| up to 4          | 4         | 16 GB  |
| 4 to 8           | 8         | 16 GB  |
| 8 to 32          | 8         | 32 GB  |
| 32+              | 16        | 64 GB  |

## Kubernetes

We use [k3s](https://k3s.io) as our Kubernetes orchestrater, and [Helm](https://helm.sh) to define and manage the application.  

## Database

SCOT4 requires a database to operate.  SCOT4 supports the same databases as [SQLAlchemy](https://sqlalchemy.org).  We have tested SCOT4 with [PostgreSQL](https://postgresql.org), [MySQL](https://mysql.com), and [SQLite](https://sqlite.org).  We have had reports of success with [Microsoft SQL Server](https://microsoft.com/en-us/sql-server/sql-server-downloads) as well.

Prior to installing SCOT, be sure to have your database installed, configured, and to have credentials and permissions set to allow the creation of a database.  You will need to know the value of the `SQLALCHEMY_DATABASE_URI` environment variable. (See [SQLAlchemy Docs](https://docs.sqlalchemy.org/en/20/core/engines.html#backend-specific-urls) for details)

## Object Storage

SCOT4 stores uploaded files in a local filesystem or using an object storage system compatible with S3, like [MinIO](https://min.io).  If you wish to use a system like MinIO or Amazon S3, you will need to know the access and secret keys for the system when you install and configure SCOT4.

## TLS

You will need to have a .crt and .key files for your SSL/TLS configuration.  Using a self-signed certificate is possible but strongly discouraged because use of a self-signed certificate will prevent SCOT from running in production mode.

## IP Address

You will need to know the IP address of your system to install and configure SCOT4.
