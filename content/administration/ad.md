+++
title = "Active Directory Authentication"
description = "How to get SCOT to Authenticate using AD"
weight = 6
+++

SCOT can use AzureAD to authenticate users.  

![AD Auth](/images/ADAuth.png)

To configure this type of authentication fill in the following fields:

Scopes
: The scopes to request when logging in (optional)

Authority
: The authority used to log in to Azure; this is usually in the format https://login.microsoftonline.com/<tenant_id> or https://login.microsoftonline.com/

ClientID
: The client ID of your application in Azure Active Directory

Access Roles
: If set, users will be required to have the given application role in order to log in

Callback URL
: The callback url configured for your SCOT instance, this should usually be the base of the SCOT GUI, e.g. https://your-scot-instance.com/

Client Secret
: A client secret configured for your application in Azure Active Directory

Provider Name
: A name that identifies this authentication instance

Auto-create Groups
: When set, SCOT will auto-create roles matching the names of all configured application roles when a user logs in


Un-Email Usernames
: Whether or not to attempt to convert the username provided by Azure out of email address format (dropping the portion after the @)


Certificate Authority Bundle
: When set to a path, the given certificate bundle is used instead of the default certificate bundle. Useful if the network is behind an intercepting proxy.



