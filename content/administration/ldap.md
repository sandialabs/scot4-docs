+++
title = "LDAP Authentication"
description = "How to get SCOT to Authenticate using LDAP"
weight = 5
+++

LDAP Authentication is set up by an admin user by going to the Administration Settings->Authentication panel. 

![Ldap Settings](/images/LdapSettings.png)

Setting up LDAP for authentication can be very tricky, and it is recommended that you work with your local LDAP administrator to set the following settings.

Server
: The address of the LDAP server

Bind User
: the user with permission to bind to LDAP and perform queries

Bind Password
: password for the Bind User

Test User
: A user that can be looked up to test user queries

Test Group
: A group that can be looked up to test group queries

Group Base DN
: the DN for retrieving a set of groups.  E.g. `ou=groups,ou=orgname1,dc=dcname1,dc=dcname2`

Group Filter
: LDAP group lists can get rather large and when the total length of that list exceeds 1k characters, some LDAP implementations truncate the list.  By applying a filter, you can help ensure that the group list is under that limit.  We recommend picking a prefix to attach to all the groups you want to uses with SCOT.  So if you pick a prefix like wg-scot, all your SCOT related groups would look like: wg-scot-response, wg-scot-mgmt, etc.  The corresponding group filter would be `(| (cn=wg-scot*))`

User Base DN
: The base search domain for users

User Filter
: The filter to use when searching for users

Provider Name
: A name that identifies this authentication instance

Auto-create Groups
: When set, all discovered ldap groups will be created as roles in SCOT

Username Attribute
: The ldap attribute containing a user's username (default: "uid")

Group Name Attribute
: The ldap attribute containing the group's name (default: "cn")

User Email Attribute
: The ldap attribute containing a user's email address

User Group Attribute
: The ldap attribute on a user containing that user's group memberships

Group Member Attribute
: The ldap attribute on a group containing that group's users

User Full Name Attribute
: The ldap attribute containing a user's full nam
