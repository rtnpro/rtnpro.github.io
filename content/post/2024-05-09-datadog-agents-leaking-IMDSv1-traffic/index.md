---
title: "Why do I still see METADATA_NO_TOKEN_REJECTED from IMDSv2 EKS nodes?"
date: 2024-05-09T13:18:47+05:30
draft: true
---

# Introduction

If you are running AWS EKS clusters with kube2iam, Datadog agents, and other
cluster components installed, and if you have migrated your EKS nodes from
IMDSv1 to IMDSv2, and if you still see `METADATA_NO_TOKEN_REJECTED` metrics
for your IMDSv2 EKS nodes, then you are in the right place.

Environment

# What's done so far?

- You have already updated your applications running on EKS to use latest AWS SDKs
  as recommended [here]
  (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/use-a-supported-sdk-version-for-imdsv2.html)
- You have updated all cluster components or addons like Datadog, kube2iam,
  aws-ebs-csi-driver, aws-node, etc. to latest versions with IMDSv2 support
- You have migrated EKS nodes from IMDSv1 to IMDSv2


# Observations

- `METADATA_NO_TOKEN` metrics for your EKS cluster have been drained to 0 {{<figure_webp src="Screen Shot 2024-05-10 at 11.32.53 AM.png">}}
- Cluster components like `kube2iam`, `datadog`, `aws-ebs-csi-driver`, etc. are working without any errors
- Applications on EKS cluster are able to access AWS resources and there are no errors related to IAM
- But, now you see a few `METADATA_NO_TOKEN_REJECTED` events coming from the IMDSv2 EKS nodes {{<figure_webp src="Screen Shot 2024-05-10 at 11.29.23 AM.png">}}

# Investigation

We ran aws-imds-packet-analyzer as a service on two sample EKS nodes:
- one with business applications installed
- one tainted node without business applications installed, but only cluster components running on it

