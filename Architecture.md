# Kubernetes Architecture

Cluster is a group of many nodes and Pods.

There are mainly 2 Nodes in Kubernetes

## MasterNode / Control Plane

## Worker Node

Containers of a application are deployed on worker nodes and most of the job was done by worker nodes and master node is used to manange all the containers across all the worker nodes.

There are some processes which are specific to MasterNode and WorkerNode.

Commands we enter using kubectl are hit to API server in Master node and then 

## Worker Node Processes

### Container Runtime

### Kubelet

### KubeProxy


Every Worker Node has these 3 processes installed through container runtime containers run inside a pod.

Kubelet is responsible for running the pod. When the request  was received from API server to run pod this kubelet communicates with 
#### container runtime (responsible to fetch image, attach networks..) using CRI, CNI   and runs the pod.

KubeProxy responsible for maintaining network rules on nodes. Because of these rules pods can communicate updates networking configuration for pod.

// image of worker node


## Master Node Processes

###  API Server

###  Scheduler

### Controller Manager

### etcd
### Cloud controller manager
Master Node Processes are used to interact with workernodes cluster

API Server is an entry point to the Kubernetes cluster all the services interacts with this component.

The scheduler decides on which worker node a new pod has to be scheduled talks to api server , and kubelet on that worker node creates a new pod. 

ControllerManager detects state changes in the cluster, and when any pod dies, it informs the API server , which then does its job.
There are various types 
Deployment controller manager
Replicaset controller manager
Daemon set controller manager
Controller manager looks after all these controller managers.

etcd is called the brain of the cluster; it stores cluster-related data as a key-value pair, which means resources that node has used, the health of the cluster, and any pods that have died. All these are stored in etcd, and the etcd controller knows about pods, and the scheduler knows where to place a pod.

etcd doesn't store application related data.

Cloud controller manager is , when the kubernetes is implemented as a managed service by any cloud then this service is used.
  For example:
If we want to create a pod in AWS EKS which is AWS  managed k8s service AWS communicates with  this cloud controller manager to  create pod .


![image](https://user-images.githubusercontent.com/96729391/226091362-d7c829aa-17cd-43c8-81cc-519edc103f36.png)

