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

## Kubernetes Basic

detail: [Kubernetes Components](https://kubernetes.io/docs/concepts/overview/components/)

### kubelet

- kubelet is the primary node agent. It watches for pods that have been assigned to its node (either by apiserver or via local configuration file).

### Clustering etcd

On each node, copy the `etcd.yaml` file into `/etc/kubernetes/manifests/etcd.yaml`

### NodePort

- 编辑一个`service`： `kubectl -n kube-system edit service kubernetes-dashboard`

### kubectl proxy

- `kubectl proxy` creates proxy server between your machine and Kubernetes API server. By default it is only accessible locally (from the machine that started it).
	- To access ***HTTPS*** endpoint of dashboard go to: `http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/`
	- To access ***HTTP*** endpoint of dashboard go to: `http://localhost:8001/api/v1/namespaces/kube-system/services/http:kubernetes-dashboard:/proxy/`


## 相关教程

### Kubernetes Dashboard

- [Accessing Dashboard 1.7.X and above](https://github.com/kubernetes/dashboard/wiki/Accessing-Dashboard---1.7.X-and-above)

### 自建 Docker Registry

- [Deploy a registry server](https://docs.docker.com/registry/deploying/)
 - [Sharing a local registry with minikube](https://blog.hasura.io/sharing-a-local-registry-for-minikube-37c7240d0615)



----

## 相关资料

- etcd
	- [etcd - GitHub](https://github.com/coreos/etcd)
	- etcd is a distributed reliable key-value store for the most critical data of a distributed system.
	- etcd is used as Kubernetes’ backing store. All cluster data is stored here. Always have a backup plan for etcd’s data for your Kubernetes cluster.
- [Building High-Availability Clusters](https://kubernetes.io/docs/admin/high-availability/) - We choose to use the kubelet that we run on each of the worker nodes.
	- Clustered etcd already replicates your storage to all master instances in your cluster. This means that to lose data, all three nodes would need to have their physical (or virtual) disks fail at the same time.
	- ![High-Availability Clusters](https://d33wubrfki0l68.cloudfront.net/2555d34e3008aab4b049ca5634cfabc2078ccf92/3269a/images/docs/ha.svg)
- [Tools for Monitoring Compute, Storage, and Network Resources](https://kubernetes.io/docs/tasks/debug-application-cluster/resource-usage-monitoring/)
- [Learn the Kubernetes Key Concepts in 10 Minutes](http://omerio.com/2015/12/18/learn-the-kubernetes-key-concepts-in-10-minutes/)
	- [十分钟带你理解Kubernetes核心概念](http://dockone.io/article/932)
	- [Getting Started with Kubernetes on Google Container Engine](http://omerio.com/2016/01/02/getting-started-with-kubernetes-on-google-container-engine/)
