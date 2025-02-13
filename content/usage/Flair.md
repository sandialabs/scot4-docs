+++
title = "Flair and Entities"
description = "What are Flair and Entities?"
weight = 3
+++

![SCOT-Flair-Logo](/images/flair-page-logo.png)

One of SCOT's unique features is the ability to automatically highlight and cross-reference key pieces of data within submissions to SCOT.  Flair refers to the highlighting and the addition of icons following that data. (The process that does the highlighting is also called "flair").  Entities are the pieces of data that are being highlighted. Entities can be thought of a collection of IOC (indicators of compromise) but are not limited to just IOC's.

## What's it really doing?

The Flair engine searches strings (HTML and plain text) with a set of regular expressions.  When one matches, it wraps that match in a HTML span that allows the front-end to recognize that text as an Entity.  The Flair engine also updates SCOT's database of Entities and their locations.  

At display time, the front-end queries the SCOT database for icons (flair items) to place after the span.  Upon clicking on that span, SCOT will display all the locations that the entity has previously been found, as well as additional enrichment data that has been stored for the entity.  

## Core Entities

The core set of entities that the flair process detects include:

* CVE references
* Browser User Agent strings (experimental)
* CIDR blocks
* IPv6 Addresses
* IPv4 Addresses (with and without common obfuscation)
* MD5, SHA1, and SHA256 Hashes
* E-mail message ids
* Domain Names (with and without common obfuscation)
* Filenames with common extensions
* UUIDs
* Clsids
* E-mail addresses
* WinRegistry keys
* jarm_hash
* Country names (American English versions)
* Internal Scot Links
* SIDs

See [Core Entity Descriptions](/usage/core_entity.md) for more information about the core entities.

## User Defined Entities

In addition to the Core set, SCOT4 users can add "User Defined" Entities based on string matches.  While viewing an Entry, a user can click and highlight any string within the Entry.  A pop-up window will then ask the user to confirm creation of user defined entity and ask what [Entity Class](/about/terminology/#entity-class) to assign to this item.  

Note: Choose your user defined entities carefully.  5 minutes after introducing this feature someone highlighted the word "the" (user defined flair has a 3 character minimum) and created an entity from it.  This dramatically slowed the system while it dutifully began finding all instances of "the" in the data and flaired them.  Cleanup was not fun. (contact [scot4developers@sandia.gov](mailto:scot4developers@sandia.gov) for details)  SCOT follows the Unix philosophy of giving you enough rope to shoot yourself in the foot. Consider yourself warned!

## Entity Icons

Following the text of the Entity, SCOT adds various icons that can provide quick information to the analyst.  Most commonly, the icon immediately following the text is a circle with a number in it.  The number represents the number of times this Entity appears in SCOT.  Icons after the number vary depending on the Entity Class and the results of Enrichments applied to the Entity.  For example, an IP address could contain a flag icon of the country of its GeoIP origin.  Another icon could be applied that signifies it's presence on a blocklist of some sort.

## Entity Modal

Clicking within the highlighted region of the Entity will bring up the Entity Modal.  

![Entity Modal](/images/EntityModal.png)

In this modal, we can interact with the Entity directly.  First, some entities may be total noise, like for example www.google.com, and you can instruct SCOT to not track them by selecting the "tracked" pull down and changing it to "untracked."  

Working our way down, we see that the Entity Type is "ipaddr" and the Entity value is "x7.236.20.13"  When this Entity was created, an enrichment process (see [SOAR](/usage/soar) queried the MaxMind GeoIP database and assigned the flag of the country of origin.  EntityClass to this IP.  Next we can apply additional Entity Classes and Tags to this Entity.  Entity classes will be discussed later, but it is a way to label the Entity so SCOT can know what [pivots](/about/terminology/#pivots) to display and what [actions](/about/terminology/#actions) to take.

Next is a list of pivot buttons that is based on the Entity Type and Entity Class.  Clicking on one of these buttons will open a new browser tab and initiate a query of that web service with the IP address pre-filled out.  See [Pivot Admin](/administration/pivots) for directions on creating and managing the set of pivots.

The "Recent Appearances" table allows you to see where else this IP address appeared and hyperlink to those appearances.  Next to "Recent Appearances" are variable tabs, that display additional information about this IP address that have come from automated Actions that stored their enriched data with the Entity.  In this case we can see additional GeoIP data for this IP.

Finally, we see a series of Entries.  These Entries can be from Actions or from your team's manual input.  It is very useful to store the results of your research about this Entity here because it will be available to all users whenever this entity is encountered.

