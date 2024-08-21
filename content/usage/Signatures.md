+++
title = "Signatures "
description = "What are Signatures?"
weight = 5
+++

This is one of SCOT's newest features.  Signatures are a place to store a copy of any detection signatures your team might be using.  The expectation is that you will have some form of automated feed from your detections systems, git  or other method to push updates to SCOT.

Once in SCOT, you now have a place to provide contextual and historical data for the development of the signature.  This can be the starting point for detection engineering.  Your team can document the reasons for the signature, what it is attempting to detect, and weaknesses of that signature.  

In the near future, SCOT will introduce a way to link Alerts to the Signature that triggered it.  This will allow for the collection of metrics on the effectiveness of the signature.  Signatures with high false positives  or low confidence can be identified and refined.  Another future feature, will be the ability to map a signature onto a framework like ATT&CK, to provide coverable metrics for your detection systems.



