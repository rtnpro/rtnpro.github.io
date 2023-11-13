+++
author = "Ratnadeep Debnath"
categories = ["blueZ", "devel", "Fedora", "rtnpro"]
date = 2009-04-06T18:15:18Z
description = ""
draft = false
slug = "rtnpro-starts-his-bluez-journey"
tags = ["blueZ", "devel", "Fedora", "rtnpro"]
title = "rtnpro starts his blueZ journey..."

+++


Just now … I wrote my first ever blueZ code, a simple one though, that too, from an online  tutorial at

> http://people.csail.mit.edu/albert/bluez-intro/c404.html

Its a nice tutorial for newbies like me to get started with blueZ.

First of all, I needed to setup my blueZ development environment. Since I use Fedora 10, I did the following

> yum install -y bluez-libs bluez-libs-devel libgdbus libgdbus-devel

Also, one can manually install it from the source packages available at

> www.bluez.org

The first code that I wrote is a simplescan.c which scans for bluetooth devices in proximity and displays its bluetooth address ( a 48-bit address unique for all bluetooth devices managed by IEEE). Here’s the following code :

> #include <stdio.h>  
>  #include <stdlib.h>  
>  #include <unistd.h>  
>  #include <sys/socket.h>  
>  #include <bluetooth/bluetooth.h>  
>  #include <bluetooth/hci.h>  
>  #include <bluetooth/hci_lib.h>
> 
> int main(int argc, char **argv)  
>  {  
>  inquiry_info *ii = NULL;  
>  int max_rsp, num_rsp;  
>  int dev_id, sock, len, flags;  
>  int i;  
>  char addr[19] = {0};  
>  char name[248] = {0};
> 
> dev_id = hci_get_route(NULL);  
>  sock = hci_open_dev( dev_id);  
>  if (dev_id < 0 || sock < 0){  
>  perror(“opening socket”);  
>  exit(1);  
>  }
> 
> len = 8;  
>  max_rsp = 255;  
>  flags = IREQ_CACHE_FLUSH;  
>  ii = (inquiry_info*)malloc(max_rsp * sizeof(inquiry_info));
> 
> num_rsp = hci_inquiry(dev_id, len, max_rsp, NULL, &ii, flags);  
>  if( num_rsp < 0 ) perror(“hci_inquiry”);
> 
> for (i = 0; i < num_rsp; i++) {  
>  ba2str(&(ii+i)->bdaddr, addr);  
>  memset(name, 0, sizeof(name));  
>  if (hci_read_remote_name(sock, &(ii+i)->bdaddr, sizeof(name), name, 0) < 0)  
>  strcpy(name, “[unknown]”);  
>  printf(“%s %sn”, addr, name);  
>  }  
>  free(ii);  
>  close( sock );  
>  return 0;  
>  }

Then to compile the code :

> [rtnpro@xps blueZ]$ gcc -o simplescan simplescan.c -lbluetooth

Then to execute it :

> [rtnpro@xps blueZ]$ ./simplescan
> 
> 00:1D:98:78:A2:A1 Nokia 5310 XpressMusic

See the output, its my Nokia mobile phone.

