+++
author = "Ratnadeep Debnath"
categories = ["CentOS", "Atomic", "Docker", "Projectatomic", "nulecule", "rtnpro"]
date = 2015-09-07T07:42:26Z
description = ""
draft = false
slug = "nuleculizing-an-docker-image"
tags = ["CentOS", "Atomic", "Docker", "Projectatomic", "nulecule", "rtnpro"]
title = "Nuleculize your Docker application"

+++


It's easier to explain things with an example. So, I will take up [centos/postgresql](https://hub.docker.com/r/centos/postgresql/) docker image and nuleculize it for Docker and Kubernetes as providers. This post assumes that you have setup an Atomic environment on CentOS7/Fedora. If you have not done so, you can follow the instructions [here](http://www.rtnpro.com/setting-up-atomic-on-centos-7/).

**First**, we'll create scaffolding for our Nulecule PostgreSQL application.

	mkdir -p nulecule-postgresql/artifacts/{docker,kubernetes}
    touch nulecule-postgresql/{Dockerfile,LICENSE,Nulecule,README.md}
    touch nulecule-postgresql/docker/postgresql-app-pod_run
    touch nulecule-postgresql/kubernetes/{postgresql-pod,postgresql-service}.yaml
    

**Second**, we'll edit ``nulecule-postgresql/Nulecule`` file as below:
```
---
specversion: 0.0.2
id: postgresql-atomicapp

metadata:
  name: PostgreSQL Atomic App
  appversion: 1.0.0
  description: This is PostgreSQL
graph:
  - name: postgresql-atomicapp
    params:
      - name: db_user
        description: Database User
      - name: db_pass
        description: Database Password
      - name: db_name
        description: Database Name
    artifacts:
      kubernetes:
        - file://artifacts/kubernetes/postgresql-pod.yaml
        - file://artifacts/kubernetes/postgresql-service.yaml
      docker:
        - file://artifacts/docker/postgresql-app-pod_run
```
        
 **Third**, we'll edit ``nulecule-postgresql/Dockerfile`` as below:
```
FROM projectatomic/atomicapp:0.1.3

MAINTAINER Ratnadeep Debnath <rtnpro@redhat.com>

LABEL io.projectatomic.nulecule.specversion="0.0.2" \
	  io.projectatomic.nulecule.providers="kubernetes,docker"

ADD /Nulecule LICENSE /application-entity/
ADD /artifacts /application-entity/artifacts
```
 
 **Fourth**, we'll edit ``nulecule-postgresql/artifacts/docker/postgresql-app-pod_run`` as below:
```
docker run -d --name postgresql-atomicapp-app -p 5432:5432 -e DB_USER=$db_user -e DB_PASS=$db_pass -e DB_NAME=$db_name centos/postgresql
```
**Fifth**, we'll edit ``nulecule-postgresql/artifacts/kubernetes/postgresql-pod.yaml`` as below:
```
apiVersion: v1
id: postgresql
spec:
  containers:
    - name: postgresql
      image: centos/postgresql
      env:
        - name: DB_NAME
          value: $db_name
        - name: DB_USER
          value: $db_user
        - name: DB_PASS
          value: $db_pass
      ports:
        - containerPort: 5432
metadata:
  name: postgresql
  labels:
    name: postgresql
kind: Pod
```
    
**Sixth**, we'll edit ``nulecule-postgresql/artifacts/kubernetes/postgresql-service.yaml`` as below:
```
kind: Service
apiVersion: v1
metadata:
  name: postgresql
  labels:
    name: postgresql
    name: postgres
spec:
  ports:
    - protocol: TCP
      port: 5432
      targetPort: 5432
      nodePort: 0
  selector:
    name: postgresql
  portalIP: None
  type: ClusterIP
  sessionAffinity: None
status:
  loadBalancer: {}
```   
 Edit ``nulecule-postgresql/{LICENSE,README.md}`` as needed.
 
 **Seventh**, we'll build our Nulecule application image as below:
```
cd nulecule-postgresql
sudo docker build -t nulecule-centos-postgresql .
```
    
 **Eighth**, we'll the run our newly created nulecule image:
``` 
sudo atomic run nulecule-centos-postgresql
``` 
 Enter database name and credentials during runtime, when prompted to do so (if you have not configured ``answers.conf`` already). Once the command finishes execution, you can check running **postgresql** application on kubernetes as below:
```
$ kubectl get service postgresql
NAME      LABELS    SELECTOR       IP               PORT(S)
postgresql   name=db   name=postgresql   10.254.167.159   5432/TCP
```
You can also access the **postgresql** database as below:
```
$ sudo docker run --rm -ti centos/postgresql psql -h 10.254.167.159 -U <db user> -d <db name> -W
Password for user test: <password>
psql (9.2.7)
Type "help" for help.

test=# \l
                             List of databases
  Name     |  Owner   | Encoding  | Collate | Ctype |   Access privileges
-----------+----------+-----------+---------+-------+-----------------------
 postgres  | postgres | SQL_ASCII | C       | C     |
 template0 | postgres | SQL_ASCII | C       | C     | =c/postgres          +
    	   |          |           |         |       | postgres=CTc/postgres
 template1 | postgres | SQL_ASCII | C       | C     | =c/postgres          +
    	   |          |           |         |       | postgres=CTc/postgres
 test      | postgres | SQL_ASCII | C       | C     | =Tc/postgres         +
	       |          |           |         |       | postgres=CTc/postgres+
		   |          |           |         |       | test=CTc/postgres
(4 rows)

test=# \conninfo
You are connected to database "test" as user "test" on host "10.254.167.159" at port "5432".
```
	
This was a summary of steps you need to follow to nuleculize your existing Docker application. For more info, you can refer to [Getting started guide of Nulecule](https://github.com/projectatomic/nulecule/blob/master/docs/getting-started.md).

