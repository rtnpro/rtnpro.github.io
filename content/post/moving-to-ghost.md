+++
author = "Ratnadeep Debnath"
categories = ["blog", "nikola", "ghost", "rtnpro"]
date = 2015-02-28T12:55:36Z
description = "Migrated blog from nikola static site generator to ghost."
draft = false
slug = "moving-to-ghost"
tags = ["blog", "nikola", "ghost", "rtnpro"]
title = "Moving to Ghost"

+++


Today, I migrated my blog: [http://www.rtnpro.com](http://www.rtnpro.com) to [Ghost](https://ghost.org/). I was previously writing blogs and maintaining my static blog website using [Nikola](http://getnikola.com/). I like Nikola for being a very featureful and powerful static blog generator. I find the ReStructuredText format handy when generating complex HTML, and especially, I fell in love with the custom RST shortcuts provided by Nikola. But, flexibility breeds complexity.

##### Why migrate?

- A blog post should be simple and easily renderable in blog aggregators. When you do too much custom HTML/CSS stuff in your post, e.g., playing grid layout of Twitter bootstrap, etc. things break.
- When you start using a lot of custom things provided by a static blog generator like Nikola, migration becomes a pain. Luckily, I had just started to use Nikola specific RST shortcuts.
- You want to write blogs, and not code. It was a pain to migrate to newer versions of Nikola. Usually, it involved a lot of fiddling with the conf files, sometimes the source code as well.
- I kinda like WYSIWYG, and especially web based editors accessible from anywhere over the internet.
- The final blow was my nikola based blog's RSS feed not being parsed by the planet aggregator: https://github.com/rubys/venus used by [Fedor planet](http://planet.fedoraproject.org/), [DGPLUG planet](http://planet.dgplug.org/), etc. For a second, I thought, I can write a patch to fix wherever the bug is. But then, enough is enough. I want to write blog posts and not code each time to do it.

##### Migration

- Installed `ghost` plugin in my wordpress blog and exported all content in a format `ghost` understands
- Installed `ghost` on local machine
- Signup and enter the admin dashboard
- Import blog posts from my wordpress blog
- Migrate RST blog posts from Nikola to Ghost
- Once everything looked fine, I installed `ghost` on my http://www.rtnpro.com server
- Exported all local `ghost` content and imported that in my ghost instance for http://www.rtnpro.com
- Rsync'd all the local images from `content/images/` to the remote server
- Setup `supervisord` to run `ghost` on my server as a daemon
- Added `nginx` rules to allow access to all public pages of [www.rtnpro.com](http://www.rtnpro.com) over `HTTP` and blocked access to admin pages at http://www.rtnpro.com/ghost/. I've configured another `nginx` virtual host to allow access to the `admin` pages over `HTTPS`.

Migration was not that painful as I was expecting it to be. It took me a few hours for doing the entire migration. I am liking Ghost's simplicity and the admin dashboards and the web editor for writing blog posts.

Now, I can focus on writing blog posts when I write one and not go debugging through some code :)

