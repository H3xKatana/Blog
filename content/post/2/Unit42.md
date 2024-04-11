---
title: "Unit42 writeup"
summary: "Unit42 htb sherlocks writeup"
# weight: 1
# aliases: ["/first"]
tags: ["Post","hackthebox","sherlocks","DFIR"]
author: "Katana"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Unit42 hack the box  sherlock writeup"
canonifyURLs : true
canonicalURL: "https://h3xkatana.github.io/Blog/post/unit42/"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
#ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "/img/" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

INTRO:
In this Sherlock, you will familiarize yourself with Sysmon logs and various useful EventIDs for identifying and analyzing malicious activities on a Windows system. Palo Alto's Unit42 recently conducted research on an UltraVNC campaign, wherein attackers utilized a backdoored version of UltraVNC to maintain access to systems. This lab is inspired by that campaign and guides participants through the initial access stage of the campaign.


![your picture](https://github.com/H3xKatana/Blog/blob/main/content/post/2/1.png?raw=true)

We have a file containing Windows event logs that capture suspicious activity.
These logs are accompanied by well-defined rules for detection.

Upon filtering, we discovered that there are 56 occurrences of Event ID 11, 
which is highly suspicious because this ID typically signifies file creation events.

Event ID 1 provides detailed records of processes, including their names, 
hashes, and parent paths. This information can be instrumental in identifying malware.

Event ID 22 presents domain records, 
indicating that the file was downloaded from Dropbox, which raises suspicion.

Event ID 2 indicates a process altering file creation times, a behavior that is often indicative of malicious activity.

For Task 5, the instruction is to search for Event ID 11 alongside the presence of "once.cmd",
suggesting a specific detection scenario.

Task 6 involves utilizing Event ID 22 to uncover domain names associated with potentially malicious activities.

Task 7 entails leveraging Event ID 3, which typically indicates network connections, 
to identify suspicious connections, particularly those involving TCP connections to suspicious IP addresses.

Finally, Task 8 involves using Event ID 5 to compile a list of terminated processes, 
which could provide insights into potentially malicious activities that have been halted.

In summary, by analyzing specific event IDs and their associated details, 
we can effectively detect and investigate suspicious activities within the Windows event logs.