# Kubernetes

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Kubernetes](#kubernetes)
  - [The need](#the-need)
  - [Architecture](#architecture)
  - [Basic Concepts](#basic-concepts)
    - [Pods](#pods)
  - [Kubernetes Configuration](#kubernetes-configuration)

<!-- /code_chunk_output -->

Kubernetes is an open source **container orchestration tool** developed by google that helps us manage containerized apps in different deployment environments e.g. physical, virtual or cloud.

## The need

The usage of **microservices** increased usage of **containers**. Tools like Kubernetes provide us with these:

- High availability / no downtime
- Scalability / high performance
- Disaster recovery / backup & restore

## Architecture

In kubernetes we have at least one **master node** & a couple of **worker nodes**. Each node has a **kubelet** process running on it.

**NOTE:** Kubelet is a Kubernetes process that makes it possible for the cluster to communicate to each other.

There are several docker containers running on worker nodes as applications.
Master node runs several Kubernetes processes to manage the cluster properly:

- There is an **API Server** as a container on master node that is the entrypoint to K8S cluster, so this is the process which different Kuber clients will talk to.
- Another process running on master is **Controller Manager**. It keeps track of what's happening in the cluster.
- **Scheduler** is responsible for Pods' placement based on workload of each worker node.
- **etcd key/value storage** holds the current state of cluster. The backup / restore process mentioned earlier is possible using etcd snapshots.

**Virtual Network** is the last piece of the cluster that enables nodes to talk to each other. It creates one unified machine.

**NOTE:** Worker nodes are much bigger because they run several applications, but master node runs only some light processes, but it is much more important. If you lose the master node, you won't be able to access the cluster. It's crucial to have some other masters.

## Basic Concepts

### Pods

Pods are the smallest unit in a k8s environment. Pod is a wrapper of a container. On each worker node we may have multiple pods, and in each pod we may have multiple containers. Usually, there is one pod per app. The virtual network assigns **IP address** to each pod, so each pod is its onw self-contained server. Pods use the assigned IP addresses to communicate.

**NOTE:** In k8s we don't create containers. We only work with pods. Pod manages the containers inside.

Pods are created frequently. When a new pod is created it gets a new IP address. To manage these changes there is a component called **Service**. Instead of dynamic IP addresses there are services sitting in front of each pod that talk to each other.
Service has 2 main functionalities:

1. permanent IP address
2. load balancer

## Kubernetes Configuration

All k8s configurations go through the API server mentioned earlier. We can use the UI, API script or CLI i.e. `kubectl` to send the configuration requests. The requests can be in **YAML** or **JSON** format.
