+++
author = "Ratnadeep Debnath"
categories = ["planet", "home", "tech", "Kubernetes", "rtnpro", "nginx-ingress", "external-dns", "openvpn", "clusterip"]
date = 2019-08-22T13:54:58Z
description = ""
draft = false
image = "https://images.unsplash.com/photo-1505881502353-a1986add3762?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=1080&fit=max&ixid=eyJhcHBfaWQiOjExNzczfQ"
slug = "enabling-publish-service-for-clusterip-nginx-ingress-controller-service"
tags = ["planet", "home", "tech", "Kubernetes", "rtnpro", "nginx-ingress", "external-dns", "openvpn", "clusterip"]
title = "Enabling publish-service for ClusterIP nginx-ingress controller service"

+++


Currently, [**nginx-ingress**](https://github.com/kubernetes/ingress-nginx/) controller can be run as a `ClusterIP` type service, however, it does not allow publishing this service's endpoints to associated ingress objects. As a result, [**external-dns**](https://github.com/kubernetes-incubator/external-dns) is not able to detect these ingress objects without any IP Address to create/update `A` DNS records for them.

## Why?

Many may argue that why will someone need DNS records for ingress objects using `ClusterIP` type nginx-ingress service. They aren't reachable from outside the kubernetes cluster, anyways.

However, I see a number of reasons why you will want to create DNS records for ingress objects associated with a `ClusterIP` type `nginx-ingress` service.

* Regarding accessing the cluster's internal network from outside, we can always install OpenVPN in the Kubernetes cluster using this [chart](https://github.com/helm/charts/tree/master/stable/openvpn), and expose it to the internet using `NodePort` or `LoadBalancer` type service.
* Always creating `LoadBalancer` type service can be costly.
* Some cloud providers like [**DigitalOcean**](https://www.digitalocean.com/) do not support private load balancer, and their compute nodes are accessible over the public internet. So, we cannot expose our internal applications via `LoadBalancer` or `NodePort` type service in these cloud providers.

So, I see a solid reason why we might want to enable publishing endpoints for `ClusterIP` type `nginx-ingress` service to the associated `ingress` objects, so that `external-dns` can discover them for publishing DNS records for them.

## How?

If you look at the source of `nginx-ingress` here: [kubernetes/ingress-nginx](https://github.com/kubernetes/ingress-nginx/blob/84102eec2ba270f624c57023aab59aab4471178e/internal/ingress/status/status.go#L176-L198), you will see that `publishService` is done only for `LoadBalancer` and `NodePort` type service. All we need to do is enable it for `ClusterIP` as well.

This is how I achieved it here: [https://github.com/kubernetes/ingress-nginx/pull/4462](https://github.com/kubernetes/ingress-nginx/pull/4462)

{{< figure src="/images/2019/08/image.png" >}}

I have already built a docker image from this change and I am using this custom image in my personal DigitalOcean Kubernetes cluster. And, it worked like a charm.

`docker.io/rtnpro/nginx-ingress-controller-amd64:latest`

If you want to do something similar, please feel free to use my work above.

