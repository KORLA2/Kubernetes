# Kubernetes Architecture

Cluster is a group of many nodes and Pods.

There are mainly 2 Nodes in Kubernetes

## MasterNode / Control Plane

## Worker Node

Containers of a application are deployed on worker nodes and most of the job was done by worker nodes and master node is used to manange all the containers across all the worker nodes.

There are some processes which are specific to MasterNode and WorkerNode.

## Worker Node Processes

### Container Runtime

### Kubelet

### KubeProxy


Every Worker Node has these 3 processes installed through container runtime containers run inside a pod.

Kubelet can communicate with node and container. Kubelet starts pod with container inside.

KubeProxy provides intelligence to service, instead of randomly forwarding request that service receives to any other pods that the service was attached to. It forwards to the node from which it has received request ,to reduce network overheads.

![image](https://user-images.githubusercontent.com/96729391/226091358-a1915d25-5979-4b44-857b-bff3b72692df.png)



## Master Node Processes

###  API Server

###  Scheduler

### Controller Manager

### etcd

Master Node Processes are used to interact with workernodes cluster

API Server is an entry point to the Kubernetes cluster.

The scheduler decides on which worker node a new pod has to be scheduled, and kubectl on that worker node creates a new pod.

ControllerManager detects state changes in the cluster, and when any pod dies, it informs the scheduler, which then does its job.

etcd is called the brain of the cluster; it stores cluster-related data as a key-value pair, which means resources that node has used, the health of the cluster, and any pods that have died. All these are stored in etcd, and the etcd controller knows about pods, and the scheduler knows where to place a pod.

etcd doesn't store application related data.

![image](https://user-images.githubusercontent.com/96729391/226091362-d7c829aa-17cd-43c8-81cc-519edc103f36.png)

