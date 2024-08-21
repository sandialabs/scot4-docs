+++
title = "Feeding SCOT"
description = "Getting data to flow into SCOT"
weight = 1
+++

The first step in using SCOT is automating the input of alert and intel data.

## Alerts

There are two ways to get alert data into SCOT.  The first is to use the REST API to create new alertgroups.  See the swagger documentation for details, but generally this is as simple as posting a JSON object containing alert data to the /api/alertgroup end point.

The other way is to use the SCOT Inbox processor to monitor an IMAP or MSGraph inbox.  The inbox processor will parse new HTML email and create an Alertgroup based on the data within.  Currently, SCOT comes with two parsers: a Splunk saved alert parser, and a generic alert.  The Splunk saved alert parser will pull out the data in the HTML tables sent by Splunk and create an Alertgroup consisting of an Alert per row in the table.  The generic alert processor just copies the HTML text into the Alertgroup.  Additional parsers are possible, please contact the SCOT4 development team to discuss.

## Dispatches

Similarly, Dispatches can be created via API or dedicated E-mail inbox.  In addition, Dispatches can be input from an RSS feed.  To define an RSS Feed, go to Threat->Feeds.

## Configuration

Configuration of the Inbox Processor is done through setting of environment variable in the SCOT4 helm chart for the Inbox Processor.  The following list describes the options available.

S4INBOX_IMAP_SERVERNAME
: The hostname of the IMAP server. Set this if you are accessing an IMAP server.

S4INBOX_IMAP_PORT          
: The port that the IMAP server listens to.  A number is expected.
  
S4INBOX_IMAP_INBOX         
:  The name of the inbox, typically "INBOX".
  
S4INBOX_IMAP_USERNAME      
:  The username of the inbox owner.
  
S4INBOX_IMAP_SSL_VERIFY    
:  0 to disable SSL verification, 1 to require it.  Allows you to work around self signed certificates.
  
S4INBOX_IMAP_PEEK          
:  0 marks messages read, 1 leaves them unread.  Useful for testing, but typically you will want to mark messages read to prevent re-input into the alertgroup or dispatch queue
  
S4INBOX_GRAPH_LOGIN_URL    
:  The URL to log into MS GRAPH with.
  
S4INBOX_GRAPH_GRAPH_URL    
:  The graph URL itself
  
S4INBOX_GRAPH_SCOPE        
:  The graph's scope
  
S4INBOX_GRAPH_TENET_ID     
:  The tenet id
  
S4INBOX_GRAPH_CLIENT_ID    
:  The client id
  
S4INBOX_GRAPH_USERADDRESS  
:  The mailbox address
  
S4INBOX_PERMITTED_SENDERS  
:  Semi-colon ';' separated string listing permitted senders.  The inbox processor will reject e-mails not from members of this list.
  
S4INBOX_LOG_LEVEL          
:  TRACE, DEBUG, INFO, WARN, ERROR.  Controls the detail in the inbox processor logs.  TRACE is the most verbose and ERROR will only contain errors.  
  
S4INBOX_LOG_FILE           
:  Fully qualified Filename to append logs to.  
  
S4INBOX_SCOT_API_INSECURE_SSL 
:  0 disables SSL verification, 1 to require it.  Allows you to work if you have a self-signed certificate for the SCOT API server.
  
S4INBOX_SCOT_API_URI_ROOT     
:  The prefix for api uri, e.g. https://s4.sandia.gov/api/v1.  replace s4.sandia.gov with your hostname.
  
S4INBOX_MSV_FILTER_DEFINITIONS
:  the filename holding the MSV filter definitions. If you are using Mandiant Security Validation (Verodin), these filter definitions allow you selectively prevent MSV alerts from appearing in the Alergroup queue.
  
S4INBOX_MSV_DBM_FILE          
:  The filename of the DBM file for MSV deduplication.  Inbox Process must keep track of E-mail message-Id's to prevent the duplicate input.
  
S4INBOX_SCOT_INPUT_QUEUE      
:  Alertgroup, Event, or Dispatch. Each container running The Inbox Processor can enter data to the listed Queue.  Set this to the queue for each container.
  
S4INBOX_MAIL_CLIENT_CLASS     
:  Scot::Inbox::Imap or Scot::Inbox::MSGraph, depending on the e-mail server you are using.
  
S4INBOX_TEST_MODE             
:  Read inbox regardless of "unread" state and do not change read flags
  

### SECRETS

The following secrets are passed in via Env Var.


S4INBOX_IMAP_PASSWORD      
:  The users password
  
S4INBOX_GRAPH_CLIENT_SECRET 
:  The password
  
S4INBOX_SCOT_API_KEY          
:  The api key for the SCOT api server
  
