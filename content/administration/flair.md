+++
title = "Flair Engine Admin"
description = "Configuration of Flair Subsystem"
weight = 9
+++

Flairing of user data is done by the Flair subsystem.  There is an administrative UI for the Flair subsystem.  You must log in as "flair-admin" using the password you set up at install time.

You can retrieve the flair-admin password by logging into the SCOT host system 
and running:

```
su - scot4
kubectl -n scot4 get secrets scot4-flair-secrets -o jsonpath='{.data.S4FLAIR_ADMIN_PASS}' | base64 --decode; echo ""
```


This administrative UI allows you to: 
* create, update, or delete the set of Regexes that the Flair system uses to identify Entities.
* add an API key for something other than SCOT to use the Flair engine.
* add, update, or delete administrator accounts
* view metrics about Flairing.
* Check on the status of the [Minion](https://docs.mojolicious.org/Minion/Guide) Job Queue system
* View the OpenAPI (swagger) documentation for the Flair Engine.

To access the Flair UI, you type in the URL https://<your_server_name>/flair-ui.  You will be presented with the following view:

![FlairUI](/images/FlairMainMenu.png)

Once you select one of the buttons on the bottom row, you will be asked to authenticate:

![Flair Authenticate](/images/FlairAuthenticate.png)

## Regex

Selecting "Regex" will allow you to adjust the regular expressions that Flair uses to find Entities.  Here's the table view:

![Regex Table](/images/RegexTable.png)

Click on the ID if you wish to edit or delete the Regular Expression

![Regex Detail](/images/RegexDetail.png)

### Fields

regex_id
: primary key.  do not adjust unless you know what you are doing.

name
: the short name of the entity you are trying to find.  

description
: a longer text that gives a better description of the Entity you want to find with this regex.

match
: this is the [Perl Regular Expression](https://perldoc.perl.org/perlre) that you are defining to find the match.  Perl RE's have gained capabilities over time.  Use RE's compatible with Perl 5.30.  Assume that the imxs flags are enabled for your regex.

entity_type
: this is type of entity being found.  This string will link this discovered entity with actions and pivots.

regex_type
: core = core set of regexes that come with SCOT.  udef = user defined regexes.

re_order
: Lower number regexes will be attempted before higher numbers.

multi-word
: 0 = false, 1 = true.  Multi-word matches span over white space and new lines.  If you are trying to match something that contains a space or newline, set this to true.

## ApiKeys

You may wish to use the Flair API for other things.  This allows you to set up a convenient API key to use.  

![CreateApiKey](/images/CreateApiKey.png)

### Fields

username
: username to associate with this apikey

apikey
: the apikey to use.

flairjob
: True = allow this apikey to create Flair jobs.

regex_ro
: True = allow this apikey to read the list of Regexes in the Flair Engine

regex_crud
: True = allow this apikey to create, read, update, and delete a regex.

metrics
: True = allow this apikey to read the metrics table

## Admins

This table displays the list of Flair Admins.  You may also create a new admin:

![CreateAdmin](/image/CreateFlairAdmin.png)

### Fields

username
: the username of the flair admin

who
: the full name of the user.

pwhash
: the PBKDF2 hash of the users password. E.g. {X-PBKDF2}HMACSHA2+512:.......==

## Metrics

A table of metrics collected by the Flair Engine.

### Fields

MetricId
: primary key

Year
: the 4 digit year

Month
: the number of the month.  1 = January.

Day
: the number of the day.

Hour
: the 24 hour number of the hour.

Metric
: This is the metric being collected for this YearMonthDayHour.  

Value
: The numeric value of the metric.

### Metrics

flairjobs_requested
: the number of flair jobs requested during a given hour

entry_processed
: the number of Entries processed

parsed_data_size
: number of bytes processed that hour

images_replaces
: the number of images replaced by imgmunger within flair.

entities_found
: the number of entities discovered

completed_flairjobs
: the number of flair jobs that completed successfully

elapsed_flair_time
: the number seconds spent "flairing" data that hour.

## Minion

The minion button will take you to the [Minion](https://docs.mojolicious.org/Minion/Guide) User Interface.  Here you can see real-time stats and inspect jobs and view failed job errors.

## Swagger Docs

This will take you the the OpenApi documentation for the Flair API.
