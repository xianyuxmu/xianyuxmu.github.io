---
layout: post
title: Container - Kubernetes
date: 2018-08-15 20:00:00 +0800
categories:
- Tech
tags:
- Kubernetes
- 2019年08月
---


# Container

## Images/镜像

from [Images](https://kubernetes.io/docs/concepts/containers/images/)

- The default pull policy is `IfNotPresent` which causes the Kubelet to skip pulling an image if it already exists.
- Docker Manifests: 不同平台下加载不同的镜像。详细说明: [docker的manifest特性](http://reborncodinglife.com/2019/05/24/docker-manifest-feature/)

----


## Container Environment Variables

from [Container Environment Variables](https://kubernetes.io/docs/concepts/containers/container-environment-variables/)

- **Container environment**
	- The Kubernetes Container environment provides several important resources to Containers:
		- A filesystem
		- Information about the Container itself.
		- Information about other objects in the cluster.
	- The `hostname` of a Container is the name of the Pod in which the Container is running.

----


## Runtime Class

from [Runtime Class](https://kubernetes.io/docs/concepts/containers/runtime-class/)

RuntimeClass is a feature for selecting the container runtime configuration. The container runtime configuration is used to run a Pod’s containers.

----	


## Container Lifecycle Hooks

from [Container Lifecycle Hooks](https://kubernetes.io/docs/concepts/containers/container-lifecycle-hooks/)

Analogous to many programming language frameworks that have component lifecycle hooks, such as Angular, Kubernetes provides Containers with lifecycle hooks. The hooks enable Containers to be aware of events in their management lifecycle and run code implemented in a handler when the corresponding lifecycle hook is executed.
