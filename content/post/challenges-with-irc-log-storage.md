+++
author = "Ratnadeep Debnath"
categories = ["Fedora", "waartaa", "irc", "planet", "ircb", "logging", "fedora-hubs", "rtnpro"]
date = 2016-04-21T08:52:25Z
description = ""
draft = false
slug = "challenges-with-irc-log-storage"
tags = ["Fedora", "waartaa", "irc", "planet", "ircb", "logging", "fedora-hubs", "rtnpro"]
title = "Storing IRC logs for Fedora Hubs"

+++


We have started working to power realtime IRC chat on Fedora Hubs pages, using Waartaa. We plan to load the Waartaa chat widget as an ``<iframe>`` inside Fedora Hubs.

The user story we are looking forward to create is as follows.

A user logs in to Fedora Hubs using his FAS account. He then creates a network connection to IRC and connects to the IRC server. Under the hood, it will run an **ircb** bot for the user's IRC connection in **waartaa**. The user can now chat on IRC from Fedora Hubs pages, or, from an IRC client of his choice, simultaneously, using the same IRC nick.

In Fedora Hubs, we want to have notifications for channel mentions or PMs when the user is away, and also allow scrollback to past logs. This requires the logs to be saved for future reference. However, there are concerns and challenges in storing IRC logs:

**Privacy concerns**

Not all channels like getting logged, publically. Usually, it's an etiquette to take permission from channel admins before placing a bot in the channel for logging. In waartaa, we do not display the logs publically. Also, the way Waartaa works is that it does not need a particular bot to log a channel. As long as there is a single user from Waartaa online on a IRC channel, waartaa can log a channel. IRC channels don't discourage using bouncers and waartaa is an intelligent bouncer under the hood. I see no blockers to stop us from logging IRC channels.

**Storage complexity**

Storing IRC logs per channel for every user, like desktop IRC clients, will lead to an increased storage complexity. So, we plan to store logs only per channel, same for all user. These common logs will not include the notice messages that the user receives. If needed, we can store the notice messages for a user, which is usually less in number compared to chat logs, separately, and fetch them when needed to be displayed in the UI. This will help us save disk space and also cache latest logs for a channel for all users.

**Log expiry**

We plan to store logs, for a limited period only, say two weeks. This will help us to link to the logs for recent mentions.

These are the above areas (there may be others) I think we need to discuss with the community to take policy decisions. I will start a thread on the mailing list for this. Once that is done, we can go ahead and start implementing the mechanisms.

