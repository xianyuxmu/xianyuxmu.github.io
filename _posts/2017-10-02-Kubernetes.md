---
layout: post
title: Kubernetes
date: 2017-10-02 09:56:00 +0800
categories:
- Tech
tags:
- Kubernetes

---


In a Kubernetes cluster, application performance can be examined at many different levels: ***containers, pods, services, and whole clusters***.

## Kubernetes Components

detail: [Kubernetes Components](https://kubernetes.io/docs/concepts/overview/components/)

### kubelet

- kubelet is the primary node agent. It watches for pods that have been assigned to its node (either by apiserver or via local configuration file).

### Clustering etcd

On each node, copy the `etcd.yaml` file into `/etc/kubernetes/manifests/etcd.yaml`

## 相关资料

- etcd
	- [etcd - GitHub](https://github.com/coreos/etcd)
	- etcd is a distributed reliable key-value store for the most critical data of a distributed system.
	- etcd is used as Kubernetes’ backing store. All cluster data is stored here. Always have a backup plan for etcd’s data for your Kubernetes cluster.
- [Building High-Availability Clusters](https://kubernetes.io/docs/admin/high-availability/) - We choose to use the kubelet that we run on each of the worker nodes.
	- Clustered etcd already replicates your storage to all master instances in your cluster. This means that to lose data, all three nodes would need to have their physical (or virtual) disks fail at the same time.
	- ![High-Availability Clusters](https://d33wubrfki0l68.cloudfront.net/2555d34e3008aab4b049ca5634cfabc2078ccf92/3269a/images/docs/ha.svg)
- [Tools for Monitoring Compute, Storage, and Network Resources](https://kubernetes.io/docs/tasks/debug-application-cluster/resource-usage-monitoring/)
