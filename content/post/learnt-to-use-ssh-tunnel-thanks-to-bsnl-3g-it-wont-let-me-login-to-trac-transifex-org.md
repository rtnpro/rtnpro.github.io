+++
author = "Ratnadeep Debnath"
categories = ["rtnpro"]
date = 2011-03-19T08:05:03Z
description = ""
draft = false
slug = "learnt-to-use-ssh-tunnel-thanks-to-bsnl-3g-it-wont-let-me-login-to-trac-transifex-org"
tags = ["rtnpro"]
title = "Learnt to use SSH tunnel - Thanks to BSNL 3G, it won't let me login to trac.transifex.org"

+++


For some days, I was not able to login to trac.transifex.org. I though that it might be a browser issue. So, I deleted the browser data and tried to login using firefox, opera and google-chrome, but I still couldn’t login.

Then, I established an ssh tunnel from my computer to a remote server and set up proxy settings in my browser to use the ssh tunnel. And now, I can login correctly. This shows that it is not a browser issue but a connection issue.  
 I think something is terribly wrong with BSNL 3G servers and their maintainers.

Similarly, koji-client does not work over BSNL 3G. I have been tired complaining to the BSNL 3G Customer Care about this issue. But they don’t understand the issue only and issue is never solved.

