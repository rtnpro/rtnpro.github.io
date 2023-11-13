+++
author = "Ratnadeep Debnath"
categories = ["CentOS", "Atomic", "Docker", "Kubernetes", "Projectatomic", "rtnpro"]
date = 2015-08-29T10:10:29Z
description = ""
draft = false
slug = "setting-up-atomic-on-centos-7"
tags = ["CentOS", "Atomic", "Docker", "Kubernetes", "Projectatomic", "rtnpro"]
title = "Setting up Atomic on CentOS 7"

+++


This post will run you through how to setup an environment [Atomic](https://github.com/projectatomic/atomicapp) apps on CentOS 7, by hand. For quickstart, you can just run the **Vagrantfile** at [here](https://github.com/rtnpro/vagrant-ansible-centos-7-atomic).

## Setup

### Install dependencies

    # yum install docker atomic kubernetes etcd

### Configure docker storage pool

Assuming that you have a new volume: ``/dev/vdb`` attached to your machine, configure ``/etc/sysconfig/docker-storage-setup`` as below:

    # cat <<EOF > /etc/sysconfig/docker-storage-setup
	DEVS=/dev/vdb
	VG=docker-vg
	EOF

### Start and enable Docker

	# systemctl start docker
    # systemctl enable docker
    
You can run ``# docker info`` to check the new specs for docker storage pool.

### Configure service account key for Kubernetes

	# mkdir /etc/pki/kube-apiserver
	# openssl genrsa -out /etc/pki/kube-apiserver/serviceaccount.key 2048
    # sed -i.back '/KUBE_API_ARGS=*/c\KUBE_API_ARGS="--service_account_key_file=/etc/pki/kube-apiserver/serviceaccount.key"' /etc/kubernetes/apiserver
    # sed -i.back '/KUBE_CONTROLLER_MANAGER_ARGS=*/c\KUBE_CONTROLLER_MANAGER_ARGS="--service_account_private_key_file=/etc/pki/kube-apiserver/serviceaccount.key"' /etc/kubernetes/controller-manager
    
### Start and enable Kubernetes

	# for SERVICE in etcd kube-apiserver kube-controller-manager  kube-scheduler docker kube-proxy  kubelet; do 
        systemctl restart $SERVICE
        systemctl enable $SERVICE
        systemctl status $SERVICE
    done
    
## Running atomicapp
 
 Now, we are all set to run an atomic app.
 
 	# atomic run projectatomic/helloapache
    
 This will, by default, use ``Kubernetes`` as a provider. You can find the newly created ``helloapache`` pod as below:
 
 	$ kubectl get pods
	NAME          READY     STATUS    RESTARTS   AGE
	helloapache   1/1       Running   0          2h

