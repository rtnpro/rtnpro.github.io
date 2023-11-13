+++
author = "Ratnadeep Debnath"
categories = ["rtnpro", "CentOS", "containers", "planet"]
date = 2017-04-14T10:19:53Z
description = ""
draft = false
slug = "centos-community-container-pipeline"
tags = ["rtnpro", "CentOS", "containers", "planet"]
title = "CentOS Community Container Pipeline"

+++


[CentOS Community Container Pipeline](https://wiki.centos.org/ContainerPipeline) empowers running a container registry (currently at https://registry.centos.org) to facilitate upstream and distro components to be delivered in a format suitable to be consumed by container tool chains on CentOS Linux.It enables upstream projects to build, test and deliver latest and safest container images, everytime and effortlessly (or with minimal efforts).

#### Problem statement
Containers are a great way to package and deliver applications and there are loads of container images around. However, few of them are authorized, trustworthy and well maintained. It’s easy for developers to release a container image, but not that easy to maintain it over a period of time, and to ensure that their container images always carry the latest fixes and updates.

#### Solution
CentOS came up with the CentOS Container Pipeline Service to enable continuous build, test and delivery of containers, right from source code to a public container registry, https://registry.centos.org. Apart from automatically building container images from source code (which Docker Hub already does), the pipeline also:

- triggers image builds when:
   - the base image for the container is updated
   - there is a package update for the container
- allows automated testing of built container images
- scans built container images for security vulnerabilities, metadata, etc.

and then it delivers it to the public container registry along with sending detailed mail notifications to developers. This enables the developers to ensure that their container images are always up to date and secure with minimal manual intervention.

#### How to get started?

There's a detailed doc on how to get started with the container pipeline at https://github.com/CentOS/container-index. However, I will summarize it here as well:

1. Fork and clone https://github.com/CentOS/container-index.
1. Copy ``cccp.yml`` file included in the above repo into your project's directory containing the ``Dockerfile``.
1. Customize ``cccp.yml`` as needed.``cccp.yml`` is heavily commented.
1. Back in the ``container-index`` repo, in the ``index.d`` directory, you can do either
   1. Put your project in a new namespace ``yml`` file, by copying ``index_template.yml`` file to a desired file.
   1. You can add your project details in an existing namespace ``yml`` file.

     Please note that, in your project details, ``app-id`` must be the same as the namespace filename containing your project.
1. Commit your changes and send a pull request to https://github.com/CentOS/container-index and wait for a maintainer to accept it.

Once the pull request is merged, and the pipeline will pick up your project's artifacts, and build a container image for it, test it and finally deliver it to https://registry.centos.org. The container pipeline will also send you a mail containing build report of your project's container build once the build has succeeded or failed. The pipeline will also track your project periodically for changes, and package updates from repositories, and re build your project's container image.

To summarize, once you register your project with the CentOS Container Pipeline, the pipeline maintains your project's container images for you, with as minimal intervention as needed.

