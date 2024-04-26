---
title: "Jingle Bell HackTheBox Sherlock Writeup"
summary: "Explore Windows artifact analysis focusing on notification artifacts in this Jingle Bell HackTheBox Sherlock writeup."
tags: ["HackTheBox", "Sherlock", "DFIR", "Windows Artifacts", "Notification Artifacts"]
author: "Katana"
description: "Explore Windows artifact analysis focusing on notification artifacts in this Jingle Bell HackTheBox Sherlock writeup."
canonicalURL: "https://h3xkatana.github.io/Blog/post/Jingle-Bell/"
---

## Introduction

In this Sherlock writeup, we delve into analyzing Windows artifacts, specifically focusing on notification artifacts. All notifications in Windows are stored in a SQLite database within a notification table, typically in XML format.

![Notification Artifact](https://github.com/H3xKatana/Blog/blob/main/content/post/3/1.png?raw=true)

To facilitate analysis, you can use the following Python script to import the database file as a CSV file or utilize any online SQLite viewer.

```python
# Python script for exporting Windows 10 notification data to CSV
# Usage: python WPNtoCSV.py inputDB outputCSV

import sqlite3
import csv
import sys

def generateCSV(csr, outputfilename):
    csr.execute("""SELECT n.'Order', n.Id, n.Type, nh.PrimaryId AS HandlerPrimaryId, nh.CreatedTime AS HandlerCreatedTime, nh.ModifiedTime AS HandlerModifiedTime, n.Payload,
                          CASE WHEN n.ExpiryTime != 0 THEN datetime((n.ExpiryTime/10000000)-11644473600, 'unixepoch') ELSE n.ExpiryTime END AS ExpiryTime,
                          CASE WHEN n.ArrivalTime != 0 THEN datetime((n.ArrivalTime/10000000)-11644473600, 'unixepoch') ELSE n.ArrivalTime END AS ArrivalTime
                   FROM Notification n
                   INNER JOIN NotificationHandler nh ON n.HandlerID = nh.RecordID""")
    result = csr.fetchall()
    with open(outputfilename, "w", newline='', encoding="utf-8") as f:
        writer = csv.writer(f, delimiter='\t')
        writer.writerow(list(map(lambda x: x[0], csr.description)))
        for line in result:
            lst = list(line)
            lst[-2] = "" if lst[-2] == 0 else lst[-2]
            lst[-1] = "" if lst[-1] == 0 else lst[-1]
            writer.writerow(lst)

def printMetainfo(csr):
    csr.execute("SELECT * FROM Metadata")
    result = csr.fetchall()
    print("""\n            
        Notifications - Metadata
        ------------------------
        """)
    for line in result:
        print("\t" + line[0] + ":  " + str(line[1]))
    print()

if __name__ == "__main__":
    if len(sys.argv) == 3:
        conn = sqlite3.connect(sys.argv[1])
        csr = conn.cursor()
        printMetainfo(csr)
        generateCSV(csr, sys.argv[2])
    else:
        print("""\n            
        Windows 10 Notifications to CSV 
        -------------------------------
        This script processes the wpndatabase.db notifications database from Windows 10 
        and gives a truncated, tab-delimited file as output.
        File location:  %APPDATA%\Local\Microsoft\Windows\\Notifications\wpndatabase.db
            
        Usage: 
        WPNtoCSV.py   inputDB   outputCSV 
            
        Example:
        WPNtoCSV.py   wpndatabase.db  notifications.csv
        """)

```
![Notification Artifact](https://github.com/H3xKatana/Blog/blob/main/content/post/3/2.png?raw=true)


## Task Analysis

1. **Identify Sender:** Analyze the URL to identify the sender of the notification.

2. **Title Examination:** Examine the titles of the notifications.

3. **Message Sender Identification:** Determine who is sending the messages.

4. **Room Number Analysis:** Investigate the significance of room numbers in the notifications.

5. **Password Detection:** Detect potentially easy-to-find passwords within the messages.

6. **Drive URL Analysis:** Examine plain URLs related to drives.

7. **Timestamp Conversion:** Utilize the provided script to convert timestamps for message delivery.


```python
 import datetime

# Define the timestamp value from the message
timestamp_str = "1681986889.660179"

# Convert the timestamp string to a float (assuming it's in seconds since Unix epoch)
timestamp_seconds = float(timestamp_str)

# Convert the timestamp to a datetime object in UTC timezone
utc_datetime = datetime.datetime.utcfromtimestamp(timestamp_seconds)

# Print the UTC datetime in a readable format
print("Message Delivered at (UTC):", utc_datetime)
```


8. **Monetary Value Assessment:** Analyze monetary values mentioned, e.g., "10000".
