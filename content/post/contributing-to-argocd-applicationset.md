+++
author = "Ratnadeep Debnath"
categories = ["argocd", "Kubernetes", "cd", "zapier", "applicationset", "opensource", "contribution", "cncf"]
date = 2021-05-11T12:00:00Z
description = ""
draft = false
slug = "contributing-to-argocd-applicationset"
summary = "Exploring ArgoCD ApplicationSet at Zapier and contributor experience"
tags = ["argocd", "Kubernetes", "cd", "zapier", "applicationset", "opensource", "contribution", "cncf"]
title = "Contributing to ArgoCD ApplicationSet"

+++


# The problem

At [Zapier](https://zapier.com), we use ArgoCD to manage our applications across multiple Kubernetes clusters. We use the [App of Apps](https://argoproj.github.io/argo-cd/operator-manual/cluster-bootstrapping/#app-of-apps-pattern) pattern to maintain application config in our [ArgoCD](https://argoproj.github.io/argo-cd/)'s gitops repo. We started facing a number of challenges with the App of apps pattern:

* We needed to do some extra tooling to define the ArgoCD `Application` specs for each app.
* Since, we have multiple clusters per environment, we ended up having loads of **duplicate** code for small changes we need to do for each cluster. We badly needed some sort of templating on top of app config value files to prevent duplication of code.
* With so many config files along with environment, cluster level overrides, it becomes huge challenge during various cluster level operations, e.g., disable traffic to a cluster, disable apps in a cluster, etc.

# Looking for a solution

During our summer 2020 (August) Engineering Retreat, we thought of working on some sort of tooling (template engine) to render cluster specific Argo CD application config files from a common template and some environment, cluster specific metadata. During that process, we realised that folks at Argo were also brainstorming on similar lines and trying to figure out ways improve upon the App of apps pattern. They were working on a new spec called the ApplicationSet. So, we decided to explore this project and see if we can help each other.

[https://docs.google.com/document/d/1juWGr20FQaJmuuTIS8mBFmWWDU422M_FQMuhp5c1jt4/edit#heading=h.j20rtjo8coht](https://docs.google.com/document/d/1juWGr20FQaJmuuTIS8mBFmWWDU422M_FQMuhp5c1jt4/edit#heading=h.j20rtjo8coht)

[https://github.com/argoproj-labs/applicationset](https://github.com/argoproj-labs/applicationset)

The ApplicationSet repo did not have a full working implementation back then, but the spec looked promising for our use case at Zapier. As an early user of the project, I ran into some issues and started working on to implement certain features we needed out of the project.

# Contributions

## Implement cluster generator

Cluster generator allows generating ArgoCD `Application` specs from `ApplicationSet` using metadata available in the registered Kubernetes cluster.

[https://github.com/argoproj-labs/applicationset/blob/master/examples/cluster/cluster-example.yaml](https://github.com/argoproj-labs/applicationset/blob/master/examples/cluster/cluster-example.yaml)

This [pull request](https://github.com/argoproj-labs/applicationset/pull/18) implemented the cluster generator for ApplicationSet.

## Implement Git file discovery generator

Git file discovery generator allows reading `JSON` config files stored in a Git repo and use it to generate `Application` specs from `ApplicationSet`.

[https://github.com/argoproj-labs/applicationset/tree/master/examples/git-generator-files-discovery](https://github.com/argoproj-labs/applicationset/tree/master/examples/git-generator-files-discovery)

This [pull request](https://github.com/argoproj-labs/applicationset/pull/45) implemented the Git file discovery generator for ApplicationSet.

# Thanks

A huge shoutout to [OmarKahani](https://github.com/OmerKahani) and [Devan Goodwin](https://github.com/dgoodwin) to review my pull requests and merge my code to ApplicationSet repo. Thanks to the ArgoCD ApplicationSet team to acknowledge my contributions in their [v0.1.0](https://github.com/argoproj-labs/applicationset/releases/tag/v0.1.0) release. The contributor experience for ArgoCD ApplicationSet was really great with friendly and helpful upstream maintainers.



