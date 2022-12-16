---
layout: post
title: Investigation Scenario - Anaylsis #1 
subtitle: 
tags: [Investigation, Analysis]
comments: true
---

Recently I've begin to make a change from red team to blue team, I've been exploring different investigation scenarios and thought I'll go ahead and blog about one of the Chris Sander's challenge. Below is the challenge brief. 


"TeamViewer_Desktop.exe executed on a system in your network.

What do you look for to start investigating this event? 

Assume you have access to any evidence source you want."



## Initial Analysis

There seem to be a lot of ways that different analyst could start their investigation, however below are some of the first steps that I'd take if I were to start my investigation.

1. Check if the file is actually from TeamViewer or if its a random file that is just named teamviewer.exe.

2. If the file isn't from TeamViewer -- further analysis required to check if they are malicious.

3. If the file is indeed from TeamViewer, to check with System Owner and Organization policies that if such remote administration tool is allowed in the organization.

4. Location of the file that has been executed. 

From the comments, it seems like a few has suggested to Quarantine / Isolate the machine straightaway, However I don't necessarily think that is a good idea without getting answers to specific questions, since these could be legitimate binaries running with a purpose.

## Further Anaylsis

Depending on the initial analysis, more questions can be asked. Such as, checking for FileHash in VirusTotal and other sites to check if the file is malicious. However below are some of the additional analysis that could be useful in this scenario:- (A lot of these are taken from the comments)

1. Checking for the origin of the file (Where it came from)
2. Searching EDR / SIEM to see if this binary was run on any other systems.
3. Checking for Network Connection originating from EXE.
4. Checking EDR for the hash of the file, and searching on VirusTotal and other sources for info about the hash.

## Summary

If TeamViewer is something that is allowed within the organization and if the file has been downloaded from teamviewer, i.e it is confirmed that is indeed a TeamViewer file, then we can go ahead with closure of this incident.

However, if the results are otherwise, i.e origin is from a shady source, file isn't from teamviewer and hash returns to be malicious, this has to be escalated to the incident reponse team asap.
