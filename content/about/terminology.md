+++
title = "Terminology"
description = "Defining Terms used in SCOT documentation"
weight = 2
+++

Having a common vocabulary is important to improve understanding.  Many terms in cyber security are overloaded with meanings and often have different connotations to different teams.  The SCOT developers have attempted to pick terms carefully to avoid these problems but sometimes it is unavoidable.  This section provides context for common terms used through the documentation.

## Analyst
Broadly, a user of SCOT.  Someone who is performing a cyber security function such as researching threat Intel or responding to alerts.

## Incident Response
The process of triaging, investigating, and remediating events of cyber security concerns.  For example, looking at a list of alerts from an intrusion detection system to see if a compromise of security has been attempted or succeeded.

## Incident Response Team
The team of analysts responsible for handling cyber security investigation and remediation.

## (Cyber) Threat Intelligence
The process of researching and analyzing potential cyber threats and threat actors.

## Threat Intel Team
Team responsible for collecting and analyzing cyber threat intelligence.

## Alert
A notification from a detection system.  In SCOT context, typically a set of key value pairs.  Alerts can come in many ways, for example: from an IDS system, a Splunk Saved Search, or an e-mail.  The Alert is the starting point of the Incident Response workflow.  Alerts can be closed, re-opened, or promoted into Events.

## Alertgroup
A collection of Alerts.  Alerts often come in batches if you are using something like Splunk to aggregate logs and running saved searches on that data.  For analyst efficiency, it is useful to group these batches of alerts into an alertgroup where the analyst may quickly view the details in a grid and apply bulk actions.

## Event
An alert or group of related alerts that warrant further investigation.  Typically, an analyst viewing an alert, should be able to determine if there is something interesting about the alert in a few minutes time.  Promoting that alert to an Event, signals the rest of the team that there is something that requires additional attention by the team.  

Event is the where the majority of the incident response work will be recorded.  Each analyst will enrich the Event by adding Entries and providing/updating a summary.  The Event lifecycle consists researching and collecting data about the event.  A summary can be created to help bring the team up to speed quickly.  Mitigation activities should be added to the Event.  Events of a serious nature can be aggregated and promoted into an Incident.  Finally, Events are closed when no further activity is expected.  Of course, SCOT will allow re-opening of an event if closure is premature.

## Incident
A grouping of one or more Events that you wish treat as a larger event.  Maybe there is a threshold on what your have to report and when you reach that you can promote the Event(s) into an Incident for tracking or reporting.  

## Entry
An Entry holds a block of data that is associated with something in SCOT.  The data is primarily HTML text, but additional data types are planned.  Entries can be linked to Entities, Events, Incidents, Dispatches, Intels, Products, Signatures, and Guides.  HTML data within an Entry is scanned by the Flair Engine to identify Entities.  

Entries are owned by the creator of the data but may be edited by anyone with matching modify permissions.  Read and Modify sets may be a subset of the permissions from the linked SCOT Entity, Event, Incident, Dispatch, Intel, Product, Signature, or Guide.  An Entry will only be displayed to an Analyst if that analyst is within a matching role in the Read set.

Entries can be designated as a Summary and doing so moves the entry to the top of the threaded list of entries being displayed.  Entries can also be marked as Tasks.

## Task
Tasks track "To Do" items and serve as reminders of things to do and can act as requests to the team for help.  Tasks can not be assigned to others, but Analysts can take ownership of any task they can view.  This proactive model prevents many queue management problems.

## Entity
Entities are string fragments that SCOT can detect automatically within Entry data.  These can be thought of as IOC's, but can be broader as well.  The Flair engine currently finds near 20 types of string patterns and can search for user defined strings as well.  Example entity types include: IP Addresses, Domain names, Email addresses, MD5 hashes, SHA1/256 hashes, Email message-ids, and filenames with common extensions.  For complete list see Flair Engine documentation.

Once detected within an Entry, Entities are cross referenced with the entirety of SCOT's records.  Various enrichments and actions can take place on Entities depending on their Entity Class.

Detected Entities are highlighted within the Entry and are clickable to see the associations and perform additional actions.

## Flair
Flair is the highlighting and decoration of Entities.  Decorations include various icons that can give the analyst important information about the entity at a glance, such as flags for Geo IP, the number of times this entity has been seen, or that addition information about the Entity is available by clicking on it.

## Dispatch
Dispatches are "raw" pieces of threat Intel.  They are generally automatically input into SCOT by subscribing to RSS or Email feeds.  They are the threat intel equivalent of an Alert.

## Intel
Intel are promoted dispatches or other information that are pertinent and applicable to the team.  Storage of intel within SCOT allows the Flair Engine to cross reference included IOC's so that you can immediately see if an IOC is already present in your history or if it should appear in a future alert.  This linkage is very powerful way to provide the incident response analyst context.

## Product
The refined work of your threat intel team.  Maybe it is a report about a threat actor or a piece of novel malware.  These are collected here and can be shared with partners.

## Guides
Guides act as mini instruction manuals to help your analysts know how to respond to incoming Alerts.  Guides can be linked to various items in SCOT and can provide the operating procedures you want the team to perform for various alerts.

## Signatures
A Signature is a copy of a detection signature used by your detection systems.  Examples include Yara, Snort, or a Splunk search.  Having a copy of the signature is SCOT provides the following advantages: the analyst can see the search conditions without logging into the detection system, Entries with context/instructions can be attached, and, in the future, SCOT will use these records to track effectiveness and detection coverage. 

## Entity Class
Entity classes are applied to Entities and allow for various behaviors to be applied to an entity based on its class.  For example an Entity containing an IP with a Firewall Block entity class, would allow a custom icon to be displayed, instantly informing the analyst that the IP address is already being blocked.  Additionally, that class could allow for various pivots to be permitted and other automated Actions to be performed to enrich the data about the entity.

## Pivots
A pivot is a link to another system to continue the investigation on an entity class.  For example, a pivot can be constructed for IP addresses within your organization that looks up the IP in your network information system.  Pivots are steps you want to take with human intervention.

## Actions
Automated actions that fire for matching Entity classes.  Typically, these kick off Airflow DAGs while passing in the value of the Entity.  This allows for the automated enrichment of entity data, or other automated tasks.
