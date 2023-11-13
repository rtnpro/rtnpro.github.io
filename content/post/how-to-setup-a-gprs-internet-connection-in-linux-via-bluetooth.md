+++
author = "Ratnadeep Debnath"
categories = ["bluetooth", "Fedora", "hcitool scan", "internet", "modem connection", "ratnadeep", "rfcomm", "system-config-network", "rtnpro"]
date = 2009-04-29T16:58:29Z
description = ""
draft = false
slug = "how-to-setup-a-gprs-internet-connection-in-linux-via-bluetooth"
tags = ["bluetooth", "Fedora", "hcitool scan", "internet", "modem connection", "ratnadeep", "rfcomm", "system-config-network", "rtnpro"]
title = "How to setup a GPRS internet connection in Linux via bluetooth ..."

+++


Often I hear questions like this “How do I connect to the internet from linux via my bluetooth mobile phone GPRS?”

Yes, there are ways to do that, but telling a newbie to go to the terminal and do hcitool scan, rfcomm, etc. spooks him out. And also The system-config-network, while setting up a Modem Connection through it, one has to com across port numbers, baud rate, etc. and a user not used to Linux, most of the time finds these things weird, especially the users. These issues need to be looked into and some actions are required to come up with a more user friendly interface in this regard. That’s another topic of discussion though …

Let’s see how to connect to the internet via GPRS over a phone with a Bluetooth Modem :

1. Use the bluetooth-applet to setup your phone if it is not yet paired with your PC. Alternatively you can do this via the terminal

> $ bluetooth-applet

and then follow the on – screen instructions.

2. Once done setting up your phone, do the following in the terminal to know your mobile phone’s bluetooth address

> $ hcitool scan

In my system, it shows something like this

> Scanning …  
>  00:1D:98:78:A2:A1    Nokia 5310 XpressMusic  
>  00:1D:98:78:A2:A1 is the bluetooth address of my mobile phone. Yours will be of similar type.

3. Now as root, do the following

> #rfcomm connect rfcomm0 <bluetooth address> 1

Keep this process running

4. Open System->Administration->Network, select New, then select Modem Connection, and enter the following values for the respective field

> Modem Device:    /dev/rfcomm0
> 
> Baud Rate: 460800

this is the safe limit for most Nokia phones, may vary for different phones.

Then click Forward. Then fill in the requisite details, then click Forward, and accept the default settings, like automatically obtaining IP Address and DNS Information from provider. Then select Apply. And then click on File-> Save to save your new settings.

Then click the Activate button for the modem connection just set up, and enjoy.

4. Every time, you want to connect to the Internet, before activating the connection via Network, you have to connect your phone as a bluetooth modem as in step 3 .

Hope to see a new GUI dedicated to setting up bluetooth internet connection in Linux soon.

