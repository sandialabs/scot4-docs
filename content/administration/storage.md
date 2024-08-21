+++
title = "Object Storage"
description = "Configuration of Object Storage"
weight = 8
+++

By default, you will start with Local File System as your object storage system.  It is important to note that you may only have one object storage system active at a time, and that changing this setting after files have been uploaded to SCOT is difficult.  

If you already have an S3 compatible object storage system and wish to keep uploaded files in that system, initial install time is the best time to enable this.

The following page will detail the configuration options available for each type.

## Local File System

![Local Storage Config](/images/LocalStorageConf.png)

The following options are available:

Provider Name
: If you wish to change the provider name for some reason

Base Directory
: The base directory to store uploaded files.  Default is `/var/scot_files`

Deleted Items Directory
: The directory to move deleted uploaded files.  Default is  `/var/scot_files/_deleted_items` This enables a rudimentary "undelete."  Files that are "purged" are truly gone.  

## S3

## DellEMC

![DellEMC storage](/images/DellEMC.png)

Api Access Key
: Enter the key here

Storage Account Endpoint
: Enter the endpoint for your DellEMC system

Storage Account Endpoint Port
: Enter the listening port 

API Secret Access Key
: Enter your secret access key

No Proxy Env variable
: configure any hosts that should not go through web proxy

HTTP Proxy Env variable
: configure a proxy if necessary

Base Bucket Path
: Path to your Base Bucket

EMC Namespace
: Put your namespace here.

Provider Name
: Configure the name of the provider

Deleted Items Bucket
: the bucket to hold deleted items
