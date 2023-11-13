+++
author = "Ratnadeep Debnath"
date = 2020-08-24T05:31:10Z
description = ""
draft = true
slug = "proxysql-on-kubernetes"
title = "ProxySQL on Kubernetes"

+++


This post shows you how to confidently run a ProxySQL HA setup in Kubernetes.

[ProxySQL](https://proxysql.com) is a high performance MySQL proxy. Some of it's key features are:

* Application layer proxy
* Zero downtime changes
* Database firewall
* Advanced query rules
* Data sharding and transformation
* Failover detection

Although a single ProxySQL instance/node can multiplex thousands of client connections, it's always better to run multiple instances of ProxySQL for HA. You will need some sort of Load Balancer to act as a proxy for your ProxySQL cluster, so that applications have a single endpoint to connect to ProxySQL. Restarting ProxySQL process or rotating the node will involve:

* Take out the instance from Load balancer to stop new connections to the ProxySQL instance
* Wait for connections to drain
* Restart ProxySQL or rotate the node
* Attach the node back to the LB, once ProxySQL service is up and running on the node.

To do the above operation gracefully, you will need some sort of tooling to automate the process. But, why do all these things yourself? Kubernetes already solves this problem using Service controller. Kubernetes makes it easier to manage a fleet of ProxySQL instances, be 1 or 100, ship updates, rotate ProxySQL instances or pods, without much hassle. Setting up a Kubernetes cluster just for ProxySQL might be an overkill, but if you are already running a Kubernetes cluster for your applications, then you are at an advantage.

Now, if you are convinced that you want to run ProxySQL on Kubernetes, you are welcome to continue reading to find out how.

### Planning your infra for ProxySQL

ProxySQL is a CPU intensive process. It uses more CPU than RAM. Since ProxySQL is a core piece of your application of your application stack, almost, as critical as a database, you might consider to run ProxySQL on dedicated nodes, to prevent noisy neighbours. You can taint your nodes so that only ProxySQL pods and cluster addons like fluentd, node-local-dns, datadog, etc. can get scheduled on the ProxySQL specific nodes.

### Configure deployments

From our experience, the best setup to run ProxySQL pods are:

* Spread out ProxySQL pods evenly across multiple AZs in your cloud provider. This allows you to tolerator AZ outages
* Run at least 2 ProxySQL pods in a node
* Graceful shutdown

You can configure the affinity rules for the pod to achieve this. What works for us is:

```YAML
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  ...
  name: proxysql
  namespace: proxysql
spec:
  ...
  template:
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
              - matchExpressions:
                  - key: zapier.com/nodegroup
                    operator: In
                    values:
                      - proxysql-us-east-1b
                      - proxysql-us-east-1c
                      - proxysql-us-east-1d
        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
            - podAffinityTerm:
                labelSelector:
                  matchExpressions:
                    - key: release
                      operator: In
                      values:
                        - proxysql
                topologyKey: kubernetes.io/hostname
              weight: 100
  ...
```

Using node affinity rules, we tell the pods to get scheduled in nodes with the specified `zapier.com/nodegroup` labels. With `podAntiAffinity` rules, we ensure that the pods get scheduled in all the nodes across all AZs evenly, first. Since, we have half the number of nodes than the number of pods, we will have another pod get scheduled on the nodes, because of `preferredDuringSchedulingIgnoredDuringExecution`. We will explain in a bit why we need to run multiple pods in a node.

As for graceful shutdown, we use Kubernetes lifecycle hooks to achieve this.

```
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  ...
  name: proxysql
  namespace: proxysql
spec:
  ...
  template:
    metadata:
      labels:
        app: proxysql
        release: proxysql
    spec:
      ...
      containers:
        - image: 'zapier/proxysql:v2.0.10-1'
          imagePullPolicy: Always
          lifecycle:
            preStop:
              exec:
                command:
                  - /bin/sh
                  - '-c'
                  - sleep 190
      ...
```

What it does is that whenever the pod gets a KILL signal from Kubernetes, it goes to `Terminating` status, stops receiving new connection requests, and executes the `preStop` script, and sleeps for 190s. This allows enough time for in flight connections to complete and close. After the sleep is over, the pod is terminated.

### Use a LoadBalancer service with Local ExternalTrafficPolicy

We'd prefer not to use any load balancer between ProxySQL pods and the applications. ProxySQL already adds a hop between the applications and the databases. A load balancer will add one more hop. We tried using weighted DNS records for load balancing, but soon gave up on that approach because of uncertainty of DNS TTLs at various layers, not having total control of managing an endpoint.

So, we chose to use a `LoadBalancer` type Service to expose ProxySQL to applications. However, the downside to default `LoadBalancer` type service is that the proxy for the service runs on all Kubernetes nodes, whether the node runs the pods backing the service. This means that many a times, a client connection will land up on a node which is not running the ProxySQL pod, and the kube-proxy on that node has to find out on which node a ProxySQL pod is running and route traffic to that node. This adds another hop. In this setup we had increased average latency between the applications and the database.

That said, setting `externalTrafficPolicy: Local` for a Kubernetes Service, we can tell Kubernetes to run the proxy for the service only on nodes which run the pods backing the service, ProxySQL pods, in this case. However, there's a shortcoming in this setup, as well. What we noticed during our benchmarks is that if a single ProxySQL pod is running on a node, and it gets rotated, the Load Balancer takes some time to detect that the node as unhealthy, and it keeps sending traffic to the node till then. We saw a number of connection timeouts and terminations during this small window. Having multiple pods running on a node helps us to get around this edge case. Using **PodDisruptionBudget**, we can ensure that at max, only 1 pod for ProxySQL deployment can be unavailable. So, during rotating ProxySQL pods, when a pod is uEven if a pod goes out during rotation, we have other pods running on that node. The load balancer still sees the node as healthy and we have no connection issues.

```YAML
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  labels:
    app: proxysql
    release: proxysql
  name: proxysql
  namespace: proxysql
spec:
  minReadySeconds: 30
  replicas: 10
  selector:
    matchLabels:
      app: proxysql
      release: proxysql
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: proxysql
        release: proxysql
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
              - matchExpressions:
                  - key: example.com/nodegroup
                    operator: In
                    values:
                      - proxysql
        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
            - podAffinityTerm:
                labelSelector:
                  matchExpressions:
                    - key: release
                      operator: In
                      values:
                        - proxysql
                topologyKey: kubernetes.io/hostname
              weight: 100
      containers:
        - env:
            - name: PROXYSQL_WORKDIR
              value: /proxysql
            - name: PROXYSQL_ADMIN_HOST
              value: 127.0.0.1
            - name: PROXYSQL_ADMIN_PORT
              value: '6032'
            - name: PROXYSQL_ADMIN_USERNAME
              value: admin
            - name: PROXYSQL_ADMIN_PASSWORD
              value: admin
            - name: PROXYSQL_MYSQL_THREADS
              value: '4'
            - name: PROXYSQL_MYSQL_STACKSIZE
              value: '1048576'
            - name: PROXYSQL_MYSQL_INTERFACES
              value: '0.0.0.0:3306;/proxysql/proxysql.sock'
            - name: PROXYSQL_SHUTDOWN_GRACE_PERIOD
              value: '120'
          image: 'zapier/proxysql:v2.0.10-1'
          imagePullPolicy: Always
          lifecycle:
            preStop:
              exec:
                command:
                  - /bin/sh
                  - '-c'
                  - sleep 190
          livenessProbe:
            exec:
              command:
                - mysqladmin
                - ping
                - '-h'
                - 127.0.0.1
                - '-P'
                - '6032'
                - '-uadmin'
                - '-padmin'
                - ping
            initialDelaySeconds: 10
            periodSeconds: 5
            timeoutSeconds: 3
          name: proxysql
          ports:
            - containerPort: 3306
          readinessProbe:
            exec:
              command:
                - sh
                - '-c'
                - >
                  eval $(cat $PROXYSQL_WORKDIR/secrets/secrets.env | sed
                  's/^/export /') && mysql -h 127.0.0.1 -P 3306
                  -u${app_db_primary_user} -p${app_db_primary_password} -e
                  'SELECT 1' 2> /dev/null && mysql -h 127.0.0.1 -P 3306
                  -u${app_db_replica_user} -p${app_db_replica_password} -e
                  'SELECT 1' 2> /dev/null
            initialDelaySeconds: 180
            periodSeconds: 5
            timeoutSeconds: 3
          resources:
            limits:
              memory: 600Mi
            requests:
              cpu: 1000m
              memory: 600Mi
          volumeMounts:
            - mountPath: /proxysql/conf
              name: proxysql-conf
            - mountPath: /proxysql/secrets
              name: proxysql-secrets
        - env:
            - name: PROXYSQL_ADMIN_USER
              value: admin
            - name: PROXYSQL_ADMIN_PASSWORD
              value: admin
            - name: DATADOG_HOST
              valueFrom:
                fieldRef:
                  fieldPath: status.hostIP
            - name: DATADOG_PORT
              value: '8125'
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: ENVIRONMENT
              value: production
            - name: RELEASE_NAME
              value: proxysql
          image: 'zapier/proxysql-datadog-metrics:v0.2.1'
          imagePullPolicy: Always
          name: proxysql-datadog-metrics
          resources:
            limits:
              memory: 128Mi
            requests:
              cpu: 100m
              memory: 128Mi
      terminationGracePeriodSeconds: 200
      tolerations:
        - effect: NoSchedule
          key: dedicated
          operator: Equal
          value: proxysql
      volumes:
        - configMap:
            name: proxysql-proxysql
          name: proxysql-conf
        - name: proxysql-secrets
          secret:
            secretName: proxysql-db-creds
```

```
apiVersion: v1
kind: Service
metadata:
  annotations:
    external-dns.alpha.kubernetes.io/aws-weight: '10'
    external-dns.alpha.kubernetes.io/hostname: >-
      proxysql.internal.zapier.com
    external-dns.alpha.kubernetes.io/set-identifier: production-01
    service.beta.kubernetes.io/aws-load-balancer-connection-draining-enabled: 'true'
    service.beta.kubernetes.io/aws-load-balancer-connection-draining-timeout: '190'
    service.beta.kubernetes.io/aws-load-balancer-connection-idle-timeout: '4000'
    service.beta.kubernetes.io/aws-load-balancer-cross-zone-load-balancing-enabled: 'true'
    service.beta.kubernetes.io/aws-load-balancer-healthcheck-healthy-threshold: '2'
    service.beta.kubernetes.io/aws-load-balancer-healthcheck-interval: '5'
    service.beta.kubernetes.io/aws-load-balancer-healthcheck-timeout: '3'
    service.beta.kubernetes.io/aws-load-balancer-healthcheck-unhealthy-threshold: '2'
    service.beta.kubernetes.io/aws-load-balancer-internal: 0.0.0.0/0
  labels:
    app: proxysql
  name: proxysql
  namespace: proxysql
spec:
  externalTrafficPolicy: Local
  ports:
      port: 3306
      protocol: TCP
      targetPort: 3306
  selector:
    app: proxysql
    release: proxysql
  type: LoadBalancer
```

```YAML
apiVersion: v1
data:
  proxysql_db_password: ++++++++
  proxysql_db_user: ++++++++
  secrets.env: ++++++++
  db_primary_endpoint: ++++++++
  db_primary_password: ++++++++
  db_primary_user: ++++++++
  db_replica_1_endpoint: ++++++++
  db_replica_2_endpoint: ++++++++
  db_replica_3_endpoint: ++++++++
  db_replica_endpoint: ++++++++
  db_replica_password: ++++++++
  db_replica_user: ++++++++
kind: Secret
metadata:
  name: proxysql-db-creds
  namespace: proxysql
type: Opaque
```

```
apiVersion: v1
data:
  proxysql.cnf.tpl: |-
    datadir="/var/lib/proxysql"

    admin_variables=
    {
          admin_credentials="${PROXYSQL_ADMIN_USERNAME}:${PROXYSQL_ADMIN_PASSWORD}"
          mysql_ifaces="${PROXYSQL_ADMIN_HOST}:${PROXYSQL_ADMIN_PORT}"
          refresh_interval=2000
    }

    mysql_variables=
    {
          threads=${PROXYSQL_MYSQL_THREADS}
          max_connections=6000
          default_query_delay=0
          default_query_timeout=60000
          have_compress=true
          poll_timeout=2000
          interfaces="${PROXYSQL_MYSQL_INTERFACES}"
          default_schema="information_schema"
          stacksize=${PROXYSQL_MYSQL_STACKSIZE}
          server_version="5.6.9"
          connect_timeout_server=1000
          connect_timeout_server_max=10000
          monitor_history=60000
          monitor_connect_interval=200000
          monitor_ping_interval=200000
          ping_interval_server_msec=10000
          ping_timeout_server=200
          commands_stats=true
          sessions_sort=true
          free_connections_pct=30
          autocommit_false_is_transaction=true
          monitor_username="${proxysql_db_user}"
          monitor_password="${proxysql_db_password}"
          enforce_autocommit_on_reads=true
    }
    mysql_servers =
    (
          { address="${db_v6_primary_endpoint}" , port=3306 , hostgroup=10, max_connections=120, status="ONLINE" },
          { address="${db_v6_replica_1_endpoint}" , port=3306 , hostgroup=20, max_connections=80, weight=100, status="ONLINE" },
          { address="${db_v6_replica_2_endpoint}" , port=3306 , hostgroup=20, max_connections=80, weight=100, status="ONLINE" }
    )
    mysql_users =
    (
          { username = "${db_primary_user}" , password = "${db_primary_password}" , default_hostgroup = 10 , active = 1 },
          { username = "${db_replica_user}" , password = "${db_replica_password}" , default_hostgroup = 20 , active = 1 }
    )
    mysql_query_rules =
    (
          {
                  rule_id=300
                  active=1
                  proxy_port=3306
                  username="${db_primary_user}"
                  destination_hostgroup=10
                  apply=1
                  log=1
          },
          {
                  rule_id=400
                  active=1
                  proxy_port=3306
                  username="${db_replica_user}"
                  destination_hostgroup=20
                  apply=1
                  log=1
          }
    )
kind: ConfigMap
metadata:
  labels:
    app: proxysql
    release: proxysql
  name: proxysql
  namespace: proxysql

```



