---
layout: post
title:  "VirtualBox"
date:   2010-03-18 00:00:00
categories: [VirtualBox,Virtualization]
tags:
comments: true
---

<p>I have been using <a href="http://www.virtualbox.org/">VirtialBox</a> as an alternative to WMware Workstation for the last few months and it has proved to be a great success. My use of virtual machines is mainly for development and testing purposes so my evaluation is with that in mind. I have been using Windows XP and Mac OS X as the host operating system with Windows XP, CentOS and Ubuntu as guests.</p>
<p>There have been several releases of VirtualBox in the last year which have including enhanced support for VMware .vmdk virtual disk files. With support for .vmdk disks moving from VMware to VirtualBox on either Windows or OS X is as simple as:</p>
<ul>
<li>Mount the .vmdk disk using the "Virtual Media Manager"</li>
<li>Create a new Virtual Machine with the approprtate settings using the disk</li>
</ul>
<p>While VMware Workstation is an excellent product needing to buy a Windows version and a Mac version doubles the cost and using it on additional machines needs additional licenses.</p>
<p>VirtualBox is available in two editions:</p>
<ul>
<li> VirtualBox Open Source Edition (OSE) - This addition is available as source code you can build yourself under the GPL licence.</li>
</ul>
<ul>
<li> VirtualBox - This edition adds RDP and USB support to the OSE version and is available as a easy to install binary distribution. This is released under a Personal Use and Evaluation License (PUEL). The license is explained <a href="http://www.virtualbox.org/wiki/Licensing_FAQ">here</a> and includes:</li>
</ul>
<p><em>"It doesn't matter whether you just use it for fun or run your multi-million euro business with it. Also, if you install it on your work PC at some large company, this is still personal use. However, if you are an administrator and want to deploy it to the 500 desktops in your company, this would no longer qualify as personal use."</em></p>
<p>Summary:</p>
<ul>
<li> VirtualBox is a good alternative to VMware workstation with similar functionality.</li>
<li>Moving an existing virtual machine from VMware to VirtialBox is a simple process.</li>
<li>The PUEL license covers most development and testing scenarios for multiple users in an organisation at no cost.</li>
</ul>
