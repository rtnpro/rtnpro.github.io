+++
author = "Ratnadeep Debnath"
categories = ["planet", "Red Hat", "CentOS", "containers", "rootconf", "devconf", "centos-container-pipeline", "rtnpro"]
date = 2017-05-24T06:45:39Z
description = ""
draft = false
slug = "rootconf-devconf-2017"
tags = ["planet", "Red Hat", "CentOS", "containers", "rootconf", "devconf", "centos-container-pipeline", "rtnpro"]
title = "Rootconf/Devconf 2017"

+++


This year's [Rootconf](https://rootconf.in/2017/) was special as it also hosted [Devconf](http://devconf.in) for the first time in India. The conference took place at MLR Convention Centre, JP Nagar, Bangalore on 11-12 May, 2017. The event had 2 parallel tracks running, 1 was for Rootconf and the other one for Devconf. Rootconf is a place like other Hasgeek events where you get to see friends and make new friends, learn about what they are up to and share your stints.

There was a great line up of talks and workshops in this year's Rootconf/Devconf. Some of the talks that I found interesting were:

- State of the open source monitoring landscape by [Bernd Erk](https://twitter.com/gethash) from Icinga
- Deployment strategies with Kubernetes by [Aditya Patawari](https://twitter.com/adityapatawari)
- [Pooja Shah](https://twitter.com/TechGirlPooja) speaking on their bot at Moengage to automate their CI/CD workflow
- Running production APIs on spot instances by [S Aruna](https://twitter.com/arunasank)
- FreeBSD is not a Linux distribution by Philip Paeps
- Automate your devops life with Openshift Pipelines by [Vaclav Pavlin](https://twitter.com/vpavlin)
- Fabric8: an end-to-end development platform by [Baiju](https://twitter.com/baijum)
- Making Kubernetes simple for developers using Kompose by [Suraj Deshmukh](https://twitter.com/surajd_)
- Workshop on Ansible by [Praveen Kumar](https://twitter.com/kumar_praveen) and [Shubham Minglani](https://twitter.com/ContainsCafeine)
- Deep dive into SELinux by Rejy M Cyriac

I, one of the [contributors](https://github.com/CentOS/container-pipeline-service/graphs/contributors) to the [CentOS Community Container Pipeline](https://wiki.centos.org/ContainerPipeline), gave a talk about the pipeline on how to build, test, deliver latest and safest container images, effortlessly. You can find the slides/demo for the talk [here](https://docs.google.com/presentation/d/17JX8_wMQKQR1C21E39vtVvE8r-NHvSe5yJUNKBZYunY/edit?usp=sharing). The talk was well received by the people and people were interested in our project and wanted to use it. A huge shout out to the container pipeline team to make this project happen. I will share some of the questions asked about the pipeline along with the answers for them:

- Can I use container pipeline to deploy my applications to production?

  The answer is that it depends on your use case. Nevertheless, you can use the images, e.g., redis, postgresql, mariadb, etc from container-pipeline, from registry.centos.org, and deploy it in production. If your application is Open Source, you can also build container image for your application on the pipeline and consume the image in production. However, you should be ready to expect some delay for your project’s new container image to be delivered, as the container pipeline is also used by other projects. If you want your containerized application to be deployed to production ASAP, you might consider setting up the container pipeline on premises, or use something like Openshift pipelines

- How can I deploy container pipeline on premise?

  We deploy container pipeline in production using Ansible, and you can do that as well. To start, you can look into the ``provisions/`` directory of our repository https://github.com/centos/container-pipeline-service

- Can we use scanners other than container-pipeline or integrate them with other workflow?

  We can use the scanners pulling them from registry.centos.org and call them through in any workflow to do the scan piece.

- What if the updated versions of rpms break my container image?

  In the current scenario we update the images if there is any change in dependency image or rpm update. But, in the future, we will be having an option to disable automatic image rebuilds on updates. However, we'll be notifying the image maintainer about such updates, so that the maintainer can decide whether to re build the image or not.

- Can we put images with non CentOS base image in the pipeline?

  For now, you can, but we do not encourage it, as you will be missing out on many of the valuable features of the container pipeline, e.g., package verify scanners, automatic image rebuilds on package updates, etc.

I also had a conversation with the Digitalocean folks where we discussed about doing a blog post about the CentOS container pipeline on their blog. We also had Zeeshan and Bamacharan from our team answering to queries about the pipeline at the Red Hat booth in Rootconf.

To sum up, it was a great conference, especially, in terms of showcasing many of our projects from Red Hat: Fabric8, Openshift Pipelines, CentOS Container Pipeline, etc. and getting feedback from the community. We'll continue to reach out to the community and getting them involved so that we can develop great solutions for them.

