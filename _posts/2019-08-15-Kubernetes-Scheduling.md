---
layout: post
title: Scheduling - Kubernetes
date: 2019-08-15 20:00:00 +0800
categories:
- Tech
tags:
- Kubernetes
- 2019年08月
---

# Scheduling

## Kubernetes Scheduler

from [Kubernetes Scheduler](https://kubernetes.io/docs/concepts/scheduling/kube-scheduler/)

**In Kubernetes, scheduling refers to making sure that Pods are matched to Nodes so that Kubelet can run them.**

A scheduler watches for newly created Pods that have no Node assigned. For every Pod that the scheduler discovers, the scheduler becomes responsible for finding the best Node for that Pod to run on. 

----

## Scheduler Performance Tuning

from [Scheduler Performance Tuning](https://kubernetes.io/docs/concepts/scheduling/scheduler-perf-tuning/)

自己看。