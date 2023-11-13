+++
author = "Ratnadeep Debnath"
date = 2021-06-08T05:28:09Z
description = ""
draft = true
slug = "manage-dns-for-an-existing-domain-using-aws-route53"
title = "Manage DNS for an existing domain using AWS Route53"

+++


I usually purchase my domains from Godaddy, but I prefer to manage DNS for the domains using AWS Route53, alongside my cloud infrastructure. Every time, I need to setup DNS management using AWS Route53 for an existing domain, I'd end up searching and find some very verbose documents on how to do so. This can get a bit frustrating for such a trivial thing. So, I decided to document a quick setup process here.

* Create a HostedZone for the domain or sub domain you wish to manage in Route53
* Note the nameservers for the NS record for the created HostedZone

{{< figure src="/images/2021/06/image-1.png" >}}

* If you have an existing domain with other DNS records, you can just add new `NS` records for the above nameservers. If you don't have any existing records in your domain, you can replace the existing nameservers for the domain with the above ones. If you want to just manage DNS for a subdomain, add the above NS records for the subdomain.



# 

