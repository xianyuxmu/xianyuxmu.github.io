---
layout: post
title: Workloads - Kubernetes
date: 2018-08-15 20:00:00 +0800
categories:
- Tech
tags:
- Kubernetes
- 2019年08月
---

# Workloads

## Pod

----

## Pod Overview

from [Pod Overview](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/)

- A Pod is the basic execution unit of a Kubernetes application–the smallest and simplest unit in the Kubernetes object model that you create or deploy. A Pod represents processes running on your `Cluster`.
- A Pod encapsulates an application’s container (or, in some cases, multiple containers), storage resources, a unique network IP, and options that govern how the container(s) should run.
- **Pods in a Kubernetes cluster can be used in two main ways:**
	- **Pods that run a single container.** The “one-container-per-Pod” model is the most common Kubernetes use case.
	- **Pods that run multiple containers that need to work together.**
- `replication` is generally referred in scaling your application.
- Pods provide two kinds of shared resources for their constituent containers: ***networking and storage***.
	- **Networking**: Each Pod is assigned a unique IP address. Every container in a Pod shares the network namespace, including the IP address and network ports. Containers inside a Pod can communicate with one another using `localhost`.
	- **Storage**: A Pod can specify a set of shared storage Volumes . All containers in the Pod can access the shared volumes, allowing those containers to share data.
- **Pods do not, by themselves, self-heal**. If a Pod is scheduled to a Node that fails, or if the scheduling operation itself fails, the Pod is deleted; likewise, a Pod won’t survive an eviction due to a lack of resources or Node maintenance. 
	- Kubernetes uses Controllers to implement Pod scaling and healing.
- **Pods and Controllers**
	- A Controller can create and manage multiple Pods for you, handling replication and rollout and providing self-healing capabilities at cluster scope.
- **Pod Templates**
	- Controllers use Pod Templates to make actual pods.


## Pods

from [Pods](https://kubernetes.io/docs/concepts/workloads/pods/pod/)

![A multi-container Pod](https://d33wubrfki0l68.cloudfront.net/aecab1f649bc640ebef1f05581bfcc91a48038c4/728d6/images/docs/pod.svg)

- Pods are the smallest deployable units of computing that can be created and managed in Kubernetes.
- A Pod (as in a pod of whales or pea pod) is a group of one or more containers (such as Docker containers), with shared storage/network, and a specification for how to run the containers.
- Containers within a Pod share an IP address and port space, and can find each other via `localhost`.
- As discussed in pod lifecycle, Pods are created, **assigned a unique ID (UID)**, and scheduled to nodes where they remain until termination (according to restart policy) or deletion.


### Alternatives considered

*Why not just run multiple programs in a single (Docker) container?*

- Transparency. 让Pod可以监控container
- Decoupling software dependencies/服务解耦
- Ease of use/便于使用
- Efficiency/高效。容器更加轻量。

### Durability of pods

- Pods aren’t intended to be treated as durable entities
- In general, users shouldn’t need to create Pods directly. For example, use Deployments, Controllers.


## Pod Lifecycle

from [Pod Lifecycle](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/)

- A PodSpec has a `restartPolicy` field with possible values *Always, OnFailure, and Never.*


## Init Containers

from [Init Containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)


Specialized containers that run before app containers in a `Pod`.

一个Wordpress网页服务，它可能依赖的**Init Containers**有: mysqldb。


## Pod Preset

from [Pod Preset](https://kubernetes.io/docs/concepts/workloads/pods/podpreset/)

A `Pod Preset` is an API resource for injecting additional runtime requirements into a Pod at creation time.


## Controllers

---

## ReplicaSet

from [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)

- A ReplicaSet’s purpose is to maintain a stable set of replica Pods running at any given time. *As such, it is often used to guarantee the availability of a specified number of identical Pods.*
- When a ReplicaSet needs to create new Pods, it uses its Pod template.

#### When to use a ReplicaSet

A ReplicaSet ensures that a specified number of pod replicas are running at any given time.

***We recommend using Deployments*** instead of directly using ReplicaSets, unless you require custom update orchestration or don’t require updates at all.

⚠️注意：如果有Pod的labels和ReplicaSet中的labels一样，则ReplicaSet会直接复用这些Pod。

``` yaml
controllers/frontend.yaml 

apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  # modify replicas according to your case
  replicas: 3
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: php-redis
        image: gcr.io/google_samples/gb-frontend:v3
```

![HPA](https://d33wubrfki0l68.cloudfront.net/4fe1ef7265a93f5f564bd3fbb0269ebd10b73b4e/1775d/images/docs/horizontal-pod-autoscaler.svg)

相关：Pod水平自动扩容——[Horizontal Pod Autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)

## ReplicationController

from [ReplicationController](https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/)

> Note: A `Deployment` that configures a `ReplicaSet` is now the recommended way to set up replication.

## Deployments

from [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

详细阐述了如何使用`Deployments`，详情直接看上面文档。


## StatefulSets

from [StatefulSets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)

StatefulSet is the workload API object used to manage stateful applications.

Like a Deployment , a StatefulSet manages Pods that are based on an identical container spec. Unlike a Deployment, a StatefulSet maintains a sticky identity for each of their Pods.

## DaemonSet

from [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)

A DaemonSet ensures that all (or some) Nodes run a copy of a Pod.

DaemonSets are similar to Deployments in that they both create Pods, and those Pods have processes which are not expected to terminate (e.g. web servers, storage servers).

Use a Deployment for stateless services, like frontends, where scaling up and down the number of replicas and rolling out updates are more important than controlling exactly which host the Pod runs on. Use a DaemonSet when it is important that a copy of a Pod always run on all or certain hosts, and when it needs to start before other Pods.

## Garbage Collection

from [Garbage Collection](https://kubernetes.io/docs/concepts/workloads/controllers/garbage-collection/)

The role of the Kubernetes garbage collector is to delete certain objects that once had an owner, but no longer have an owner.

Some Kubernetes objects are owners of other objects. For example, a ReplicaSet is the owner of a set of Pods. The owned objects are called dependents of the owner object. Every dependent object has a `metadata.ownerReferences` field that points to the owning object.

 Kubernetes automatically sets the `ownerReference` field of each Pod in the ReplicaSet.


## TTL Controller for Finished Resources

from [TTL Controller for Finished Resources](https://kubernetes.io/docs/concepts/workloads/controllers/ttlafterfinished/)

The TTL controller provides a TTL mechanism to limit the lifetime of resource objects that have finished execution. TTL controller only handles Jobs for now, and may be expanded to handle other resources that will finish execution, such as Pods and custom resources.

## Jobs - Run to Completion

from [Jobs - Run to Completion](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/)

A Job creates one or more Pods and ensures that a specified number of them successfully terminate. 

Deleting a Job will clean up the Pods it created.

Example:

``` yaml
controllers/job.yaml 

apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  template:
    spec:
      containers:
      - name: pi
        image: perl
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
  backoffLimit: 4
```

## CronJob

from [CronJob](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)

A Cron Job creates Jobs on a time-based schedule.


