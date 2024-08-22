+++
title = "Migration From SCOT 3"
description = "Bringing forward your SCOT3 data"
weight = 5
+++

If you are upgrading from SCOT 3 and wish to bring forward your data to SCOT 4, these are the steps to take.

## Backup SCOT 3

Step one is the backup your SCOT3 instance.  The best way to do this is to run the backup script.  

```
sudo /opt/scot/bin/backup.pl
```

This will create a tar file in the /opt/scotbackup directory.  The filename in the format of `scotback.YYYMMDDhhmm.tgz`. 

## Move Backup

Copy the backup file to the host of your new SCOT4 cluster.  Extract the backup into the home directory of scot4 user as the scot4 user.

```
$ whoami
scot4
$ pwd
/home/scot4
$ ls scotback*
scotbackup202406061213.tgz
$ tar xzvf scotbackup202406061213.tgz
```

## Spin up Migration PVC

instructions go here

```
kubectl apply -f migration_spec.yaml
```

## Copy /home/scot4/opt to PVC

```
cp -r /home/scot4/opt/* /path/to/pvc
```

## Run migration Pod

```
kubectl apply -f migration.yaml
```

## Clean Up

```
kubectl delete pvc <name of pvc>
```


