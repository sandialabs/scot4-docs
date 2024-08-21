+++
title = "Tasks "
description = "What are Tasks?"
weight = 6
+++

Scot has the ability to mark an Entry as a Task.  This is useful to place a reminder to do something and can be used anywhere you can attach Entries.  When displayed, an Entry marked as a Task is displayed with a special color header bar.  Red signifies an open task, and green signifies a closed task.  Tasks are initially owned by the creating user.

Tasks are very lightweight and do not provide the ability to assign to others, do not have deadlines, and do not provide reminders or other workflow features.  All tasks can be viewed by anyone that has read permissions to them.  The idea is that if you want to help, would will select an open task and take ownership of it, thus signalling that you will work on it.  You can attache the results of your work as a child Entry to the task and close the task when finished.

Example Scenario:  The incident handler has promoted an alert to an Event.  This appears to be a serious issue and several things need to be investigated as soon as possible.  The incident handler or another analyst can create several tasks such as "investigate firewall logs", "notify system owner", and "collect forensic image."  Other team members will see the open tasks and voluntarily take ownership of the task they can help with.  This achieves an organic distribution of work and team coordination.

In future releases of SCOT, we are planning a feature called "Checklists."  A Checklist is a predefined set of tasks that can be rapidly applied to an Entriable class with SCOT (Events, Intel, etc.)  Initially, the application of a checklist will be performed manually, with an eventual mechanism to automatically apply checklists based on criteria to follow. 


