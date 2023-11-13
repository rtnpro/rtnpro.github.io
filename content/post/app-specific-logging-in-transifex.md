+++
author = "Ratnadeep Debnath"
categories = ["django", "inspect", "inspect.stack()", "logger", "python", "ratnadeep debnath", "transifex", "rtnpro"]
date = 2012-06-30T05:49:21Z
description = ""
draft = false
slug = "app-specific-logging-in-transifex"
tags = ["django", "inspect", "inspect.stack()", "logger", "python", "ratnadeep debnath", "transifex", "rtnpro"]
title = "App specific logging in Transifex"

+++


Yesterday, I was working on adding app specific loggers in Transifex. By app specific logger I mean a logger which shows the app name which generated the log. As of now, the logs in Transifex look something like this:

[sourcecode language=”python”]

2012-06-29 13:01:43,300 tx DEBUG Saved: Project Avant Window Navigator  
 2012-06-29 13:01:43,312 tx DEBUG Saved: Project Switchdesk  
 2012-06-29 13:01:43,324 tx DEBUG Saved: Project Usermode  
 2012-06-29 13:01:43,342 tx DEBUG Saved: Project desktop-effects  
 2012-06-29 13:01:43,349 tx DEBUG Saved: Project im-chooser  
 2012-06-29 13:01:43,355 tx DEBUG Saved: Project Test Project  
 2012-06-29 13:01:43,364 tx DEBUG Saved: Project Test Private Project  
 2012-06-29 13:01:45,704 tx DEBUG Saved: Project Test Project  
 2012-06-29 13:01:45,717 tx DEBUG Saved: Project Test Private Project  
 2012-06-29 13:01:45,731 tx DEBUG Resource Resource1: New ResourcePriority created.  
 [/sourcecode]

