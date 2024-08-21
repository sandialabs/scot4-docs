+++
title = "Threat Intel Workflow"
description = "A sample workflow for Threat Intel using SCOT"
weight = 4
+++

Where IR is focused on responding and remediating cyber security events, Cyber Threat Intel (CTI) is focused on finding cyber threats to the organization and creating detections and mitigations for those threats. SCOT has a workflow to help track the activities and work product of the CTI. 


# Dispatches

Like with the IR workflow, raw intel items flow into SCOT and are stored in Dispatches.  Useful and relevant dispatches can be promoted to Intel items for further development.  See [Feeding](/administration/Feeding.html) for the ways to automate the input of raw intel from RSS feeds, mailing lists, or APIs.  Dispatches can also be manually created by the Threat Intel analyst to capture serendipitous discoveries.  

TI analysts then review the collection of Dispatches.  Not all dispatches may be relevant to your organization and may be "closed."  Other dispatches will be relevant, however, and these may be promoted to become an Intel.  These dispatches will be "flaired" which may highlight non-obvious connections to Incident Response efforts or other Intel items.  

![Dispatch Detail](/images/DispatchDetail.png)

Even if the dispatch is not currently relevant, you may wish to document additional information or research results to associate with the dispatch.  This is done by creating a series of Entries for this dispatch.  That way if the situation should change, the team will not loose any work previously done on this dispatch.

Another reason to keep "non-relevant" dispatches around is that as your situational awareness evolves, the dispatch may hold new insights for your analysis.  For example, two weeks ago you may have assumed that attempts to connect from a certain IP address were "normal lock rattling," but now that a dispatch is showing that IP is from an known APT, you can quickly go back to those seemingly benign alerts and reevaluate them with this new context.

# Intel

Intel items are the next stage in processing and allows the CTI analyst to add additional information in Entries to this item.  Here is where you can store the teams analysis and other research products regarding the Intel item.  Much like IR events, sharing your analysis as entries prevents duplicative work by the team and provides a record of the avenues explored by the TI team.  Summaries can also be created to provide an overview to management or other team members. 

![Intel Detail](/images/IntelDetail.png)


# Products

Products hold the most refined, or developed, cyber threat intelligence developed by your CTI team. Your products can take different forms, such as formal reports, threat cards, in-depth analysis of malware or exploits, or others based on need.  Entries can be attached and can be used to track who you have shared your product with, commentary from the team, and other metadata for the product.

Examples of products could be your kill-chain analysis, graphics highlighting connections, your diamond model analysis, or a collection of TTPs that identify a threat actor.

# Entities Link All

Entities are the thread that tie many items together in SCOT.  The Entity list allow the analyst to search and view all Entities within SCOT and jump directly to where they are found.  Often, in makes more sense to store some Threat Intel information directly attached to the Entity, instead of as a separate Intel or Product.  

For example, if a domain name is highly associated with a threat actor, you may decide to include that information as an entry attached to the domain name.  This way, when an IR team member sees that domain in an alert, they can click on the domain name and instantly see that association.

The Flair Engine is the primary source for Entities, but it you can also have external sources of IOC's use the API to insert Entities within the SCOT database.  The benefit of this is that when the flair engine detects these IOC's in new alerts or entries, they will already be linked and possibly contain enrichments.  

# Feeds

SCOT has the ability to consume RSS feeds and input those articles into dispatches.  Under the Threat top-level menu, select "Feeds" and you can create, edit, or disable a set of RSS URI's that SCOT will pull from.


