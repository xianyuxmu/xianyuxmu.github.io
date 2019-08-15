---
layout: post
title: Kubernetes Objects - Kubernetes
date: 2019-08-15 20:00:00 +0800
categories:
- Tech
tags:
- Kubernetes
- 2019年08月
---


# Kubernetes Objects

## Understanding Kubernetes Objects

from [Understanding Kubernetes Objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/common-labels/)

- Specifically, Kubernetes Objects can describe:
	- What containerized applications are running (and on which nodes)
	- The resources available to those applications
	- The policies around how those applications behave, such as restart policies, upgrades, and fault-tolerance
- **Object Spec and Status**:
	- The ***spec***, which you must provide, describes your desired state for the object–the characteristics that you want the object to have.
	- The ***status*** describes the actual state of the object, and is supplied and updated by the Kubernetes system.
-  Most often, you provide the information to kubectl in a `.yaml` file. `kubectl` converts the information to JSON when making the API request
- Required Fields: apiVersion, kind and metadata.


## Namespaces

from [Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)

- Kubernetes supports multiple virtual clusters backed by the same physical cluster. **These virtual clusters are called namespaces**.
- **When to Use Multiple Namespaces**
	- Namespaces are a way to divide cluster resources between multiple users.
	- Namespaces are intended for use in environments with many users spread across multiple teams, or projects. 
	- Namespaces are intended for use in environments with many users spread across multiple teams, or projects. 
- **Working with Namespaces**
	- default: The default namespace for objects with no other namespace.
	- kube-system: The namespace for objects created by the Kubernetes system.
	- kube-public: This namespace is created automatically and is readable by all users (including those not authenticated). 


## Labels and Selectors

from [Labels and Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)

- Labels are key/value pairs that are attached to objects, such as pods. Labels are intended to be used to specify identifying attributes of objects that are meaningful and relevant to users, but do not directly imply semantics to the core system. Labels can be used to organize and to select subsets of objects.
	- "release" : "stable"
	- "environment" : "dev"
	- "track" : "daily"
- **Label selectors**
	- Unlike names and UIDs, labels do not provide uniqueness. In general, we expect many objects to carry the same label(s).


## Annotations

from [Annotations](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/)

You can use Kubernetes annotations to attach arbitrary non-identifying metadata to objects. Clients such as tools and libraries can retrieve this metadata.

``` yaml
apiVersion: v1
kind: Pod
metadata:
  name: annotations-demo
  annotations:
    imageregistry: "https://hub.docker.com/"
spec:
  containers:
  - name: nginx
    image: nginx:1.7.9
    ports:
    - containerPort: 80
          
```


## Field Selectors

from [Field Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/field-selectors/)

``` bash
kubectl get pods --field-selector status.phase=Running
```


## Recommended Labels

Shared labels and annotations share a common prefix: `app.kubernetes.io`. 

Labels without a prefix are private to users. The shared prefix ensures that shared labels do not interfere with custom user labels.

``` bash
apiVersion: apps/v1
kind: StatefulSet
metadata:
  labels:
    app.kubernetes.io/name: mysql
    app.kubernetes.io/instance: wordpress-abcxzy
    app.kubernetes.io/version: "5.7.21"
    app.kubernetes.io/component: database
    app.kubernetes.io/part-of: wordpress
    app.kubernetes.io/managed-by: helm
```



