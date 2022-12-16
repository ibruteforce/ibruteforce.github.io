---
layout: post
title: Investigation Scenario - Anaylsis #1 
subtitle: 
tags: [Investigation, Analysis]
comments: true
---

As I've made a change from red team to blue team, I've been exploring different investigation scenarios and thought I'll go ahead and blog about one of the Chris Sander's challenge. Below is the challenge brief. 


"TeamViewer_Desktop.exe executed on a system in your network.

What do you look for to start investigating this event? 

Assume you have access to any evidence source you want."



## Initial Analysis

There seem to be a lot of ways that different analyst could start their investigation, however below are some of the first steps that I'd take if I were to start my investigation.

1. Check if the file is actually from TeamViewer or if its a random file that is just named teamviewer.exe.

2. If the file isn't from TeamViewer -- further analysis required to check if they are malicious.

3. If the file is indeed from TeamViewer, to check with System Owner and Organization policies that if such remote administration tool is allowed in the organization.

4. Location of the file that has been executed. 

From the comments, it seems like a few has suggested to Quarantine / Isolate the machine straightaway, However I don't necessarily think that is a good idea, since these could be legitimate binaries run with a purpose.

## Further Anaylsis