```
[2024-05-06 11:21:26,127] [WARNING] IMDSv1(!) (pid:yyyyyyy:agent argv:agent run) called by -> (pid:zzzzzzz:containerd-shim argv:/usr/bin/containerd-shim-runc-v2 -namespace k8s.io -id <datadog agent container id> -address /run/containerd/containerd.sock) -> (pid:1:systemd argv:/usr/lib/systemd/systemd ...) Req details: PUT /instance_identity/v1/token?version=2022-03-08 HTTP/1.1, Host: 169.254.169.254, User-Agent: Go-http-client/1.1, Content-Length: 20, Metadata-Flavor: ibm, Accept-Encoding: gzip, , {"expires_in": 3600}
[2024-05-06 11:21:26,128] [WARNING] IMDSv1(!) (pid:4467:kube2iam argv:kube2iam --host-interface=eni+ --node=ip-xx-xx-xx-xx.ec2.internal --host-ip=xx.xx.xx.xx --iptables=true --auto-discover-base-arn --auto-discover-default-role=true --app-port=xxxx --metrics-port=xxxx) called by -> (pid:xxxx:containerd-shim argv:/usr/bin/containerd-shim-runc-v2 -namespace k8s.io -id <kube2iam container id> -address /run/containerd/containerd.sock) -> (pid:1:systemd argv:/usr/lib/systemd/systemd ...) Req details: PUT /instance_identity/v1/token?version=2022-03-08 HTTP/1.1, Host: 169.254.169.254, User-Agent: Go-http-client/1.1, Content-Length: 20, Accept-Encoding: gzip, Metadata-Flavor: ibm, X-Forwarded-For: 10.80.2.250,
[2024-05-06 11:21:26,128] [WARNING] IMDSv1(!) (pid:4467:kube2iam argv:kube2iam --host-interface=eni+ --node=ip-xx-xx-xx-xx.ec2.internal --host-ip=xx.xx.xx.xx --iptables=true --auto-discover-base-arn --auto-discover-default-role=true --app-port=xxxx --metrics-port=xxxx) called by -> (pid:xxxx:containerd-shim argv:/usr/bin/containerd-shim-runc-v2 -namespace k8s.io -id <kube2iam container id> -address /run/containerd/containerd.sock) -> (pid:1:systemd argv:/usr/lib/systemd/systemd ...) Req details: GET /metadata/instance/compute/vmId?api-version=2017-04-02&format=text HTTP/1.1, Host: 169.254.169.254, User-Agent: Go-http-client/1.1, Accept-Encoding: gzip, Metadata: true, X-Forwarded-For: 10.80.2.250,
[2024-05-06 11:21:26,128] [WARNING] IMDSv1(!) (pid:4467:kube2iam argv:kube2iam --host-interface=eni+ --node=ip-xx-xx-xx-xx.ec2.internal --host-ip=xx.xx.xx.xx --iptables=true --auto-discover-base-arn --auto-discover-default-role=true --app-port=xxxx --metrics-port=xxxx) called by -> (pid:xxxx:containerd-shim argv:/usr/bin/containerd-shim-runc-v2 -namespace k8s.io -id <kube2iam container id> -address /run/containerd/containerd.sock) -> (pid:1:systemd argv:/usr/lib/systemd/systemd ...) Req details: GET /computeMetadata/v1/instance/hostname HTTP/1.1, Host: 169.254.169.254, User-Agent: Go-http-client/1.1, Accept-Encoding: gzip, Metadata-Flavor: Google, X-Forwarded-For: 10.80.2.250,
[2024-05-06 11:21:26,128] [WARNING] IMDSv1(!) (pid:yyyyyyy:agent argv:agent run) called by -> (pid:zzzzzzz:containerd-shim argv:/usr/bin/containerd-shim-runc-v2 -namespace k8s.io -id <datadog agent container id> -address /run/containerd/containerd.sock) -> (pid:1:systemd argv:/usr/lib/systemd/systemd ...) Req details: GET /computeMetadata/v1/instance/name HTTP/1.1, Host: 169.254.169.254, User-Agent: Go-http-client/1.1, Metadata-Flavor: Google, Accept-Encoding: gzip,
[2024-05-06 11:21:26,129] [WARNING] IMDSv1(!) (pid:4467:kube2iam argv:kube2iam --host-interface=eni+ --node=ip-xx-xx-xx-xx.ec2.internal --host-ip=xx.xx.xx.xx --iptables=true --auto-discover-base-arn --auto-discover-default-role=true --app-port=xxxx --metrics-port=xxxx) called by -> (pid:xxxx:containerd-shim argv:/usr/bin/containerd-shim-runc-v2 -namespace k8s.io -id <kube2iam container id> -address /run/containerd/containerd.sock) -> (pid:1:systemd argv:/usr/lib/systemd/systemd ...) Req details: GET /computeMetadata/v1/instance/name HTTP/1.1, Host: 169.254.169.254, User-Agent: Go-http-client/1.1, Accept-Encoding: gzip, Metadata-Flavor: Google, X-Forwarded-For: 10.80.2.250,
[2024-05-06 11:21:26,432] [WARNING] IMDSv1(!) (pid:yyyyyyy:agent argv:agent run) called by -> (pid:zzzzzzz:containerd-shim argv:/usr/bin/containerd-shim-runc-v2 -namespace k8s.io -id <datadog agent container id> -address /run/containerd/containerd.sock) -> (pid:1:systemd argv:/usr/lib/systemd/systemd ...) Req details: GET /computeMetadata/v1/?recursive=true HTTP/1.1, Host: 169.254.169.254, User-Agent: Go-http-client/1.1, Metadata-Flavor: Google, Accept-Encoding: gzip,
[2024-05-06 11:21:26,433] [WARNING] IMDSv1(!) (pid:4467:kube2iam argv:kube2iam --host-interface=eni+ --node=ip-xx-xx-xx-xx.ec2.internal --host-ip=xx.xx.xx.xx --iptables=true --auto-discover-base-arn --auto-discover-default-role=true --app-port=xxxx --metrics-port=xxxx) called by -> (pid:xxxx:containerd-shim argv:/usr/bin/containerd-shim-runc-v2 -namespace k8s.io -id <kube2iam container id> -address /run/containerd/containerd.sock) -> (pid:1:systemd argv:/usr/lib/systemd/systemd ...) Req details: GET /computeMetadata/v1/?recursive=true HTTP/1.1, Host: 169.254.169.254, User-Agent: Go-http-client/1.1, Accept-Encoding: gzip, Metadata-Flavor: Google, X-Forwarded-For: 10.80.2.250,
[2024-05-06 16:40:09,631] [WARNING] IMDSv1(!) (pid:1037187:agent argv:agent run) called by -> (pid:1036883:containerd-shim argv:/usr/bin/containerd-shim-runc-v2 -namespace k8s.io -id 7f889c63eaf7a16e0406f4bdc268d71147f5968de392f411940054f6a4098d50 -address /run/containerd/containerd.sock) -> (pid:1:systemd argv:/usr/lib/systemd/systemd ...) Req details: PUT /instance_identity/v1/token?version=2022-03-08 HTTP/1.1, Host: 169.254.169.254, User-Agent: Go-http-client/1.1, Content-Length: 20, Metadata-Flavor: ibm, Accept-Encoding: gzip, , {"expires_in": 3600}
[2024-05-06 16:40:09,632] [WARNING] IMDSv1(!) (pid:1029164:kube2iam argv:kube2iam --host-interface=eni+ --node=ip-xx-xx-xx-xx.ec2.internal --host-ip=xx.xx.xx.xx --iptables=true --auto-discover-base-arn --auto-discover-default-role=true --app-port=xxxx --metrics-port=xxxx) called by -> (pid:1029087:containerd-shim argv:/usr/bin/containerd-shim-runc-v2 -namespace k8s.io -id <kube2iam container id> -address /run/containerd/containerd.sock) -> (pid:1:systemd argv:/usr/lib/systemd/systemd ...) Req details: PUT /instance_identity/v1/token?version=2022-03-08 HTTP/1.1, Host: 169.254.169.254, User-Agent: Go-http-client/1.1, Content-Length: 20, Accept-Encoding: gzip, Metadata-Flavor: ibm, X-Forwarded-For: xx.xx.xx.xx,
```

