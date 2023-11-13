+++
author = "Ratnadeep Debnath"
categories = ["bind", "bluetooth", "blueZ", "gcc", "linux", "ratnadeep debnath", "rfcomm", "scan", "-lbluetooth", "rtnpro"]
date = 2009-05-08T18:19:05Z
description = ""
draft = false
slug = "working-with-some-bluetooth-coding"
tags = ["bind", "bluetooth", "blueZ", "gcc", "linux", "ratnadeep debnath", "rfcomm", "scan", "-lbluetooth", "rtnpro"]
title = "Working with some bluetooth coding :)"

+++


It’s been quite some time since I started poking with bluetooth coding, but didn’t understand it much. But it was quite some moment this afternoon. I was trying to study the rfcomm source code, and finally could able to find out the part of the code responsible for the “bind” option for rfcomm. Had a code already for scanning for nearby bluetooth devices. Edited some of theirs codes, added some lines, and came up with a code in c which scans for nearby bluetooth devices, displays them, asks the users to choose one of the devices to bind, and accordingly binds that device. But for that, the device must be already paired up with the host computer. Here’s the code:

> #include <stdio.h>  
>  #include <errno.h>  
>  #include <fcntl.h>  
>  #include <unistd.h>  
>  #include <stdlib.h>  
>  #include <string.h>  
>  #include <sys/ioctl.h>  
>  #include <sys/socket.h>  
>  #include <sys/wait.h>
> 
> #include <bluetooth/bluetooth.h>  
>  #include <bluetooth/hci.h>  
>  #include <bluetooth/hci_lib.h>  
>  #include <bluetooth/rfcomm.h>
> 
> int bt_scan (char baddr_list[][19], char name_list[][248] )  
>  {  
>  inquiry_info *ii = NULL;  
>  int max_rsp, num_rsp;  
>  int dev_id, sock, len, flags;  
>  int i, j;  
>  char addr[19] = {0};  
>  char name[248] = {0};
> 
> struct sockaddr_rc loc_addr = { 0 }, rem_addr = { 0 };  
>  char buf[1024] = {0};  
>  int s, client, bytes_read;  
>  socklen_t opt = sizeof (rem_addr);
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
>  strcpy(name_list[i], name); strcpy(baddr_list[i], addr);  
>  printf(“%d. %s %sn”, i+1, addr, name);  
>  }
> 
> free(ii);  
>  close( sock );  
>  return i;
> 
> }
> 
> static int create_dev(int ctl, int dev, uint32_t flags, bdaddr_t *bdaddr, char *strba)  
>  {  
>  struct rfcomm_dev_req req;  
>  int err;
> 
> memset(&req, 0, sizeof(req));  
>  req.dev_id = dev;  
>  req.flags = flags;  
>  bacpy(&req.src, bdaddr);
> 
> str2ba(strba, &req.dst);
> 
> req.channel = 1;
> 
> err = ioctl(ctl, RFCOMMCREATEDEV, &req);  
>  if (err == EOPNOTSUPP)  
>  fprintf(stderr, “RFCOMM TTY support not availablen”);  
>  else if (err < 0)  
>  perror(“Can’t create device”);
> 
> return err;  
>  }
> 
> int bind_bt(char *strba)  
>  {  
>  bdaddr_t bdaddr;  
>  int i, opt, ctl, dev_id, show_all = 0, err;
> 
> bacpy(&bdaddr, BDADDR_ANY);  
>  ctl = socket(AF_BLUETOOTH, SOCK_RAW, BTPROTO_RFCOMM);  
>  if (ctl < 0) {  
>  perror(“Can’t open RFCOMM control socket”);  
>  exit(1);  
>  }
> 
> dev_id = atoi (“/dev/rfcomm0″);
> 
> err = create_dev(ctl, dev_id, 0, &bdaddr, strba);  
>  close(ctl);
> 
> return err;  
>  }
> 
> int main (int argc, char *argv[])  
>  {  
>  int count, choice, err;  
>  char name_list[10][248], baddr_list [10][19], strba[19];
> 
> count = bt_scan(baddr_list, name_list);  
>  if (count > 0){  
>  x:  
>  printf(“Enter the serial number of the device that you want to connect to : “);  
>  scanf (“%d”, &choice);
> 
> if (choice > count){  
>  printf (“Enter proper choice…n”);  
>  goto x;  
>  }  
>  else{  
>  strcpy(strba, baddr_list[choice-1]);  
>  err = bind_bt(strba);  
>  if (err < 0) printf (“Might be you don’t have sufficient privileges, try running it as rootn”);  
>  }  
>  }  
>  return 0;  
>  }

To compile this code :

> gcc foo.c -lbluetooth

Run the a.out file as root because the rfcomm binding operation needs root privileges.

Any suggestions or ways to improve the code are welcome.

>

