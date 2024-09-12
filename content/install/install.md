+++
title = "Install"
description = "Install SCOT 4"
weight = 2
+++

## Easy Mode


### Create scot4 User

You must create the scot4 user prior to cloning the repository.

```
useradd -m -s /bin/bash -c "SCOT4 User" scot4
```

### Clone Repository

Clone the SCOT 4 repository as the scot4 user.

```
su - scot4
git clone https://github.com/sandialabs/scot4.git
```

### Run Helper

The repository contains a helper script to automate most of the remaining tasks.

```
su - scot4
cd scot4
./install.sh
```

### install.sh options

The install.sh program will query you for any information it needs, or has sensible
defaults.  However, you may wish to provide that information on the command line.  
Here are the options supported:

```
-b REPLICAS         set the number of API server replicas to create. (Kubernetes)
-c TLS_CRT_FILE     the fully qualified filename for your TLS certificate File
-d SQLALCHEMY_URI   set the SQLALCHEMY_DATABASE_URI necessary to connect to your external
                        database. You do not have to set this if you are using the 
                        default provided mysql database container.
-e SURGE            set the surge limit for the API server. (Kubernetes)
-g                  pause script after displaying values of the script variables
-h VERSION          set the version of Helm to download and install.  defaults to 
                        version 3.14.3
-i IPADDR           set the IP address that the API server will listen to.  Defaults to
                        the first result of "ip -4 -o addr show scope global"
-k TLS_KEY_FILE     the fully qualified filename for your TLS KEY File
-n NO_PROXY         the values to use for your NO_PROXY environment
-P HTTPS_PROXY      the value to use for HTTPS_PROXY
-p HTTP_PROXY       the value to use for HTTP_PROXY
-r REPOSERVER       the container registry hostname:port
-s SERVERNAME       sets the server name for this SCOT instance, usually scot4
-x REG_SECRET       set the pull secret for the Container Registry
-y REG_SECRET_NAME  set the name for the pull secret
```

### Option Notes

* Replica count defaults to 1.  This is fine to kick the tires by a single user or two.  If you are deploying this for use by multiple users, increase this value higher.  A replica count of 8 can easily handle up to 30 concurrent users.
* Setting a SQLALCHEMY_DATABASE_URI using -d option, informs the installer that you wish to use an existing database running somewhere else.  See SQLAlchemy docs for specifics on the URI format.  If you do not set this, the installer will bring up a pod that runs a mysql database and use that.  Data will persist in the database, but it is recommended to have DBA support to ensure correct backup and that the DB is configured to handle the expected load.
* PROXY settings: If your SCOT instance runs behind a proxy to the internet, you can configure the appropriate values this way.  For SCOT on stand alone networks with no internet access, you can omit these settings.
* Container registry, registry secret and registry secret name are advanced options for those who wish to build their own containers/pods from the sources.  These settings allow you to point to an alternate registry to obtain the containers/pods.  Do not specify these options if you wish to use containers provided by the SCOT team.

### TLS Note

* Leaving TLS_CRT_FILE and TLS_KEY_FILE blank tells the installer that you wish to install SCOT with self-signed certificates for testing purposes.  The installer will guide you through the creation of a self-signed certificate for your tests.

* To run SCOT in production mode, you must have a valid certificate for the URL that you are wanting to server SCOT from.  For example, if you want to type into your browser https://scot4.widgets.com to access scot, you must provide the crt and key files for scot4.widgets.com.  Production mode turns off certain debugging log messages and uses a secure auth cookie. It is not possible to run SCOT in production mode with self-signed certificates.


### Monitor Progress

Once the install program has completed, it will take a few minutes for the containers to download and spin up.  You can monitor progress with the following command:

```
 watch kubectl -n scot4 get pods
```

You will see the pods go through various init stages.  When the display looks like:

```
Every 2.0s: kubectl -n scot4 get pods                   dev24su: Tue Sep 10 14:41:32 2024

NAME                              READY   STATUS      RESTARTS   AGE
scot4-api-9c4c58b67-j786h         1/1     Running     0          20m
scot4-api-9c4c58b67-th4v4         1/1     Running     0          20m
scot4-db-5494bb968c-7dj7s         1/1     Running     0          20m
scot4-flair-795488b57d-jcjh2      2/2     Running     0          20m
scot4-frontend-6f4cb87f57-cdpc7   1/1     Running     0          20m
scot4-search-0                    1/1     Running     0          20m
scot4-search-init-7llfw           0/1     Completed   0          20m

```

you can <ctrl-C> the watch program and begin to use SCOT.

### Should there be an Error

If a pod is displaying a backoff error condition, you can get more details about what is causing the problem by using the command:

```
kubectl -n scot4 describe pod <pod-name-here> 
```

The most likely error would be some kind of failure to pull the container from the repository.  Make sure you are not having a network issue and retry the install once network issue has been resolved.
