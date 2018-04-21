---
layout: post
title: 容器化DevOps
date: 2018-04-14 15:57:00 +0800
categories:
- Tech
tags:
- 容器
- DevOps

---

# 环境初始化

## Docker

### 基础环境搭建

- 安装Docker：[Install Docker](https://docs.docker.com/install/)

## Kubernetes

### 基础环境搭建

- 安装Minikube：[Install Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)
- 安装kubectl：[Install and Set Up kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- 安装skaffold：[skaffold](https://github.com/GoogleCloudPlatform/skaffold#demo)
	- [Iterative Development](https://github.com/GoogleCloudPlatform/skaffold#iterative-development)
- [helm - The Kubernetes Package Manager](https://github.com/kubernetes/helm)

### kubernetes常见问题

- [Pull an Image from a Private Registry](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/)
- [Kubernetes从Private Registry中拉取容器镜像的方法](https://tonybai.com/2016/11/16/how-to-pull-images-from-private-registry-on-kubernetes-cluster/)
	- 通过secret yaml文件创建pull image所用的secret

----

# 相关资料

- kubernetes
	- kubernetes集群搭建：
		- [kubespray](https://github.com/kubernetes-incubator/kubespray) - Deploy a Production Ready Kubernetes Cluster
		- [阿里云快速部署Kubernetes - VPC环境](https://yq.aliyun.com/articles/66474)
	- [kubernetes/kubernetes](https://github.com/kubernetes/kubernetes/tree/master/examples)
- Docker
	- [Docker学习路线图](https://yq.aliyun.com/articles/40494?spm=a2c4e.11153959.teamhomeleft.23.3f4218b1FHkfIc)
	- [Use multi-stage builds](https://docs.docker.com/develop/develop-images/multistage-build/)
	- [Alpine Based Docker Images Make a Difference in Real World Apps](https://blog.codeship.com/alpine-based-docker-images-make-difference-real-world-apps/)
