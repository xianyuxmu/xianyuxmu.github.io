---
layout: post
title: Storage - Kubernetes
date: 2019-08-15 20:00:00 +0800
categories:
- Tech
tags:
- Kubernetes
- 2019年08月
---

# Storage

## Volumes

from [Volumes](https://kubernetes.io/docs/concepts/storage/volumes/)

The Kubernetes Volume abstraction solves both of these problems：

- Container starts with a clean state without On-disk files
- It is often necessary to share files between those Containers

A Kubernetes volume, on the other hand, has an explicit lifetime - the same as the Pod that encloses it. 

At its core, a volume is just a directory, possibly with some data in it, which is accessible to the Containers in a Pod.

Volumes can not mount onto other volumes or have hard links to other volumes.

### Types of Volumes

#### configMap

The `configMap` resource provides a way to inject configuration data into Pods.

#### hostPath

A `hostPath` volume mounts a file or directory from the host node’s filesystem into your Pod.


#### local

A `local` volume represents a mounted local storage device such as a disk, partition or directory.

Local volumes can only be used as a statically created PersistentVolume. Dynamic provisioning is not supported yet.

#### nfs

An `nfs` volume allows an existing NFS (Network File System) share to be mounted into your Pod.

The contents of an nfs volume are preserved and the volume is merely unmounted when a Pod is removed.


#### persistentVolumeClaim

A `persistentVolumeClaim` volume is used to mount a PersistentVolume into a Pod.

#### secret

A `secret` volume is used to pass sensitive information, such as passwords, to Pods.

Concepts
Overview
What is Kubernetes
Kubernetes Components
The Kubernetes API
Working with Kubernetes Objects
Understanding Kubernetes Objects
Kubernetes Object Management
Names
Namespaces
Labels and Selectors
Annotations
Field Selectors
Recommended Labels
Kubernetes Architecture
Nodes
Master-Node communication
Concepts Underlying the Cloud Controller Manager
Containers
Images
Container Environment Variables
Runtime Class
Container Lifecycle Hooks
Workloads
Pods
Pod Overview
Pods
Pod Lifecycle
Init Containers
Pod Preset
Disruptions
Controllers
ReplicaSet
ReplicationController
Deployments
StatefulSets
DaemonSet
Garbage Collection
TTL Controller for Finished Resources
Jobs - Run to Completion
CronJob
Services, Load Balancing, and Networking
Service
DNS for Services and Pods
Connecting Applications with Services
Ingress
Ingress Controllers
Network Policies
Adding entries to Pod /etc/hosts with HostAliases
Storage
Volumes
Persistent Volumes
Volume Snapshots
CSI Volume Cloning
Storage Classes
Volume Snapshot Classes
Dynamic Volume Provisioning
Node-specific Volume Limits
Configuration
Configuration Best Practices
Managing Compute Resources for Containers
Assigning Pods to Nodes
Taints and Tolerations
Secrets
Organizing Cluster Access Using kubeconfig Files
Pod Priority and Preemption
Scheduling Framework
Security
Overview of Cloud Native Security
Scheduling
Kubernetes Scheduler
Scheduler Performance Tuning
Policies
Limit Ranges
Resource Quotas
Pod Security Policies
Cluster Administration
Cluster Administration Overview
Certificates
Cloud Providers
Managing Resources
Cluster Networking
Logging Architecture
Configuring kubelet Garbage Collection
Federation
Proxies in Kubernetes
Controller manager metrics
Installing Addons
Extending Kubernetes
Extending your Kubernetes Cluster
Extending the Kubernetes API
Extending the Kubernetes API with the aggregation layer
Custom Resources
Compute, Storage, and Networking Extensions
Network Plugins
Device Plugins
Operator pattern
Service Catalog
Poseidon-Firmament - An alternate scheduler
Edit This Page

Volumes
On-disk files in a Container are ephemeral, which presents some problems for non-trivial applications when running in Containers. First, when a Container crashes, kubelet will restart it, but the files will be lost - the Container starts with a clean state. Second, when running Containers together in a Pod it is often necessary to share files between those Containers. The Kubernetes Volume abstraction solves both of these problems.

Familiarity with Pods is suggested.

Background
Types of Volumes
Using subPath
Resources
Out-of-Tree Volume Plugins
Mount propagation
What's next
Background
Docker also has a concept of volumes, though it is somewhat looser and less managed. In Docker, a volume is simply a directory on disk or in another Container. Lifetimes are not managed and until very recently there were only local-disk-backed volumes. Docker now provides volume drivers, but the functionality is very limited for now (e.g. as of Docker 1.7 only one volume driver is allowed per Container and there is no way to pass parameters to volumes).

A Kubernetes volume, on the other hand, has an explicit lifetime - the same as the Pod that encloses it. Consequently, a volume outlives any Containers that run within the Pod, and data is preserved across Container restarts. Of course, when a Pod ceases to exist, the volume will cease to exist, too. Perhaps more importantly than this, Kubernetes supports many types of volumes, and a Pod can use any number of them simultaneously.

At its core, a volume is just a directory, possibly with some data in it, which is accessible to the Containers in a Pod. How that directory comes to be, the medium that backs it, and the contents of it are determined by the particular volume type used.

To use a volume, a Pod specifies what volumes to provide for the Pod (the .spec.volumes field) and where to mount those into Containers (the .spec.containers.volumeMounts field).

A process in a container sees a filesystem view composed from their Docker image and volumes. The Docker image is at the root of the filesystem hierarchy, and any volumes are mounted at the specified paths within the image. Volumes can not mount onto other volumes or have hard links to other volumes. Each Container in the Pod must independently specify where to mount each volume.

Types of Volumes
Kubernetes supports several types of Volumes:

awsElasticBlockStore
azureDisk
azureFile
cephfs
cinder
configMap
csi
downwardAPI
emptyDir
fc (fibre channel)
flexVolume
flocker
gcePersistentDisk
gitRepo (deprecated)
glusterfs
hostPath
iscsi
local
nfs
persistentVolumeClaim
projected
portworxVolume
quobyte
rbd
scaleIO
secret
storageos
vsphereVolume
We welcome additional contributions.

awsElasticBlockStore
An awsElasticBlockStore volume mounts an Amazon Web Services (AWS) EBS Volume into your Pod. Unlike emptyDir, which is erased when a Pod is removed, the contents of an EBS volume are preserved and the volume is merely unmounted. This means that an EBS volume can be pre-populated with data, and that data can be “handed off” between Pods.

Caution: You must create an EBS volume using aws ec2 create-volume or the AWS API before you can use it.
There are some restrictions when using an awsElasticBlockStore volume:

the nodes on which Pods are running must be AWS EC2 instances
those instances need to be in the same region and availability-zone as the EBS volume
EBS only supports a single EC2 instance mounting a volume
Creating an EBS volume
Before you can use an EBS volume with a Pod, you need to create it.

aws ec2 create-volume --availability-zone=eu-west-1a --size=10 --volume-type=gp2
Make sure the zone matches the zone you brought up your cluster in. (And also check that the size and EBS volume type are suitable for your use!)

AWS EBS Example configuration
apiVersion: v1
kind: Pod
metadata:
  name: test-ebs
spec:
  containers:
  - image: k8s.gcr.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /test-ebs
      name: test-volume
  volumes:
  - name: test-volume
    # This AWS EBS volume must already exist.
    awsElasticBlockStore:
      volumeID: <volume-id>
      fsType: ext4
CSI Migration
FEATURE STATE: Kubernetes v1.14 alpha
The CSI Migration feature for awsElasticBlockStore, when enabled, shims all plugin operations from the existing in-tree plugin to the ebs.csi.aws.com Container Storage Interface (CSI) Driver. In order to use this feature, the AWS EBS CSI Driver must be installed on the cluster and the CSIMigration and CSIMigrationAWS Alpha features must be enabled.

azureDisk
A azureDisk is used to mount a Microsoft Azure Data Disk into a Pod.

More details can be found here.

CSI Migration
FEATURE STATE: Kubernetes v1.15 alpha
The CSI Migration feature for azureDisk, when enabled, shims all plugin operations from the existing in-tree plugin to the disk.csi.azure.com Container Storage Interface (CSI) Driver. In order to use this feature, the Azure Disk CSI Driver must be installed on the cluster and the CSIMigration and CSIMigrationAzureDisk Alpha features must be enabled.

azureFile
A azureFile is used to mount a Microsoft Azure File Volume (SMB 2.1 and 3.0) into a Pod.

More details can be found here.

CSI Migration
FEATURE STATE: Kubernetes v1.15 alpha
The CSI Migration feature for azureFile, when enabled, shims all plugin operations from the existing in-tree plugin to the file.csi.azure.com Container Storage Interface (CSI) Driver. In order to use this feature, the Azure File CSI Driver must be installed on the cluster and the CSIMigration and CSIMigrationAzureFile Alpha features must be enabled.

cephfs
A cephfs volume allows an existing CephFS volume to be mounted into your Pod. Unlike emptyDir, which is erased when a Pod is removed, the contents of a cephfs volume are preserved and the volume is merely unmounted. This means that a CephFS volume can be pre-populated with data, and that data can be “handed off” between Pods. CephFS can be mounted by multiple writers simultaneously.

Caution: You must have your own Ceph server running with the share exported before you can use it.
See the CephFS example for more details.

cinder
Note: Prerequisite: Kubernetes with OpenStack Cloud Provider configured. For cloudprovider configuration please refer cloud provider openstack.
cinder is used to mount OpenStack Cinder Volume into your Pod.

Cinder Volume Example configuration
apiVersion: v1
kind: Pod
metadata:
  name: test-cinder
spec:
  containers:
  - image: k8s.gcr.io/test-webserver
    name: test-cinder-container
    volumeMounts:
    - mountPath: /test-cinder
      name: test-volume
  volumes:
  - name: test-volume
    # This OpenStack volume must already exist.
    cinder:
      volumeID: <volume-id>
      fsType: ext4
CSI Migration
FEATURE STATE: Kubernetes v1.14 alpha
The CSI Migration feature for Cinder, when enabled, shims all plugin operations from the existing in-tree plugin to the cinder.csi.openstack.org Container Storage Interface (CSI) Driver. In order to use this feature, the Openstack Cinder CSI Driver must be installed on the cluster and the CSIMigration and CSIMigrationOpenStack Alpha features must be enabled.

configMap
The configMap resource provides a way to inject configuration data into Pods. The data stored in a ConfigMap object can be referenced in a volume of type configMap and then consumed by containerized applications running in a Pod.

When referencing a configMap object, you can simply provide its name in the volume to reference it. You can also customize the path to use for a specific entry in the ConfigMap. For example, to mount the log-config ConfigMap onto a Pod called configmap-pod, you might use the YAML below:

apiVersion: v1
kind: Pod
metadata:
  name: configmap-pod
spec:
  containers:
    - name: test
      image: busybox
      volumeMounts:
        - name: config-vol
          mountPath: /etc/config
  volumes:
    - name: config-vol
      configMap:
        name: log-config
        items:
          - key: log_level
            path: log_level
The log-config ConfigMap is mounted as a volume, and all contents stored in its log_level entry are mounted into the Pod at path “/etc/config/log_level”. Note that this path is derived from the volume’s mountPath and the path keyed with log_level.

Caution: You must create a ConfigMap before you can use it.
Note: A Container using a ConfigMap as a subPath volume mount will not receive ConfigMap updates.
downwardAPI
A downwardAPI volume is used to make downward API data available to applications. It mounts a directory and writes the requested data in plain text files.

Note: A Container using Downward API as a subPath volume mount will not receive Downward API updates.
See the downwardAPI volume example for more details.

emptyDir
An emptyDir volume is first created when a Pod is assigned to a Node, and exists as long as that Pod is running on that node. As the name says, it is initially empty. Containers in the Pod can all read and write the same files in the emptyDir volume, though that volume can be mounted at the same or different paths in each Container. When a Pod is removed from a node for any reason, the data in the emptyDir is deleted forever.

Note: A Container crashing does NOT remove a Pod from a node, so the data in an emptyDir volume is safe across Container crashes.
Some uses for an emptyDir are:

scratch space, such as for a disk-based merge sort
checkpointing a long computation for recovery from crashes
holding files that a content-manager Container fetches while a webserver Container serves the data
By default, emptyDir volumes are stored on whatever medium is backing the node - that might be disk or SSD or network storage, depending on your environment. However, you can set the emptyDir.medium field to "Memory" to tell Kubernetes to mount a tmpfs (RAM-backed filesystem) for you instead. While tmpfs is very fast, be aware that unlike disks, tmpfs is cleared on node reboot and any files you write will count against your Container’s memory limit.

Example Pod
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: k8s.gcr.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /cache
      name: cache-volume
  volumes:
  - name: cache-volume
    emptyDir: {}
fc (fibre channel)
An fc volume allows an existing fibre channel volume to be mounted in a Pod. You can specify single or multiple target World Wide Names using the parameter targetWWNs in your volume configuration. If multiple WWNs are specified, targetWWNs expect that those WWNs are from multi-path connections.

Caution: You must configure FC SAN Zoning to allocate and mask those LUNs (volumes) to the target WWNs beforehand so that Kubernetes hosts can access them.
See the FC example for more details.

flocker
Flocker is an open-source clustered Container data volume manager. It provides management and orchestration of data volumes backed by a variety of storage backends.

A flocker volume allows a Flocker dataset to be mounted into a Pod. If the dataset does not already exist in Flocker, it needs to be first created with the Flocker CLI or by using the Flocker API. If the dataset already exists it will be reattached by Flocker to the node that the Pod is scheduled. This means data can be “handed off” between Pods as required.

Caution: You must have your own Flocker installation running before you can use it.
See the Flocker example for more details.

gcePersistentDisk
A gcePersistentDisk volume mounts a Google Compute Engine (GCE) Persistent Disk into your Pod. Unlike emptyDir, which is erased when a Pod is removed, the contents of a PD are preserved and the volume is merely unmounted. This means that a PD can be pre-populated with data, and that data can be “handed off” between Pods.

Caution: You must create a PD using gcloud or the GCE API or UI before you can use it.
There are some restrictions when using a gcePersistentDisk:

the nodes on which Pods are running must be GCE VMs
those VMs need to be in the same GCE project and zone as the PD
A feature of PD is that they can be mounted as read-only by multiple consumers simultaneously. This means that you can pre-populate a PD with your dataset and then serve it in parallel from as many Pods as you need. Unfortunately, PDs can only be mounted by a single consumer in read-write mode - no simultaneous writers allowed.

Using a PD on a Pod controlled by a ReplicationController will fail unless the PD is read-only or the replica count is 0 or 1.

Creating a PD
Before you can use a GCE PD with a Pod, you need to create it.

gcloud compute disks create --size=500GB --zone=us-central1-a my-data-disk
Example Pod
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: k8s.gcr.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /test-pd
      name: test-volume
  volumes:
  - name: test-volume
    # This GCE PD must already exist.
    gcePersistentDisk:
      pdName: my-data-disk
      fsType: ext4
Regional Persistent Disks
FEATURE STATE: Kubernetes v1.10 beta
The Regional Persistent Disks feature allows the creation of Persistent Disks that are available in two zones within the same region. In order to use this feature, the volume must be provisioned as a PersistentVolume; referencing the volume directly from a pod is not supported.

Manually provisioning a Regional PD PersistentVolume
Dynamic provisioning is possible using a StorageClass for GCE PD. Before creating a PersistentVolume, you must create the PD:

gcloud beta compute disks create --size=500GB my-data-disk
    --region us-central1
    --replica-zones us-central1-a,us-central1-b
Example PersistentVolume spec:

apiVersion: v1
kind: PersistentVolume
metadata:
  name: test-volume
  labels:
    failure-domain.beta.kubernetes.io/zone: us-central1-a__us-central1-b
spec:
  capacity:
    storage: 400Gi
  accessModes:
  - ReadWriteOnce
  gcePersistentDisk:
    pdName: my-data-disk
    fsType: ext4
CSI Migration
FEATURE STATE: Kubernetes v1.14 alpha
The CSI Migration feature for GCE PD, when enabled, shims all plugin operations from the existing in-tree plugin to the pd.csi.storage.gke.io Container Storage Interface (CSI) Driver. In order to use this feature, the GCE PD CSI Driver must be installed on the cluster and the CSIMigration and CSIMigrationGCE Alpha features must be enabled.

gitRepo (deprecated)
Warning: The gitRepo volume type is deprecated. To provision a container with a git repo, mount an EmptyDir into an InitContainer that clones the repo using git, then mount the EmptyDir into the Pod’s container.
A gitRepo volume is an example of what can be done as a volume plugin. It mounts an empty directory and clones a git repository into it for your Pod to use. In the future, such volumes may be moved to an even more decoupled model, rather than extending the Kubernetes API for every such use case.

Here is an example of gitRepo volume:

apiVersion: v1
kind: Pod
metadata:
  name: server
spec:
  containers:
  - image: nginx
    name: nginx
    volumeMounts:
    - mountPath: /mypath
      name: git-volume
  volumes:
  - name: git-volume
    gitRepo:
      repository: "git@somewhere:me/my-git-repository.git"
      revision: "22f1d8406d464b0c0874075539c1f2e96c253775"
glusterfs
A glusterfs volume allows a Glusterfs (an open source networked filesystem) volume to be mounted into your Pod. Unlike emptyDir, which is erased when a Pod is removed, the contents of a glusterfs volume are preserved and the volume is merely unmounted. This means that a glusterfs volume can be pre-populated with data, and that data can be “handed off” between Pods. GlusterFS can be mounted by multiple writers simultaneously.

Caution: You must have your own GlusterFS installation running before you can use it.
See the GlusterFS example for more details.

hostPath
A hostPath volume mounts a file or directory from the host node’s filesystem into your Pod. This is not something that most Pods will need, but it offers a powerful escape hatch for some applications.

For example, some uses for a hostPath are:

running a Container that needs access to Docker internals; use a hostPath of /var/lib/docker
running cAdvisor in a Container; use a hostPath of /sys
allowing a Pod to specify whether a given hostPath should exist prior to the Pod running, whether it should be created, and what it should exist as
In addition to the required path property, user can optionally specify a type for a hostPath volume.

The supported values for field type are:

Value	Behavior
Empty string (default) is for backward compatibility, which means that no checks will be performed before mounting the hostPath volume.
DirectoryOrCreate	If nothing exists at the given path, an empty directory will be created there as needed with permission set to 0755, having the same group and ownership with Kubelet.
Directory	A directory must exist at the given path
FileOrCreate	If nothing exists at the given path, an empty file will be created there as needed with permission set to 0644, having the same group and ownership with Kubelet.
File	A file must exist at the given path
Socket	A UNIX socket must exist at the given path
CharDevice	A character device must exist at the given path
BlockDevice	A block device must exist at the given path
Watch out when using this type of volume, because:

Pods with identical configuration (such as created from a podTemplate) may behave differently on different nodes due to different files on the nodes
when Kubernetes adds resource-aware scheduling, as is planned, it will not be able to account for resources used by a hostPath
the files or directories created on the underlying hosts are only writable by root. You either need to run your process as root in a privileged Container or modify the file permissions on the host to be able to write to a hostPath volume
Example Pod
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: k8s.gcr.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /test-pd
      name: test-volume
  volumes:
  - name: test-volume
    hostPath:
      # directory location on host
      path: /data
      # this field is optional
      type: Directory
iscsi
An iscsi volume allows an existing iSCSI (SCSI over IP) volume to be mounted into your Pod. Unlike emptyDir, which is erased when a Pod is removed, the contents of an iscsi volume are preserved and the volume is merely unmounted. This means that an iscsi volume can be pre-populated with data, and that data can be “handed off” between Pods.

Caution: You must have your own iSCSI server running with the volume created before you can use it.
A feature of iSCSI is that it can be mounted as read-only by multiple consumers simultaneously. This means that you can pre-populate a volume with your dataset and then serve it in parallel from as many Pods as you need. Unfortunately, iSCSI volumes can only be mounted by a single consumer in read-write mode - no simultaneous writers allowed.

See the iSCSI example for more details.

local
FEATURE STATE: Kubernetes v1.14 stable
A local volume represents a mounted local storage device such as a disk, partition or directory.

Local volumes can only be used as a statically created PersistentVolume. Dynamic provisioning is not supported yet.

Compared to hostPath volumes, local volumes can be used in a durable and portable manner without manually scheduling Pods to nodes, as the system is aware of the volume’s node constraints by looking at the node affinity on the PersistentVolume.

However, local volumes are still subject to the availability of the underlying node and are not suitable for all applications. If a node becomes unhealthy, then the local volume will also become inaccessible, and a Pod using it will not be able to run. Applications using local volumes must be able to tolerate this reduced availability, as well as potential data loss, depending on the durability characteristics of the underlying disk.

The following is an example of PersistentVolume spec using a local volume and nodeAffinity:

apiVersion: v1
kind: PersistentVolume
metadata:
  name: example-pv
spec:
  capacity:
    storage: 100Gi
  # volumeMode field requires BlockVolume Alpha feature gate to be enabled.
  volumeMode: Filesystem
  accessModes:
  - ReadWriteOnce
  persistentVolumeReclaimPolicy: Delete
  storageClassName: local-storage
  local:
    path: /mnt/disks/ssd1
  nodeAffinity:
    required:
      nodeSelectorTerms:
      - matchExpressions:
        - key: kubernetes.io/hostname
          operator: In
          values:
          - example-node
PersistentVolume nodeAffinity is required when using local volumes. It enables the Kubernetes scheduler to correctly schedule Pods using local volumes to the correct node.

PersistentVolume volumeMode can now be set to “Block” (instead of the default value “Filesystem”) to expose the local volume as a raw block device. The volumeMode field requires BlockVolume Alpha feature gate to be enabled.

When using local volumes, it is recommended to create a StorageClass with volumeBindingMode set to WaitForFirstConsumer. See the example. Delaying volume binding ensures that the PersistentVolumeClaim binding decision will also be evaluated with any other node constraints the Pod may have, such as node resource requirements, node selectors, Pod affinity, and Pod anti-affinity.

An external static provisioner can be run separately for improved management of the local volume lifecycle. Note that this provisioner does not support dynamic provisioning yet. For an example on how to run an external local provisioner, see the local volume provisioner user guide.

Note: The local PersistentVolume requires manual cleanup and deletion by the user if the external static provisioner is not used to manage the volume lifecycle.
nfs
An nfs volume allows an existing NFS (Network File System) share to be mounted into your Pod. Unlike emptyDir, which is erased when a Pod is removed, the contents of an nfs volume are preserved and the volume is merely unmounted. This means that an NFS volume can be pre-populated with data, and that data can be “handed off” between Pods. NFS can be mounted by multiple writers simultaneously.

Caution: You must have your own NFS server running with the share exported before you can use it.
See the NFS example for more details.

persistentVolumeClaim
A persistentVolumeClaim volume is used to mount a PersistentVolume into a Pod. PersistentVolumes are a way for users to “claim” durable storage (such as a GCE PersistentDisk or an iSCSI volume) without knowing the details of the particular cloud environment.

See the PersistentVolumes example for more details.

projected
A projected volume maps several existing volume sources into the same directory.

Currently, the following types of volume sources can be projected:

secret
downwardAPI
configMap
serviceAccountToken
All sources are required to be in the same namespace as the Pod. For more details, see the all-in-one volume design document.

The projection of service account tokens is a feature introduced in Kubernetes 1.11 and promoted to Beta in 1.12. To enable this feature on 1.11, you need to explicitly set the TokenRequestProjection feature gate to True.

Example Pod with a secret, a downward API, and a configmap.
apiVersion: v1
kind: Pod
metadata:
  name: volume-test
spec:
  containers:
  - name: container-test
    image: busybox
    volumeMounts:
    - name: all-in-one
      mountPath: "/projected-volume"
      readOnly: true
  volumes:
  - name: all-in-one
    projected:
      sources:
      - secret:
          name: mysecret
          items:
            - key: username
              path: my-group/my-username
      - downwardAPI:
          items:
            - path: "labels"
              fieldRef:
                fieldPath: metadata.labels
            - path: "cpu_limit"
              resourceFieldRef:
                containerName: container-test
                resource: limits.cpu
      - configMap:
          name: myconfigmap
          items:
            - key: config
              path: my-group/my-config
Example Pod with multiple secrets with a non-default permission mode set.
apiVersion: v1
kind: Pod
metadata:
  name: volume-test
spec:
  containers:
  - name: container-test
    image: busybox
    volumeMounts:
    - name: all-in-one
      mountPath: "/projected-volume"
      readOnly: true
  volumes:
  - name: all-in-one
    projected:
      sources:
      - secret:
          name: mysecret
          items:
            - key: username
              path: my-group/my-username
      - secret:
          name: mysecret2
          items:
            - key: password
              path: my-group/my-password
              mode: 511
Each projected volume source is listed in the spec under sources. The parameters are nearly the same with two exceptions:

For secrets, the secretName field has been changed to name to be consistent with ConfigMap naming.
The defaultMode can only be specified at the projected level and not for each volume source. However, as illustrated above, you can explicitly set the mode for each individual projection.
When the TokenRequestProjection feature is enabled, you can inject the token for the current service account into a Pod at a specified path. Below is an example:

apiVersion: v1
kind: Pod
metadata:
  name: sa-token-test
spec:
  containers:
  - name: container-test
    image: busybox
    volumeMounts:
    - name: token-vol
      mountPath: "/service-account"
      readOnly: true
  volumes:
  - name: token-vol
    projected:
      sources:
      - serviceAccountToken:
          audience: api
          expirationSeconds: 3600
          path: token
The example Pod has a projected volume containing the injected service account token. This token can be used by Pod containers to access the Kubernetes API server, for example. The audience field contains the intended audience of the token. A recipient of the token must identify itself with an identifier specified in the audience of the token, and otherwise should reject the token. This field is optional and it defaults to the identifier of the API server.

The expirationSeconds is the expected duration of validity of the service account token. It defaults to 1 hour and must be at least 10 minutes (600 seconds). An administrator can also limit its maximum value by specifying the --service-account-max-token-expiration option for the API server. The path field specifies a relative path to the mount point of the projected volume.

Note: A Container using a projected volume source as a subPath volume mount will not receive updates for those volume sources.
portworxVolume
A portworxVolume is an elastic block storage layer that runs hyperconverged with Kubernetes. Portworx fingerprints storage in a server, tiers based on capabilities, and aggregates capacity across multiple servers. Portworx runs in-guest in virtual machines or on bare metal Linux nodes.

A portworxVolume can be dynamically created through Kubernetes or it can also be pre-provisioned and referenced inside a Kubernetes Pod. Here is an example Pod referencing a pre-provisioned PortworxVolume:

apiVersion: v1
kind: Pod
metadata:
  name: test-portworx-volume-pod
spec:
  containers:
  - image: k8s.gcr.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /mnt
      name: pxvol
  volumes:
  - name: pxvol
    # This Portworx volume must already exist.
    portworxVolume:
      volumeID: "pxvol"
      fsType: "<fs-type>"
Caution: Make sure you have an existing PortworxVolume with name pxvol before using it in the Pod.
More details and examples can be found here.

quobyte
A quobyte volume allows an existing Quobyte volume to be mounted into your Pod.

Caution: You must have your own Quobyte setup running with the volumes created before you can use it.
Quobyte supports the Container Storage Interface . CSI is the recommended plugin to use Quobyte volumes inside Kubernetes. Quobyte’s GitHub project has instructions for deploying Quobyte using CSI, along with examples.

rbd
An rbd volume allows a Rados Block Device volume to be mounted into your Pod. Unlike emptyDir, which is erased when a Pod is removed, the contents of a rbd volume are preserved and the volume is merely unmounted. This means that a RBD volume can be pre-populated with data, and that data can be “handed off” between Pods.

Caution: You must have your own Ceph installation running before you can use RBD.
A feature of RBD is that it can be mounted as read-only by multiple consumers simultaneously. This means that you can pre-populate a volume with your dataset and then serve it in parallel from as many Pods as you need. Unfortunately, RBD volumes can only be mounted by a single consumer in read-write mode - no simultaneous writers allowed.

See the RBD example for more details.

scaleIO
ScaleIO is a software-based storage platform that can use existing hardware to create clusters of scalable shared block networked storage. The scaleIO volume plugin allows deployed Pods to access existing ScaleIO volumes (or it can dynamically provision new volumes for persistent volume claims, see ScaleIO Persistent Volumes).

Caution: You must have an existing ScaleIO cluster already setup and running with the volumes created before you can use them.
The following is an example of Pod configuration with ScaleIO:

apiVersion: v1
kind: Pod
metadata:
  name: pod-0
spec:
  containers:
  - image: k8s.gcr.io/test-webserver
    name: pod-0
    volumeMounts:
    - mountPath: /test-pd
      name: vol-0
  volumes:
  - name: vol-0
    scaleIO:
      gateway: https://localhost:443/api
      system: scaleio
      protectionDomain: sd0
      storagePool: sp1
      volumeName: vol-0
      secretRef:
        name: sio-secret
      fsType: xfs
For further detail, please see the ScaleIO examples.

secret
A secret volume is used to pass sensitive information, such as passwords, to Pods. You can store secrets in the Kubernetes API and mount them as files for use by Pods without coupling to Kubernetes directly. secret volumes are backed by tmpfs (a RAM-backed filesystem) so they are never written to non-volatile storage.

Caution: You must create a secret in the Kubernetes API before you can use it.
Note: A Container using a Secret as a subPath volume mount will not receive Secret updates.
Secrets are described in more detail here.

storageOS
A storageos volume allows an existing StorageOS volume to be mounted into your Pod.

StorageOS runs as a Container within your Kubernetes environment, making local or attached storage accessible from any node within the Kubernetes cluster. Data can be replicated to protect against node failure. Thin provisioning and compression can improve utilization and reduce cost.

At its core, StorageOS provides block storage to Containers, accessible via a file system.

The StorageOS Container requires 64-bit Linux and has no additional dependencies. A free developer license is available.

Caution: You must run the StorageOS Container on each node that wants to access StorageOS volumes or that will contribute storage capacity to the pool. For installation instructions, consult the StorageOS documentation.
apiVersion: v1
kind: Pod
metadata:
  labels:
    name: redis
    role: master
  name: test-storageos-redis
spec:
  containers:
    - name: master
      image: kubernetes/redis:v1
      env:
        - name: MASTER
          value: "true"
      ports:
        - containerPort: 6379
      volumeMounts:
        - mountPath: /redis-master-data
          name: redis-data
  volumes:
    - name: redis-data
      storageos:
        # The `redis-vol01` volume must already exist within StorageOS in the `default` namespace.
        volumeName: redis-vol01
        fsType: ext4
For more information including Dynamic Provisioning and Persistent Volume Claims, please see the StorageOS examples.

vsphereVolume
Note: Prerequisite: Kubernetes with vSphere Cloud Provider configured. For cloudprovider configuration please refer vSphere getting started guide.
A vsphereVolume is used to mount a vSphere VMDK Volume into your Pod. The contents of a volume are preserved when it is unmounted. It supports both VMFS and VSAN datastore.

Caution: You must create VMDK using one of the following methods before using with Pod.
Creating a VMDK volume
Choose one of the following methods to create a VMDK.

Create using vmkfstoolsCreate using vmware-vdiskmanager
First ssh into ESX, then use the following command to create a VMDK:

vmkfstools -c 2G /vmfs/volumes/DatastoreName/volumes/myDisk.vmdk
vSphere VMDK Example configuration
apiVersion: v1
kind: Pod
metadata:
  name: test-vmdk
spec:
  containers:
  - image: k8s.gcr.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /test-vmdk
      name: test-volume
  volumes:
  - name: test-volume
    # This VMDK volume must already exist.
    vsphereVolume:
      volumePath: "[DatastoreName] volumes/myDisk"
      fsType: ext4
More examples can be found here.

### Using subPath

Sometimes, it is useful to share one volume for multiple uses in a single Pod. The `volumeMounts.subPath` property can be used to specify a sub-path inside the referenced volume instead of its root.

Here is an example of a Pod with a LAMP stack (Linux Apache Mysql PHP) using a single, shared volume. The HTML contents are mapped to its `html` folder, and the databases will be stored in its `mysql` folder:

``` yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-lamp-site
spec:
    containers:
    - name: mysql
      image: mysql
      env:
      - name: MYSQL_ROOT_PASSWORD
        value: "rootpasswd"
      volumeMounts:
      - mountPath: /var/lib/mysql
        name: site-data
        subPath: mysql
    - name: php
      image: php:7.0-apache
      volumeMounts:
      - mountPath: /var/www/html
        name: site-data
        subPath: html
    volumes:
    - name: site-data
      persistentVolumeClaim:
        claimName: my-lamp-site-data
```

### CSI

**Container Storage Interface (CSI)** defines a standard interface for container orchestration systems (like Kubernetes) to expose arbitrary storage systems to their container workloads.


----

## Persistent Volumes

from [Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)


- A **`PersistentVolume (PV)`** is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes. It is a resource in the cluster just like a node is a cluster resource. 
- A **`PersistentVolumeClaim (PVC)`** is a request for storage by a user. It is similar to a pod. Pods consume node resources and PVCs consume PV resources. 

### Lifecycle of a volume and claim

PVs are resources in the cluster. PVCs are requests for those resources and also act as claim checks to the resource.

- Provisioning:
	- Static
	- Dynamic
- Binding
- Using
	- **Storage Object in Use Protection**: The purpose of the Storage Object in Use Protection feature is to ensure that Persistent Volume Claims (PVCs) in active use by a pod and Persistent Volume (PVs) that are bound to PVCs are not removed from the system as this may result in data loss.
- Reclaiming
- Retain
- Delete


### Persistent Volumes

Each PV contains a spec and status, which is the specification and status of the volume.


``` yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv0003
spec:
  capacity:
    storage: 5Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Recycle
  storageClassName: slow
  mountOptions:
    - hard
    - nfsvers=4.1
  nfs:
    path: /tmp
    server: 172.17.0.2
```

A PV can have a class, which is specified by setting the `storageClassName` attribute to the name of a StorageClass. **A PV of a particular class can only be bound to PVCs requesting that class. A PV with no storageClassName has no class and can only be bound to PVCs that request no particular class.**

### PersistentVolumeClaims

Each PVC contains a spec and status, which is the specification and status of the claim.

``` yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 8Gi
  storageClassName: slow
  selector:
    matchLabels:
      release: "stable"
    matchExpressions:
      - {key: environment, operator: In, values: [dev]}
```

Claims can specify a label selector to further filter the set of volumes.

A claim can request a particular class by specifying the name of a StorageClass using the attribute `storageClassName`.

### Claims As Volumes

Pods access storage by using the claim as a volume

``` yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: myclaim
```

### Volume Snapshot and Restore Volume from Snapshot Support

Volume snapshot feature was added to support CSI Volume Plugins only.

To enable support for restoring a volume from a volume snapshot data source, enable the `VolumeSnapshotDataSource` feature gate on the apiserver and controller-manager.

### Volume Cloning

Volume clone feature was added to support CSI Volume Plugins only.

### What are block devices?

Block devices enable random access to data in fixed-size blocks. **Hard drives, SSDs, and CD-ROMs drives are all examples of block devices.**

[Amazon Elastic Block Store](https://aws.amazon.com/cn/ebs/) - 易于使用、适用于任何规模的高性能数据块存储


----


## Volume Snapshots

from [Volume Snapshots](https://kubernetes.io/docs/concepts/storage/volume-snapshots/)

`VolumeSnapshotContent` and `VolumeSnapshot` API resources are provided to create volume snapshots for users and administrators.

- A **`VolumeSnapshotContent`** is a snapshot taken from a volume in the cluster that has been provisioned by an administrator. It is a resource in the cluster just like a *PersistentVolume* is a cluster resource.
- A **`VolumeSnapshot`** is a request for snapshot of a volume by a user. It is similar to a *PersistentVolumeClaim*.

----


## Storage Classes

from [Storage Classes](https://kubernetes.io/docs/concepts/storage/storage-classes/)

A `StorageClass` provides a way for administrators to describe the “classes” of storage they offer.

Administrators can specify a default `StorageClass` just for PVCs that don’t request any particular class to bind to.

``` yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: standard
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
reclaimPolicy: Retain
mountOptions:
  - debug
volumeBindingMode: Immediate
```

***Provisioner***:

- AWSElasticBlockStore
- AzureFile
- Flocker
- NFS
- Local

----


## Volume Snapshot Classes

from [Volume Snapshot Classes](https://kubernetes.io/docs/concepts/storage/volume-snapshot-classes/)

`VolumeSnapshotClass` provides a way to describe the “classes” of storage when provisioning a volume snapshot.

----

## Dynamic Volume Provisioning

from [Dynamic Volume Provisioning](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/)

Dynamic volume provisioning allows storage volumes to be created on-demand.

----


## Node-specific Volume Limits

from [Node-specific Volume Limits](https://kubernetes.io/docs/concepts/storage/storage-limits/)

Cloud providers like Google, Amazon, and Microsoft typically have a limit on how many volumes can be attached to a Node. It is important for Kubernetes to respect those limits. Otherwise, Pods scheduled on a Node could get stuck waiting for volumes to attach.















