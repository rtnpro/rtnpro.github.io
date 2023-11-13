+++
author = "Ratnadeep Debnath"
categories = ["planet", "Fedora", "selinux", "rtnpro", "systemd", "policy", "freeze"]
date = 2016-08-18T07:11:52Z
description = ""
draft = false
slug = "failed-to-load-selinux-policy-freezing"
tags = ["planet", "Fedora", "selinux", "rtnpro", "systemd", "policy", "freeze"]
title = "Failed to load SELinux policy. Freezing"

+++


In this post, I will tell you how to fix "Failed to load SELinux policy. Freezing" issue and reboot your Fedora with SELinux set to Enforcing.

It was the first day of Flock 2016, when I was working on my demos, that my laptop failed to boot after a bad system halt. The error was:
```
systemd[1]: Failed to load SELinux policy. Freezing.
```
I was running Fedora 23 on my laptop with **SELinux** set to **enforcing**. After searching about this issue on the internet,  I found that most people had got around it by setting ``selinux=0`` in the boot parameters. Well, it worked and got me back on my Fedora without SELinux, however, this was not the solution I was looking for.

I tried editing ``/etc/selinux/config`` to set ``SELINUX`` to ``permissive`` and ``disabled``. While Fedora was able to reboot with ``SELINUX`` set to ``disabled``, but not when it was set to ``permissive``.

Then, a really old school solution, suggested by [Kevin Frenzi](https://fedoraproject.org/wiki/User:Kevin) finally worked. The solution was:

- Uninstall SELinux and related packages: ``sudo dnf remove -y selinux-policy``
- Remove SELinux related files and directories" ``sudo rm -rf /etc/selinux /var/lib/selinux``
- Install SELinux again: ``sudo dnf install selinux-policy``
- Ensure SELinux is set to enforcing in ``/etc/selinux/config``

Now, Fedora is able to reboot properly with SELinux set to enforcing.

