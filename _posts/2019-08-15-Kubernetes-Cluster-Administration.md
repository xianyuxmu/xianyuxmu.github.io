---
layout: post
title: Cluster Administration - Kubernetes
date: 2018-08-15 20:00:00 +0800
categories:
- Tech
tags:
- Kubernetes
- 2019年08月
---


# Cluster Administration

## Cluster Administration Overview

from [Cluster Administration Overview](https://kubernetes.io/docs/concepts/cluster-administration/cluster-administration-overview/)

自己看文档，主要包含下面四个主题的导航🧭：

* Planning a cluster
* Managing a cluster
* Securing a cluster
* Optional Cluster Services

----


## Certificates

from [Certificates](https://kubernetes.io/docs/concepts/cluster-administration/certificates/)

证书生成、授权与其管理。

----

## Cloud Providers

from [Cloud Providers](https://kubernetes.io/docs/concepts/cluster-administration/cloud-providers/)

**[kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm/)** is a popular option for creating kubernetes clusters. kubeadm has configuration options to specify configuration information for cloud providers.

----


## Managing Resources

from [Managing Resources](https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/)

如何管理集群，主要关于下面几个专题：

* Organizing resource configurations
* Bulk operations in kubectl
* Using labels effectively
* Canary deployments
* Updating labels
* Updating annotations
* Scaling your application
* In-place updates of resources
* Disruptive updates
* Updating your application without a service outage

----

## Cluster Networking

from [Cluster Networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/)

集群网络相关，自己看文档。

----

## Logging Architecture

from [Logging Architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)

集群日志，直接看文档。

----

## Configuring kubelet Garbage Collection

from [Configuring kubelet Garbage Collection](https://kubernetes.io/docs/concepts/cluster-administration/kubelet-garbage-collection/)

Garbage collection is a helpful function of kubelet that will clean up unused images and unused containers. Kubelet will perform garbage collection for containers every minute and garbage collection for images every five minutes.

----

## Proxies in Kubernetes

from [Proxies in Kubernetes](https://kubernetes.io/docs/concepts/cluster-administration/proxies/)

直接看文档。

----

## Controller manager metrics

from [Controller manager metrics](https://kubernetes.io/docs/concepts/cluster-administration/controller-metrics/)

Controller manager metrics provide important insight into the performance and health of the controller manager. 

---

## Installing Addons

from [Installing Addons](https://kubernetes.io/docs/concepts/cluster-administration/addons/)

Add-ons extend the functionality of Kubernetes.

各种插件。

----

