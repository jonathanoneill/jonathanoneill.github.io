---
layout: post
title:  "Vodafone Mobile Broadband Tuning"
date:   2009-12-01 00:00:00
categories: [Internet]
tags:
comments: true
---

I have been using the Vodafone Mobile Broadband Stick (Huawei K3765) and noticed differences in performance (high disk and processor use) on my laptop when I was using it compared to being attached to a local network. Turning down the logging level and preventing the Mobile Connect software from starting at logon significantly improved the performance.

**1. Turn down logging**

Logging can be turned down by editing MobileConnect.exe.config and Settings.xml in C:\Program Files\Vodafone\Vodafone Mobile Connect\Bin\ as follows.

*MobileConnect.exe.config*

![](/assets/blog/vodafone-mobile-broadband-tuning/mobileconnect-exe-config.png "MobileConnect.exe.config")

*Settings.xml*

![](/assets/blog/vodafone-mobile-broadband-tuning/settings-xml.png "Settings.xml")

**2.  Don't start the Mobile Connect software at logon**

Use <a href="http://technet.microsoft.com/en-us/sysinternals/bb963902.aspx">Autoruns</a> to prevent Mobile Connect from starting at logon. It will start from the menu when you want to use the modem.

![](/assets/blog/vodafone-mobile-broadband-tuning/autorunsmobileconnect.png)
