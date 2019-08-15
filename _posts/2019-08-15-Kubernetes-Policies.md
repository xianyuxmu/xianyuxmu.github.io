---
layout: post
title: Policies - Kubernetes
date: 2018-08-15 20:00:00 +0800
categories:
- Tech
tags:
- Kubernetes
- 2019年08月
---


# Policies

## Limit Ranges

from [Limit Ranges](https://kubernetes.io/docs/concepts/policy/limit-range/)

By default, containers run with unbounded compute resources on a Kubernetes cluster. There is a concern that one Pod or Container could monopolize all of the resources. Limit Range is a policy to constrain resource by Pod or Container in a namespace.

### Limiting Pod compute resources

limit-mem-cpu-pod.yaml:

``` yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: limit-mem-cpu-per-pod
spec:
  limits:
  - max:
      cpu: "2"
      memory: "2Gi"
    type: Pod
```

### Limiting Storage resources

``` yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: storagelimits
spec:
  limits:
  - type: PersistentVolumeClaim
    max:
      storage: 2Gi
    min:
      storage: 1Gi
```

### Limits/Requests Ratio

``` yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: limit-memory-ratio-pod
spec:
  limits:
  - maxLimitRequestRatio:
      memory: 2
    type: Pod
```

----

## Resource Quotas

from [Resource Quotas](https://kubernetes.io/docs/concepts/policy/resource-quotas/)

When several users or teams share a cluster with a fixed number of nodes, there is a concern that one team could use more than its fair share of resources.

Resource quotas are a tool for administrators to address this concern.

`ResourceQuota` demo: 

``` yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: object-counts
spec:
  hard:
    configmaps: "10"
    persistentvolumeclaims: "4"
    replicationcontrollers: "20"
    secrets: "10"
    services: "10"
    services.loadbalancers: "2"
```

----

## Pod Security Policies

from [Pod Security Policies](https://kubernetes.io/docs/concepts/policy/pod-security-policy/)

A Pod Security Policy is a cluster-level resource that controls security sensitive aspects of the pod specification. The `PodSecurityPolicy` objects define a set of conditions that a pod must run with in order to be accepted into the system, as well as defaults for the related fields.