It does not tell anything about which app generated the logs. In a first glance, fixing this looks pretty straight forward and dumb. All it needs it to customize this [https://github.com/transifex/transifex/tree/devel/transifex/txcommon/log](https://github.com/transifex/transifex/tree/devel/transifex/txcommon/log) module for each app and instead of importing the logger from txcommon.log, import it from the log module inside the app.  
 But this would lead to a lot of code duplication and a lot of boring changes in the code. So, I decided to customize transifex.txcommon.log module itself so that it can detect the function calling the logger. It was pretty straight forward to do this for the handler at [https://github.com/transifex/transifex/blob/devel/transifex/txcommon/log/receivers.py#L6](https://github.com/transifex/transifex/blob/devel/transifex/txcommon/log/receivers.py#L6): def model_named() in the following way:

[sourcecode language=”python”]  
 import re

tx_module_regex = re.compile(  
 r’transifex(.addons)?.(?P<app_name>w+)(..*)?’)  
 def model_named(sender, message=”, **kwargs):  
 """  
 Receive signals for objects with a .name attribute.  
 """  
 from txcommon.log import _logger as logger  
 sender_module = sender.__module__  
 m = tx_module_regex.search(sender_module)  
 app_name = ‘.’ + m.group(‘app_name’) if m else ”  
 logger.name = ‘tx’ + app_name  
 obj = kwargs[‘instance’]  
 logger.debug("%(msg)s %(obj)s %(name)s" %  
 {‘msg': message,  
 ‘obj': sender.__name__,  
 ‘name': getattr(obj, ‘name’, ”)})

[/sourcecode]

**sender** is the object or instance for which the log is being generated. In our case, it’s a model instance. So, **sender.__module__** gives the parent module for **sender**. Using regular expressions, we extract the app name from the module name and we set the name of the logger as ‘**tx.<app_name>**‘. And we are done here (for now)! But when we do something like

[sourcecode language=”python”]  
 from transifex.txcommon.log import logger  
 logger.debug(‘foo bar’)  
 [/sourcecode]

we do not have a **sender** instance to allow us to find the **calling module** name. After some searching, I found about the [inspect](http://docs.python.org/library/inspect.html) python module. And all I needed was [inspect.stack()](http://docs.python.org/library/inspect.html#inspect.stack). Here’s what I did in [https://github.com/transifex/transifex/tree/devel/transifex/txcommon/log/__init__.py](https://github.com/transifex/transifex/tree/devel/transifex/txcommon/log/__init__.py):

1. Write a wrapper around logger instance,
2. find the caller calling the logger using **stack.inspect(),**
3. accordingly set the logger name,
4. and finally, log the event.

[sourcecode language=”python”]

import logging, re, inspect

_logger = logging.getLogger(‘tx’)

# regex to extract app name from a file path to a TXC app  
 tx_app_path_regex = re.compile(  
 r’txc/transifex(/addons)?/(?P<app_name>w+)/(..*)?’)  
 class Logger:  
 """  
 A wrapper class around _logger. This is used to log events  
 along with app names.  
 """  
 @classmethod  
 def get_app_name_from_path(cls, path):  
 """  
 Extracts app name from a file path to a TXC app

 Args:  
 path: A string for the file path  
 Returns:  
 A string for the app name or ”  
 """  
 m = tx_app_path_regex.search(path)  
 return m.group(‘app_name’) if m else ”

 @classmethod  
 def set_logger_name(cls):  
 """  
 Sets logger name to show calling app’s name.  
 """  
 # inspect.stack()[2] since cls.debug() method has now become the  
 # immediate caller in of this method in the stack. We want the caller  
 # of cls.debug() or other logging method wrappers.  
 caller_module_path = inspect.stack()[2][1]  
 app_name = cls.get_app_name_from_path(caller_module_path)  
 _logger.name = ‘tx’ + ‘.%s’ % app_name if app_name else ”

 @classmethod  
 def debug(cls, *args, **kwargs):  
 """Wrapper for _logger.debug"""  
 cls.set_logger_name()  
 _logger.debug(*args, **kwargs)

 # And similarly for other logger methods like info(), waring(), error(), critical()

logger = Logger  
 [/sourcecode]

Now, this is sweet! No one need to bother about logging events with app names. I am saved from editing hundreds of files and duplicating code ![;)](http://127.0.0.1:8080/wordpress/wp-includes/images/smilies/icon_wink.gif) It’s transparent and scalable. The logs now seem like:

[sourcecode language=”python”]  
 2012-06-29 20:39:03,635 tx.projects DEBUG Saved: Project Foo Project  
 2012-06-29 20:39:05,575 tx.projects DEBUG Saved: Project Avant Window Navigator  
 2012-06-29 20:39:05,587 tx.projects DEBUG Saved: Project Switchdesk  
 2012-06-29 20:39:05,599 tx.projects DEBUG Saved: Project Usermode  
 2012-06-29 20:39:05,612 tx.projects DEBUG Saved: Project desktop-effects  
 ……….  
 ……….  
 ……….  
 2012-06-29 22:15:07,088 tx.webhooks DEBUG Project project1 has no web hooks  
 2012-06-29 22:15:07,177 tx.releases DEBUG Deleted: ReleaseNotifications  
 2012-06-29 22:15:07,177 tx.releases DEBUG Deleted: Release All Resources  
 2012-06-29 22:15:07,466 tx.txcommon DEBUG Running low-level command ‘msgfmt -o /dev/null –check-format –check-domain -‘  
 2012-06-29 22:15:07,469 tx.txcommon DEBUG CWD: ‘/home/rtnpro/transifex/rtnpro/github/txc/transifex’  
 2012-06-29 22:15:07,661 tx.releases DEBUG release: Checking string freeze breakage.  
 2012-06-29 22:15:07,702 tx.resources DEBUG resource: Checking if resource translation is fully reviewed: Test Project: Resource1 (pt_BR)  
 2012-06-29 22:15:07,707 tx.webhooks DEBUG Project project1 has no web hooks  
 2012-06-29 22:15:07,740 tx.resources DEBUG resource: Checking if resource translation is fully reviewed: Test Project: Resource1 (ar)  
 2012-06-29 22:15:07,745 tx.webhooks DEBUG Project project1 has no web hooks  
 [/sourcecode]

Thanks for reading. If you have any suggestions or query, please feel free to comment.