For both the nodes, we saw IMDSv1 `WARNING` logs from `datadog` agent and `kube2iam` containers. On a close look,
the logs for `kube2iam` container was about `kube2iam` forwarding requests from `datadog` agent container.

# Fixing attempts

- We added `EC2_PREFER_IMDSv2` env variable to Datadog pods' containers, but we saw no improvement in
- We added `AWS_EC2_METADATA_V1_DISABLED` env variable in Datadog pods' containers to disable IMDSv1 queries at `aws_sdk_go_v2` level, but it also did not seem to help

- We tried using IRSA for datadog cluster-agent and agent pods as recommended here: https://github.com/DataDog/helm-charts/issues/1073#issuecomment-1915337602, but still the IMDSv1 requests won't stop

# The final revelation

After searching for IMDSv2 in Datadog agent's release logs, I stumbled across 7.18.0 release
which added support for IMDSv2.
https://github.com/DataDog/datadog-agent/releases/tag/7.18.0

The release notes mention the following

```
Add support for the EC2 instance metadata service (IMDS) v2 that
requires to get a token before any metadata query. The agent will
still issue unauthenticated request first (IMDS v1) before switching
to token-based authentication (IMDS v2) if it fails.
```

The reason we still see IMDSv1 requests originating from Datadog agent containers and getting rejected,
with Datadog agents on IMDSv2 nodes perfectly working is because the agents tries to use IMDSv1 by default
and fallsback to IMDSv2 on error.