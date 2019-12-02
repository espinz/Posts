---
layout: post
title:  "Windows Server 2003-2019 serial-USB possible?"
date: 2019-03-14 
categories: [os, windows]
tags: [hardware, powershell, serial, usb]
---

In the Debian server project, I used a serial to USB to make a headless connection.

Great for disastrous --- situations; Or when I have go searching for a keyboard, monitor, and mouse. YUCK!
I thought to myself. Is this possible in Windows?

Take a Windows server, crash it badly such as a BSOD or not, and connect to it from a client via serial?  

<!-- more -->
### Tools Used
* RS-232 M (Male) to USB (Male)
* null modem M/F (Male/Female) 

### On Windows Server 
enter this on command line with Admin rights (Serial Side)  
`bcdedit /bootems {default} ON`  
`bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`  

Make sure Special Administrator Console is on Automatic in Services. (mine was off and kept shutting off when I tried to run it.) 
And famously reboot.

### On client (USB Side)
I wouldn't chmod 777; chown on the device or use it as root.  
Or add a group for that device.

This was used for testing.

`sudo chmod 777 /dev/ttyUSB0`

`screen cu -l /dev/ttyUSB0 -s 115200`


All the power{shell} of Windows.

Is it useful in 2019 or in Windows Server 2019? 

I'm not sure but it works on Window Server 2016. 


