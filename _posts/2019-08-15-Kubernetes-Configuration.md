---
layout: post
title: Configuration - Kubernetes
date: 2018-08-15 20:00:00 +0800
categories:
- Tech
tags:
- Kubernetes
- 2019年08月
---



# Configuration

## Configuration Best Practices

from [Configuration Best Practices](https://kubernetes.io/docs/concepts/configuration/overview/)

一些好的实践建议，自己看。

----


## Managing Compute Resources for Containers

from [Managing Compute Resources for Containers](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/)

### Resource types

**CPU and memory are each a resource type**. A resource type has a base unit. CPU is specified in units of cores, and memory is specified in units of bytes.


内存单位:

- M: 1000*1000kb
- Mi: 1024*1024kb

### How Pods with resource requests are scheduled

When you create a Pod, the Kubernetes scheduler selects a node for the Pod to run on.

he resource usage of a Pod is reported as part of the Pod status.

----


## Assigning Pods to Nodes

from [Assigning Pods to Nodes](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/)

You can constrain a Pod to only be able to run on particular Node(s) , or to prefer to run on particular nodes. 

### nodeSelector

`nodeSelector` is the simplest recommended form of node selection constraint.

``` yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  nodeSelector:
    disktype: ssd
```

其他的还有节点类同、跨区域、节点反类同等部署Pod到Node的方式。

----

## Taints and Tolerations

from [Taints and Tolerations](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)

***Taints and tolerations work together to ensure that pods are not scheduled onto inappropriate nodes.***

有些节点有GPU等特殊的配置，需要匹配配置才能发到这个节点。

----

## Secrets

from [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)

Kubernetes `secret` objects let you store and manage sensitive information, such as passwords, OAuth tokens, and ssh keys.

A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key.

To use a secret, a pod needs to reference the secret. A secret can be used with a pod in two ways: ***as files in a volume mounted on one or more of its containers, or used by kubelet when pulling images for the pod.***

### Creating a Secret

创建直接看文档。


### Using Secrets

#### Using Secrets as Files from a Pod

配置位置: `.spec.volumes[].secret.secretName`

Each secret you want to use needs to be referred to in `.spec.volumes`.

#### Using Secrets as Environment Variables

配置位置: `env[].valueFrom.secretKeyRef`


### Using imagePullSecrets

An imagePullSecret is a way to pass a secret that contains a Docker (or other) image registry password to the Kubelet so it can pull a private image on behalf of your Pod.

### Use cases

- SSH Key
- Prod/test Credential

----


## Organizing Cluster Access Using kubeconfig Files

from [Organizing Cluster Access Using kubeconfig Files](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/)

没啥关键点，自己看文档。

----

## Pod Priority and Preemption

from [Pod Priority and Preemption](https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/)

Pod优先级相关。

----

## Scheduling Framework

from [Scheduling Framework](https://kubernetes.io/docs/concepts/configuration/scheduling-framework/)

调度框架，直接看文档。

----


