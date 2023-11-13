+++
author = "Ratnadeep Debnath"
categories = ["dgplug", "Fedora", "hello world", "kernel", "kernel-devel", "kmod", "ldd3", "linux", "Linux device drivers", "ratnadeep debnath", "yum", "rtnpro"]
date = 2010-01-29T10:49:39Z
description = ""
draft = false
slug = "an-introduction-to-linux-device-drivers-1"
tags = ["dgplug", "Fedora", "hello world", "kernel", "kernel-devel", "kmod", "ldd3", "linux", "Linux device drivers", "ratnadeep debnath", "yum", "rtnpro"]
title = "An introduction to Linux Device Drivers - #1"

+++


Today we’ll be discussing a basic c code, the famous hello.c, which will be loaded as a kernel module and discuss some of the basic aspects related to it.

> #include <linux/init.h>
> 
> #include <linux/module.h>
> 
> MODULE_LICENSE (“GPL”); // included in module.h, tells about the license the code is having.
> 
> static int __init hello_init (void)
> 
> {
> 
> printk (KERN_ALERT “Hello, worldn”);
> 
> return 0;
> 
> }
> 
> static void __exit hello_exit (void)
> 
> {
> 
> printk (KERN_ALERT “Goodbyen”);
> 
> }
> 
> module_init (hello_init);     // hello_init is the initialization function
> 
> module_exit (hello_exit);     //hello_exit is the exit function

The header* <linux/init.h>* contain various declarations and definitions related to the loading and cleaning up of modules. The macros module_init and module_exit are declared in the <linux/init.h>. The argument functions passed to ***module_init( )*** and ***module_exit( )*** are executed at the loading and unloading times respectively of a module. The initialization function, basically, sets up the device to be used later. The exit function cleans up the device ( opposite to initialization).

Now coming to the*** __init*** and ***__exit*** terms used in the code. The __init in static int __init hello_init (void) specifies that the hello_init function is executed only at module load time. The module loader drops the initialization function for other uses once the module is loaded. The initialization function better be static since they are not meant to be visible outside the specific file ( not mandatory though). Similar explanation goes for __exit. This makes the exit function executed only at exit time.

The printk function, at a first glance, might look identical to printf of <stdio.h>, but printk lets you set priority levels for the message, like KERN_ALERT, KERN_INFO, etc. We’ll discuss about them later.

How to compile the code and execute it. Let us assume that you named the source file above as hello.c. Use any suitable text editor to write a ***Makefile*** as below :

> obj-m    += hello.o
> 
> all:  
>  make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules
> 
> clean:  
>  make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean  
>  rm -f *~

I hope you have the kernel source in your system. If not you can get the kernel source from kernel.org. If you are using Fedora, you can do:

*# yum install kernel-devel*

To compile, in the current directory ( where there is hello.c and the Makefile) do :

*$ make*

Then change to super user by : $*su* <enter>

and do : *# insmod ./hello.ko*

If you do : *# tail -f /var/log/messages* (You can open this in a separate terminal to view the system logs)

then you will be able to see a line saying “Hello, world”. This is what your hello_init( ) was supposed to do at module load time. You can also see the module name “hello” among the list of loaded modules doing lsmod. Now you can unload the module do :

*# rmmod hello*

Now in /var/log/messages, you will see “Goodbye”. This is due to hello_exit( ).

continued …

