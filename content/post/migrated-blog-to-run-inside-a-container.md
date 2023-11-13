+++
author = "Ratnadeep Debnath"
categories = ["rtnpro", "CentOS", "Docker", "ghost", "selinux", "planet"]
date = 2016-06-18T16:05:00Z
description = ""
draft = false
slug = "migrated-blog-to-run-inside-a-container"
tags = ["rtnpro", "CentOS", "Docker", "ghost", "selinux", "planet"]
title = "Migrated blog to run inside a container"

+++


Today, I migrated my [Ghost](https://ghost.org/) blog to a Docker container running on CentOS 7. The config and content data for the blog has been pushed to https://github.com/rtnpro/rtnpro.com.

The setup was pretty simple.

    git clone https://github.com/rtnpro/rtnpro.com
    cd rtnpro.com

    # Allow sharing this directory with a Docker container
    sudo chcon -Rt svirt_sandbox_file_t ./

    # Allow nginx access port 2368 expose from Docker container
    sudo semanage port -a -t http_port_t -p tcp 2368

    # Run container
    sudo docker run -d -ti -p 2368:2368 \
    --name rtnpro_com_ghost \
    -v $(pwd):/var/lib/ghost \
    -v $(pwd)/themes:/usr/src/ghost/content/themes \
    -e NODE_ENV=production \
    ptimof/ghost

    cp nginx.conf /etc/nginx/conf.d/www.rtnpro.com.conf

    sudo service nginx restart

And, tada! I have my blog running inside a container now.

