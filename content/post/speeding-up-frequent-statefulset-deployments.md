+++
author = "Ratnadeep Debnath"
categories = ["Kubernetes", "statefulset", "cd", "continuous deployment", "rolling update", "eks", "nodeaffinity", "waitForFirstConsumer", "faster statefulset deployment", "blocked statefulset rolling update", "FailedAttachVolume"]
date = 2021-01-01T10:16:16Z
description = ""
draft = false
slug = "speeding-up-frequent-statefulset-deployments"
summary = "This post explores the issue of blocked rolling updates during continuous deployment to Kubernetes Statefulsets due to failed volume attach attempts. It talks about workarounds and solutions for this issue, which results in consistent and faster STS rolling update times."
tags = ["Kubernetes", "statefulset", "cd", "continuous deployment", "rolling update", "eks", "nodeaffinity", "waitForFirstConsumer", "faster statefulset deployment", "blocked statefulset rolling update", "FailedAttachVolume"]
title = "Speeding up Statefulset continuous deployments"

+++


[Kubernetes](https://kubernetes.io/)  [Statefulsets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/) are best suited for running stateful applications, for example, databases, datastores, object storage, file storage, etc. which are not deployed too often. But, **what if you have a legacy stateful application deployed as Statefulset, and needs continuous deployment throughout the day**? You will start running into the following issues:

* **Blocked rolling updates**
* **Very slow rolling update** during deployment, as Statefulset pods are rotated sequentially, one by one

> I have observed such behaviour in AWS Cloud (EKS), and this may or may not apply to other cloud providers or infrastructure setup. However, I am optimistic that my learnings here can be mapped and tweaked for other environments as well.

**Kubernetes setup summary**

* AWS EKS 1.16
* Multiple nodegroups distributed across multiple AZs (us-east-1b, us-east-1c, us-east-1d) for HA
* Storageclass spec

```YAML
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: gp2
  annotations:
    storageclass.beta.kubernetes.io/is-default-class: "true"
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
  fsType: ext4
reclaimPolicy: Delete
volumeBindingMode: Immediate
allowVolumeExpansion: true
```

Example statefulset

```YAML
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  selector:
    matchLabels:
      app: nginx # has to match .spec.template.metadata.labels
  serviceName: "nginx"
  replicas: 14 # by default is 1
  template:
    metadata:
      labels:
        app: nginx # has to match .spec.selector.matchLabels
    spec:
      terminationGracePeriodSeconds: 10
      containers:
      - name: nginx
        image: k8s.gcr.io/nginx-slim:0.8
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: "my-storage-class"
      resources:
        requests:
          storage: 1Gi
```

# Why does Statefulset rollout get blocked?

During the happy initial Statefulset deployment, when cluster might had enough room or less constraints, persistent volumes for each pod got created and pods got scheduled to different nodes, across multiple AZs. Let's say statefulset pod `web-3` had been initially scheduled on a node in `us-east-1b` AZ and is attached to `www-3` PVC.

Now during subsequent deployments, Kubernetes, for some reasons, might decide to schedule the pod `web-3` to `us-east-1c` AZ, and the pod will fail to schedule because the EBS volume for the PVC `www-3` cannot be attached to a node in `us-east-1b` AZ, a different AZ from where it was provisioned.

You can assert this behaviour by looking at the recent pod events from the stuck pod by describing the pod. You will seem error messages containing `FailedAttachVolume`. Note the `node` where the pod is being scheduled, and check it's AZ from the Kubernetes node's description. Now, describe the Persistent Volume being used by the pod's Persistent Volume Claim (PVC). You will notice that the PV is on a different AZ than where the pod is being scheduled.

# Workarounds

As a workaround, you can try to delete the pod `web-3` till Kubernetes manages to schedule it in the correct AZ. If that does not work, the you can take a more dangerous approach:

* Delete PVC for pod `www-3` so that it's provisioned once again. This will lead to data loss
* Delete pod `web-3`

If you are lucky, Kubernetes will be able to schedule the pod and the related PV in the same AZ. But, sooner or later, you will notice that it's not guaranteed. I have often seen Kubernetes try to create the PV in a different AZ and schedule the pod in another. This happens when the `volumeBindingMode` for the `StorageClass` is `Immediate`. During this case, the PV provisioning happens independent of the pod scheduling.

# Solutions

## WaitForFirstConsumer

To make PV provisioning take into account the pod's scheduling, you will need to update the `volumeBindingMode` for the `StorageClass` to `WaitForFirstConsumer`

```YAML
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: gp2
  annotations:
    storageclass.beta.kubernetes.io/is-default-class: "true"
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
  fsType: ext4
reclaimPolicy: Delete
volumeBindingMode: WaitForFirstConsumer  # <<<< Change
allowVolumeExpansion: true
```

This will just help you one time. You may still run into the situation discussed above during subsequent rollouts, if Kubernetes decides to schedule the rotated pod in a different AZ than where it was before.

## Statefulsets per AZ

You can create multiple statefulsets, for each AZ, instead of a single one. This will guarantee pods in a Statefulset get scheduled in a particular AZ, and be able to consistently bind to the associated PVC. For example, you can create 3 Statefulsets with affinity rules so that each Statefulset deploys pods to a particular AZ.

```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web-us-east-1b
spec:
  ...
  template:
    ...
    spec:
      ...
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: failure-domain.beta.kubernetes.io/zone
                operator: In
                values:
                - us-east-1b
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web-us-east-1c
spec:
  ...
  template:
    ...
    spec:
      ...
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: failure-domain.beta.kubernetes.io/zone
                operator: In
                values:
                - us-east-1c
...
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web-us-east-1d
spec:
  ...
  template:
    ...
    spec:
      ...
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: failure-domain.beta.kubernetes.io/zone
                operator: In
                values:
                - us-east-1d
```

### Bonus side effects

This will also cut down your deployment/rollout times by many (3 here) folds, since you will be doing multiple (3) pods (1 per `Statefulset`) rotations in parallel.

# Summary

If you are running a stateful application thats being continuously deployed to, you should:

* Use a `StorageClass` with `volumeBindingMode` set to `WaitForFirstConsumer`
* Use per AZ Statefulset instead of a single one to guarantee that rotated pods will end up on the same AZ, and will be able to bind to provisioned PV in the AZ.

