+++
author = "Ratnadeep Debnath"
categories = ["proxysql", "Docker", "Kubernetes", "zapier"]
date = 2020-07-20T05:09:27Z
description = ""
draft = true
image = "/images/2020/07/actionvance-eXVd7gDPO9A-unsplash.jpg"
slug = "proxysql-docker-image-with-live-config-reload"
tags = ["proxysql", "Docker", "Kubernetes", "zapier"]
title = "ProxySQL Docker image with live config reload"

+++


[https://hub.docker.com/r/zapier/proxysql](https://hub.docker.com/r/zapier/proxysql)

[ProxySQL](https://proxysql.com) is a high performance MySQL proxy. Some of it's key features are:

* Application layer proxy
* Zero downtime changes
* Database firewall
* Advanced query rules
* Data sharding and transformation
* Failover detection

ProxySQL has support for Zero downtime config updates. Config changes need to be made via the ProxySQL admin MySQL interface as documented [here](https://github.com/sysown/proxysql/wiki#configuring-proxysql-via-the-admin-interface). Mostly, you will be adding/removing/update `mysql_servers`, `mysql_users`, `mysql_query_rules` tables. Once you are done, you will need to run load configs to runtime for the changes to be effective, and also save the latest configs to disk (not /etc/proxysql.cnf, /var/lib/proxysql/proxysql.db), so that ProxySQL picks up the latest config on restart. By now, you might start to notice that you need someone (a human or some tool/agent) to apply ProxySQL config updates. Not very GitOps friendly, right? The above workflow might work for stateful environments like VMs, baremetals, etc., but will fall short in case stateless containers running on Kubernetes or other container runtime environments. The current way to update config for ProxySQL containers would be to update config and restart ProxySQL containers/pods one by one. Restarting containers/pods like ProxySQL is a time taking process as it needs to drain existing connections, take pod out of service and then restarting it, and then add it back to the service.







We have been using ProxySQL at [Zapier](https://zapier.com) for over a year now in production. It has helped us in the following ways:

* Handle 5-10x more database client connections than it'd be possible with the RDS MySQL instance
* Reduced costs. We now run only 2 read replicas instead of 4.
* Update database backends without application config update and deployment.

But there's a gotcha here. Although, ProxySQL supports zero downtime changes to config, it is not exactly config file driven, but action driven. You will need to update the ProxySQL config via the MySQL admin interface, save it and load it to runtime. ProxySQL reads `/etc/proxysql.cnf` only during initial run. The config update process is more suited towards bare metal, vm scenarios where the machines are of more permanent nature with data persistence. Also, you will need some external script to update config, load it, etc.

However, in a container runtime environment, like Kubernetes, the pods are mostly stateless and fleeting. The pods can get scheduled in any node. Also, amongst the many ProxySQL container images out there, none of them, till this date, supports live config update. They all bank on the rotation of pods for config update.

We, at Zapier, created a custom ProxySQL Docker image to work around this problem.

[https://github.com/zapier/docker-proxysql](https://github.com/zapier/docker-proxysql)

This is what we do:

* Rather than rendering the ProxySQL config from file/configmap (which is not very safe to store database credentials), we render a config file template
* We render secrets from env variables to render the config file to /etc/proxysql.cnf from this template
* In the entrypoint script, along with running the ProxySQL daemon, we also run a background script to check for config template updates at regular intervals. If any change is detected, this background function will render a new config file, and hard reload config from the config file and load it to ProxySQL runtime.

This setup works well in a vanilla Docker (compose) or Kubernetes setup. For Kubernetes, we bank on the a feature that whenever you mount an entire configmap/secret to a path, any changes to the configmap/secret is reflected in the pod filesystem.

