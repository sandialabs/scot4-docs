+++
title = "LDAP Authentication"
description = "How to get SCOT to Authenticate using LDAP"
weight = 5
+++

LDAP Authentication is set up by an admin user by going to the Administration Settings->Authentication panel. 

![Ldap Settings](/images/LdapSettings.png)

Setting up LDAP for authentication can be very tricky, and it is recommended that you work with your local LDAP administrator to set the following settings.

Server
: the FQDN of the LDAP Server

Bind User
: the user with permission to bind to LDAP and perform queries

Bind Password
: password for the Bind User

Test User
: ?

Test Group
: ?

Group Base DN
: the DN for retrieving a set of groups.  E.g. `ou=groups,ou=orgname1,dc=dcname1,dc=dcname2`

Group Filter
: LDAP group lists can get rather large and when the total length of that list exceeds 1k characters, some LDAP implementations truncate the list.  By applying a filter, you can help ensure that the group list is under that limit.  We recommend picking a prefix to attach to all the groups you want to uses with SCOT.  So if you pick a prefix like wg-scot, all your SCOT related groups would look like: wg-scot-response, wg-scot-mgmt, etc.  The corresponding group filter would be `(| (cn=wg-scot*))`

User Base DN
: The DN for looking up a user's groups.  E.g. `ou=accounts,ou=ouname,dc=dcname1,dc=dcname2`

User Filter
: E.g. `uid=%s`

Provider Name
: ?

Auto-create Groups
: Check this box if you want SCOT to detect new groups with your prefix and to automatically create a corresponding Role.

Username Attribute
: ?

Group Name Attribute
: ?

User Email Attribute
: ?

User Group Attribute
: ? 

Group Member Attribute
: ?

User Full Name Attribute
: ? 
