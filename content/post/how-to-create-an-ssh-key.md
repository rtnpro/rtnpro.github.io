+++
author = "Ratnadeep Debnath"
categories = ["Fedora", "id_rsa", "openssh", "ratnadeep debnath", "ssh", "ssh-keygen", "ssh key", "yum", "rtnpro"]
date = 2009-05-05T18:06:49Z
description = ""
draft = false
slug = "how-to-create-an-ssh-key"
tags = ["Fedora", "id_rsa", "openssh", "ratnadeep debnath", "ssh", "ssh-keygen", "ssh key", "yum", "rtnpro"]
title = "How to create an ssh key?"

+++


I’m not going to talk on the theory of ssh key encryption and all, but how to create one.

OpenSSH is needed to be installed in the system to create an ssh key. Generally, OpenSSH comes with Fedora. But, anyways you can do:

> $ yum install openssh

to install it.

Once it is installed, do the following :

> $ ssh-keygen -t rsa

You will be prompted to enter the location to store the ssh key files, press Enter for default. The default location is

> ~/.ssh/id_rsa

Then you will be asked to enter the passphrase. Be careful, choose a passphrase that you can remember, otherwise you will not be able to recover your key. In that case you have to create a new key.

By default, your new private and public keys will be stored in `~/.ssh/id_rsa` and `~/.ssh/id_rsa.pub`, respectively.

You can share the public key openly, but keep the private key a secret. The public key is used by server administrators to grant you access.

For more details, visit

[http://openssh.org/](http://openssh.org/ "http://openssh.org/")

