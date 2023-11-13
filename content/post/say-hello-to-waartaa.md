+++
author = "Ratnadeep Debnath"
categories = ["waartaa", "irc", "rtnpro"]
date = 2013-12-10T06:30:00Z
description = "Waartaa is an Open Source communication and collaboration tool build on top of IRC"
draft = false
slug = "say-hello-to-waartaa"
tags = ["waartaa", "irc", "rtnpro"]
title = "Say hello to Waartaa"

+++


### What is Waartaa?

**Waartaa** or **wārtā** is a word in Hindi: **वार्ता**, which means **to communicate**. And that's what *waartaa* is for. *Waartaa* is a web based **IRC** client as a service and it facilitates centralized logging, idling functionality, unique identification across multiple clients and a rich UI for awesome user experience. *Waartaa* is open sourced under *MIT License*. The source is at [https://github.com/waartaa/waartaa/](https://github.com/waartaa/waartaa)
.You can download, fork, customize and setup *Waartaa* as a service anywhere, be it a single user laptop/desktop, server for your self and your friends.

![](/content/images/2015/02/waartaa-1.png)


##### Why?
There are an arsenal of IRC clients and tools, so why another one?

Waartaa is not just a random fun project, although it has been fun and full of adventures to work on it. Below are a few reasons why I started to work on Waartaa:

* GUI IRC clients work only for single machines. It's a pain to sync logs across multiple devices across multiple IRC clients.
* Local IRC clients do not let you *idle* when you are not online. It's not possible for everyone to get a server to setup ZNC or similar idling server.
* Most desktop IRC clients do not have that WOW! look and feel.
* Lack of identity when logged in from multiple devices simultaneously.
* Local network configuration (proxy, firewall) and quality (speed, timeout) often becomes a hindrance to good IRC experience.

Waartaa solves the above issues as follows:

* Running IRC client as a service on better infrastructure ensures that you are always connected to IRC and capturing IRC logs.
* Waartaa serves as a central place to store your chat logs.
* No matter what device you login from to Waartaa, YOU are always YOU in the IRC and not some YOU\_, YOU\_\_, etc.
* Waartaa is built on top of web technologies. So, it works flawlessly and uniformly across multiple platforms and looks equally awesome in all the them. This adds up to a superior user experience.

---

### Features

##### Beautiful and useful chat interface
![](/content/images/2015/02/waartaa_chat_logs.png)
![](/content/images/2015/02/waartaa_nick_options.png)

###### Easy to join server/channel
![](/content/images/2015/02/waartaa_add_server.png)
![](/content/images/2015/02/waartaa_channel_join.png)

##### Stylish menus
![](/content/images/2015/02/waartaa_server_menu.png)
![](/content/images/2015/02/waartaa_channel_menu.png)
![](/content/images/2015/02/waartaa_channel_nick_menu.png)


##### Informative
![](/content/images/2015/02/waartaa_channel_connecting.png)
![](/content/images/2015/02/waartaa_channel_unread_msg_count.png)

##### Under the hood

1. **Meteor JS** [http://www.meteor.com/](http://www.meteor.com/)
2. **MongoDB**
3. Forked **node-irc** [https://github.com/waartaa/node-irc](https://github.com/waartaa/node-irc)
4. And a host of meteorite apps from [https://atmosphere.meteor.com/](https://atmosphere.meteor.com/)


##### Where can I try it?

[http://www.waartaa.com/](http://www.waartaa.com)

##### How can I contribute?

* Fork the code from [https://github.com/waartaa/waartaa](https://github.com/waartaa/waartaa)
* Setup as instructed in `README`
* Start using it
* Report any issue at [https://github.com/waartaa/waartaa/issues/new](https://github.com/waartaa/waartaa/issues/new)
* Pick up issues to fix from [https://github.com/waartaa/waartaa/issues](https://github.com/waartaa/waartaa/issues)

