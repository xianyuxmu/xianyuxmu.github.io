---
layout: post
title: Kubenetes Architecture - Kubernetes
date: 2019-08-15 20:00:00 +0800
categories:
- Tech
tags:
- Kubernetes
- 2019年08月
---


# Kubenetes Architecture

## Nodes

from [Nodes](https://kubernetes.io/docs/concepts/architecture/nodes/)

- A node is a worker machine in Kubernetes, previously known as a `minion`.
- **Node Status**
	- Addresses: 
		- HostName: The hostname as reported by the node’s kernel.
		- ExternalIP: Typically the IP address of the node that is externally routable (available from outside the cluster).
		- InternalIP: Typically the IP address of the node that is routable only within the cluster.
	- Conditions: The `conditions` field describes the status of all `Running` nodes
		- OutOfDisk\Ready\MemoryPressure\NetworkUnavailable
	- **Capacity and Allocatable**:
		- Describes the resources available on the node: CPU, memory and the maximum number of pods that can be scheduled onto the node.
		- The fields in the capacity block indicate the total amount of resources that a Node has.
	- Info: General information about the node, such as kernel version, Kubernetes version...
- **Management**
	- Unlike pods and services, a node is not inherently created by Kubernetes: it is created externally by cloud providers like Google Compute Engine.
	- **Node Controller**
		- The node controller is a Kubernetes master component which manages various aspects of nodes.
		- The first is assigning a CIDR block to the node when it is registered (if CIDR assignment is turned on)
		- The second is keeping the node controller’s internal list of nodes up to date with the cloud provider’s list of available machines.
		- The third is monitoring the nodes’ health.
	- **Self-Registration of Nodes**
		- When the kubelet flag `--register-node` is true (the default), the kubelet will attempt to register itself with the API server.
- **Manual Node Administration**
	- A cluster administrator can create and modify node objects.
	- **Node capacity**: 
		- The capacity of the node (number of cpus and amount of memory) is part of the node object. Normally, nodes register themselves and report their capacity when creating the node object. 
		- The Kubernetes scheduler ensures that there are enough resources for all the pods on a node. 


## Master-Node communication

from [Master-Node communication](https://kubernetes.io/docs/concepts/architecture/master-node-communication/)


- **Cluster to Master**
	- All communication paths from the cluster to the master terminate at the apiserver (none of the other master components are designed to expose remote services). In a typical deployment, the apiserver is configured to listen for remote connections on a secure HTTPS port (443) with one or more forms of client authentication enabled. 
	- Nodes should be provisioned with the public root certificate for the cluster such that they can connect securely to the apiserver along with valid client credentials.
	- The master components also communicate with the cluster apiserver over the secure port.
- **Master to Cluster**
	- There are two primary communication paths from the master (apiserver) to the cluster. The first is from the apiserver to the kubelet process which runs on each node in the cluster. The second is from the apiserver to any node, pod, or service through the apiserver’s proxy functionality.
	- apiserver to kubelet:
		- Fetching logs for pods.
		- Providing the kubelet’s port-forwarding functionality.
		- These connections terminate at the kubelet’s HTTPS endpoint.
		- By default, the apiserver does not verify the kubelet’s serving certificate.
	- apiserver to nodes, pods, and services:
		- The connections from the apiserver to a node, pod, or service default to plain HTTP connections and are therefore neither authenticated nor encrypted.
- **SSH Tunnels**
	- Kubernetes supports SSH tunnels to protect the Master -> Cluster communication paths.
	- In this configuration, the apiserver initiates an SSH tunnel to each node in the cluster (connecting to the ssh server listening on port 22) and passes all traffic destined for a kubelet, node, pod, or service through the tunnel. 
	- SSH tunnels are ***currently deprecated*** so you shouldn’t opt to use them unless you know what you are doing.


## Concepts Underlying the Cloud Controller Manager

from [Concepts Underlying the Cloud Controller Manager](https://kubernetes.io/docs/concepts/architecture/cloud-controller/)

![the architecture of a Kubernetes cluster without the cloud controller manager](https://d33wubrfki0l68.cloudfront.net/e298a92e2454520dddefc3b4df28ad68f9b91c6f/70d52/images/docs/pre-ccm-arch.png)

the architecture of a Kubernetes cluster without the cloud controller manager

![the architecture of a Kubernetes cluster with the cloud controller manager](https://d33wubrfki0l68.cloudfront.net/518e18713c865fe67a5f23fc64260806d72b38f5/61d75/images/docs/post-ccm-arch.png)

The cloud controller manager (CCM) concept (not to be confused with the binary) was originally created to allow cloud specific vendor code and the Kubernetes core to evolve independent of one another.
