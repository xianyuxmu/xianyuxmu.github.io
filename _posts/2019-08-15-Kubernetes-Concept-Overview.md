---
layout: post
title: Concept Overview - Kubernetes
date: 2018-08-15 20:00:00 +0800
categories:
- Tech
tags:
- Kubernetes
- 2019年08月
---



# Concept Overview

## What is Kubernetes

学习内容：[What is Kubernetes](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)

Kubernetes的优势：

- Service discovery and load balancing/服务发现、负载均衡
- Storage orchestration/存储编排
- Automated rollouts and rollbacks/自动部署和回滚
- Automatic bin packing/自动包装
- Self-healing/自我修复
- Secret and configuration management/密钥和配置管理
- Horizontal scaling/水平伸缩

## Kubernetes Components

from [Kubernetes Components](https://kubernetes.io/docs/concepts/overview/components/)

- **Master Components**
	- Master components provide the cluster’s control plane.
	- Typically, do not run user containers on this machine.
	- etcd: Consistent and highly-available key value store used as Kubernetes’ backing store for all cluster data.
	- kube-scheduler: Component on the master that watches newly created pods that have no node assigned, and selects a node for them to run on.
- **Node Components**
	- Node components run on every node, maintaining running pods and providing the Kubernetes runtime environment.
	- kubelet: An agent that runs on each node in the cluster. It makes sure that containers are running in a pod.
	- kube-proxy: 
		- kube-proxy maintains network rules on nodes. 
		- kube-proxy is a network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept.
	- Container Runtime: The container runtime is the software that is responsible for running containers.
- **Addons**
	- Addons use Kubernetes resources (DaemonSet , Deployment , etc) to implement cluster features. Because these are providing cluster-level features, namespaced resources for addons belong within the `kube-system` namespace.
	- DNS: Cluster DNS is a DNS server, in addition to the other DNS server(s) in your environment, which serves DNS records for Kubernetes services.
	- Web UI (Dashboard): Dashboard is a general purpose, web-based UI for Kubernetes clusters
	- Container Resource Monitoring
	- Cluster-level Logging


