+++
title = "Next Steps"
description = "What's next after deploying SCOT 4"
weight = 4
+++

## Join Us!

Please take a moment to join the [community](/community) of SCOT users and developers.  

## Configuration

Final configuration of SCOT is performed using the Web user interface. For the purposes of this documentation we will assume that the URL of your SCOT instance is `scot4.example.com`.

## Administration Settings

The Administration setting can be reached by logging in as scot-admin.  To get the password for scot-admin, run the following command:

```
k get secret scot4-env-secrets -o jsonpath='{.data.FIRST_SUPERUSER_PASSWORD}'|base64 --decode; echo ""
```

Once logged in, you can navigate via URL to `https://scot4.example.com/#/admin` or by clicking on the User icon in the upper right of the web page and selecting "Administration"

![Administration Pulldown](/images/AdministrationPullDown.png)

### Global Settings

Site Name
: For enterprises that may have multiple SCOT instances, this is the name of this instance. In the future, could be used for site to site sharing of information.

Environment Level
: The maximum "sensitivity" level that can be stored with SCOT.  This value is displayed in the top banner of the web UI.  There is no enforcement of this sensitivity restriction and is used for display only.

Contact
: the email address of the primary owner or contact for this system.

Time Zone
: (optional) If you want to override the browser's timezone.

### Users & Groups

In this section, you can define users and roles.  SCOT permissions are based on Roles.  Users have a set of Roles that they perform.  SCOT data objects define what Roles are permitted to read, modify, or delete them.  
 
![Users Groups Page](/images/UsersGroups.png)

Users can be added manually for local authentication.  If you use LDAP or AD, and a user not previously entered into this table, successfully authenticates and the user's group set intersects with the set of SCOT Roles, that user will be added to the table automatically.  

Sliding the User Active slider to the left will block the user from logging in via any method of Authentication.

### Authentication

This section allows you define the default authentication method for SCOT.  If you define multiple Authentication options, SCOT will serially attempt authentication with each method in the order they are displayed.  Your options are:

local authentication
: user credentials are stored in SCOT and managed through the *Users and Groups* interface above.

LDAP
: Authenticate Users via LDAP.   See [LDAP Authentication](/administration/ldap.html)

AD
: Authenticate Users via Active Directory.  See [AD Authentication](/administration/ad.html)

![Authentication Panel](/images/AuthMethods.png)

#### Audit Logs

View recent transactions with the audit log.  Users can only view their own actions.  Admin users can view all transactions.

![Audit Logs](/images/AuditLogs.png)

### Object Storage

SCOT allows you to define where you will store file uploads.  The default method is via filesystem.  If you have access to an S3 compatible object storage system, you can add that provider and configure the necessary information for SCOT to utilize that storage system.

*Note*: There can only be one storage method active at a time.  Switching storage methods will break links to objects already in SCOT and would require you to use the api to find those links and update them.  At this time, SCOT does not provide an automated means to do this for you.

![Object Storage](/images/ObjectStore.png)

### Default Permissions

Default Permissions allow the admin to set a default set of Roles that are permitted to Read, Modify, and/or Delete SCOT objects.  This mainly applies to SCOT objects that are created by the system and not an individual user.  In the case of an object created by a user, that object inherits the roles from that user and is subject to further modification by the creating user.

![Default Permissions](/images/DefaultPerms.png)
