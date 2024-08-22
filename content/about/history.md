+++
title = "History"
description = "How SCOT came to be"
weight = 4
+++


Over a decade ago, our cyber security team team was organized in small silos of expertise.  We had a IDS group, a forensics group, etc.  Recognizing that this structure was hampering our efforts to keep up with the rising quantity and complexity of cyber threats, we reorganized into a unified response team.  This presented a new challenge, how to keep this team in sync, prevent re-work, and be able to document our work.

Like many teams, we began exploring various existing solutions.  Ticketing systems were an obvious first choice, but the elliptical nature of our investigations did not lend itself to the break-fix ticket system paradigm.  Several members liked the free form nature of OneNote, but how do you share or coordinate work on private notebooks?  Wiki systems could be shared, but lacked work coordination features we required.  Workflow management systems required elaborate configuration for anything other than a linear process flow.  It became obvious to me that to solve the team's need, I would have to create a solution that would take the best of each of these areas and combine them to form an Incident Response Management system.

So after a few months of hacking, I created SCOT 1.0.  It allowed users to create "Events" and attach free form "Entries" to those events.  It did near real-time updating to all users logged into the system, that way, if someone posted the results of an inquiry, the team had immediate access to that data without refreshing the browser.  This was the first killer feature.  Now, the team could benefit from each others work on an event and stay in sync, thus preventing expensive re-work.

The team rapidly adopted SCOT 1.0 and I soon was receiving frequent visits to my office from our team with feature requests.  "Can we combine and promote Events into Incidents for reporting?"  "Can SCOT receive a stream of high fidelity alerts from our detection systems?"  "Can we promote these alerts to Events?"  

SCOT rapidly progressed to 3.0, when it received it next killer feature, Flair.  Flair would parse the data input into SCOT and find common IOC's and other "Entities."  After finding these Entities, flair would find other occurrences within the SCOT data and create links between them.  Now the team would no longer have to remember an IP address or repeatedly look it up.  The "flaired" IP address would carry enrichment data such as GEOIP with it and with a couple of links analysts could see if that IP address had every been seen by us before and more importantly, what was done in that previous case.

Around this time, we decided that we should release SCOT as an open-source project.  After much paperwork, SCOT was published to GitHub and we began to see other organizations adopt SCOT. 

Next SCOT began to integrate Cyber Threat Intelligence data.  Initially, threat intel was stored in Entries but that became difficult to find and differentiate from the Incident response data.  SCOT started with another "tab" called "Intel" to hold this data.   Over the next few years, a parallel structure to IR developed with Dispatches, Intel, and Products available to our team.

In recognition of the importance of SCOT to the cyber security efforts to Sandia, our management decided dedicate additional resources to modernize SCOT.  The SCOT4 team took a green field approach and choose the technology stack carefully.  After a year of development, SCOT 4.0 was released to the team in September 2023.  We are excited to continue to build off this revamped architecture.

I'd like to personally thank and recognize the amazing engineers that I have had the privilege to work with while creating SCOT.

### Scot 3 Engineers
* Nick Peterson
* Nick Georgieff
* Bryce Montoya
* Javier Chavez

### Scot 4 Engineers
* Greg Walkup
* Blake Moss
* Joey Dickinson
* Matt Van Veldhuizen
* Brandon Thomas
* Randy Jaramillo
* Michael Symonds
* Tyler Lubin

-- Todd Bruner


