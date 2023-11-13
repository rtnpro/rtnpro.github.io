+++
author = "Ratnadeep Debnath"
categories = ["waartaa", "irc", "planet", "ircb", "bouncer", "python3", "asyncio", "rtnpro"]
date = 2015-12-20T09:45:37Z
description = ""
draft = false
slug = "ircb-0-1-released"
tags = ["waartaa", "irc", "planet", "ircb", "bouncer", "python3", "asyncio", "rtnpro"]
title = "ircb 0.1 released!"

+++


**[ircb](https://github.com/waartaa/ircb)** gets it's initial **[0.1](https://github.com/waartaa/ircb/releases/tag/0.1)** release!

**ircb** is a versatile IRC Bouncer, made for scale. It was born out of our sheer requirements for a better IRC bouncer when hosting the demo instance of **[waartaa](http://www.waartaa.com)**. A bouncer that:

- scales to multiple hosts, to overcome connection limitations put by IRC servers on a single host
- accepts multiple simultaneous client connections for the same IRC connection
- A sane API to control it
- has awesome dashboard and analytics, built in

We plan to ship **ircb** as a standalone, consumable product. It's eventually going to be used in **waartaa** as it's core engine to handle IRC connections.

This initial release ships with the following features:

- Allow multiple simultaneous client connection for a IRC network connection
- Support SSL connection to IRC server
- Basic CLI to create users, networks
- Autojoin previously joined channels on re connecting to IRC network

I'd like to thank [Sayan Chowdhury](https://github.com/sayanchowdhury/) and [Pol Baladas Luna](https://github.com/PolBaladas) for their contributions for this release.

Please feel free to give [ircb](https://github.com/waartaa/ircb) a run and share your feedback [here](https://github.com/waartaa/ircb/issues). Your suggestions and feedback are extremely valuable to us and will help us in developing this product accordingly.

There's a lot of cool features to work on and ship. So, stay tuned for the next release.

