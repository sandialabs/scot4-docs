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

### Disk Partitioning
If you partition your disk space, it is important to allocate a large /var partition because that is where all persistent storage will reside.

There are several factors that will impact how much disk space you will need. Primarily, disk usage is driven by the amount of data being input into SCOT.  Also, configuring SCOT to use an external database server will greatly reduce your long term storage needs.  Likewise, if you use S3 to store uploaded files, storage needs can be reduced.  

In the table below, we characterize the minimum space needed based on usage patterns and assuming that you will collocate you database and file storage on this system.   When in doubt, give /var as much space as you can afford.

| Usage            | GB of Disk | Notes |
| Proof of Concept | 128 | Little automated input of data, few users |
| Light Activity   | 256 | Automated feeds of 10 to 100 items a day, User input of 0 - 100 items a day  |
| Medium Activity  | 512 | Automated feeds of 100-500 items a day, User input of 100 - 300 items a day |
| Heavy Activity   | 1TB | Automated feeds of 500+ items a day, User input 300+ items a day |

In the table above, automated items refer to Alertgroup and Dispatch items created and any other automated input of data you might do via API.  User input is primary the creation of Entries.  

Keep in mind, that your usage patterns may allow you to use less or require more space.  If at possible, configure you system so that you may adjust the /var partition to meet your needs as they change with your usage.  When planning also consider that SCOT does not do any data reduction, so usage will grow over time.  In other words, data you input into SCOT will stay there unless you manually delete it.

## Kubernetes

We use [k3s](https://k3s.io) as our Kubernetes orchestrater, and [Helm](https://helm.sh) to define and manage the application.  

## Database

SCOT4 requires a database to operate.  SCOT4 supports the same databases as [SQLAlchemy](https://sqlalchemy.org).  We have tested SCOT4 with [PostgreSQL](https://postgresql.org), [MySQL](https://mysql.com), and [SQLite](https://sqlite.org).  We have had reports of success with [Microsoft SQL Server](https://microsoft.com/en-us/sql-server/sql-server-downloads) as well.

SCOT comes with a mysql container that can be used or can be configured to use an existing database in your infrastructure.  If you choose to use and existing database, be sure to have your database installed, configured, and to have credentials and permissions set to allow the creation of a database.  You will need to know the value of the `SQLALCHEMY_DATABASE_URI` environment variable for your database. (See [SQLAlchemy Docs](https://docs.sqlalchemy.org/en/20/core/engines.html#backend-specific-urls) for details)

## Object Storage

SCOT4 stores uploaded files in a local filesystem or using an object storage system compatible with S3, like [MinIO](https://min.io).  If you wish to use a system like MinIO or Amazon S3, you will need to know the access and secret keys for the system when you install and configure SCOT4.

## TLS

You will need to have a .crt and .key files for your SSL/TLS configuration.  Using a self-signed certificate is possible but strongly discouraged because use of a self-signed certificate will prevent SCOT from running in production mode.

## IP Address

You will need to know the IP address of your system to install and configure SCOT4.
