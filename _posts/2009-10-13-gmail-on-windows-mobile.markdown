---
title:  "Gmail on Windows Mobile"
date:   2009-10-13 00:00:00
categories: [Google,Windows Mobile]
tags:
comments: true
---
I have been using a phone running Windows Mobile 6 for some time using a POP email client and  Microsoft ActiveSync running on my PC to backup contacts and calendar. While this solution worked it had several limitations:

* Email relied on the POP client polling the server
* Sent items remained in the phones sent items (unless you cc yourself)
* Calendar and contact backup relied on phone being connected to the PC

In September Google added [push Gmail suport](http://googlemobile.blogspot.com/2009/09/google-sync-now-with-push-gmail-support.html "Google Sync: Now with push Gmail support") to [Google Sync](http://m.google.com/sync "Google Sync") which allows email, contact and calender to be syncronised between Gmail and Windows Mobile using the Microsoft Exchange ActiveSync protocol.

The set-up was straight forward:

* Make sure you have a backup of contacts,  calendar and email on the phone as the initial synch will delete from the phone.
* Followed the step by step instructions [Set Up Your Windows Mobile Phone](http://www.google.com/support/mobile/bin/answer.py?answer=138636&amp;topic=14299 "Google Sync: Set Up Your Windows Mobile Phone")
* Exported contacts from Outlook to CSV and used the import feature for Gmail contacts (if they are not already in Gmail).

This has been running for a few weeks now and I have only had a couple of issues:

* When contact are synchronised they appear as "Last Name, First Name" on the phone even though the full name is in a single field on the Gmail contact i.e. "First Name Last Name". This is OK for actual peoples names but it also does it for any name with a space e.g. "ACME Ltd" appears on the phone as "Ltd, ACME".
* It worked away for a few days and then the email stopped synchronising. Having checked the forum I found that others had the same problem when the "Message format" on the phones ActiveSync settings was set to "HTML". By setting this to "Plain Text" it has been working OK since.  It is only the occasional email that is only in HTML format and cannot be read and this is usually ezines.
* By default only the Inbox is synchronised so if you want to see any other folders on the phone (including sent items) you need to set it up on the the phones Outlook (Folders &gt; Manage Folders).
* You may also need to set the "Download the past" on the phones ActiveSync settings to e.g. "1 month". The default on my phone was "3 days".

Overall this has proved to be a good solution where the phone and mail, calendar and contact have stayed in synchronised. It also opens up the possibility of a simple upgrade to a new phone in the future that Google will synchronise with but only time will tell.
