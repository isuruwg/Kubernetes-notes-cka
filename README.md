# TOC <!-- omit in toc -->

- [1. Introduction](#1-introduction)
  - [1.1. Course notes from Udemy course](#11-course-notes-from-udemy-course)
  - [1.2. Nodes (Minions)](#12-nodes-minions)
  - [1.3. Cluster](#13-cluster)
  - [1.4. Master](#14-master)
  - [1.5. Other components](#15-other-components)
  - [1.6. Master vs Worker Nodes](#16-master-vs-worker-nodes)
- [2. Core concepts](#2-core-concepts)
  - [Example pod definition](#example-pod-definition)
- [3. Exam registration](#3-exam-registration)
- [4. References](#4-references)

# 1. Introduction

This repo contains my notes and experiments for the CKA and CKAD exams.

## 1.1. Course notes from Udemy course

The course notes from the [Udemy CKA course](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests) is available as a Git submodule in this repo.

This Git submodule is a fork from https://github.com/kodekloudhub/certified-kubernetes-administrator-course which I have slightly changed with my own notes.

## 1.2. Nodes (Minions)

Nodes are machines where Kubernetes is installed in. 

## 1.3. Cluster

A cluster is a set of nodes grouped together

## 1.4. Master

The master is another node with Kubernetes installed in it, and configured as a Master. The master watches
over the nodes in the cluster and is responsible for the actual orchestration of containers on the worker nodes.

## 1.5. Other components

  ![Kubernetes Architecture](certified-kubernetes-administrator-course/images/k8s-arch.PNG)
  
  ![Kubernetes Architecture 1](certified-kubernetes-administrator-course/images/k8s-arch1.PNG)

When you install Kubernetes on a system, you are actually installing the following components;

1. An API Server
2. An ETCD service
3. A Kubelet service
4. A container runtime
5. Controllers
6. Schedulers

**API Server:** The API server acts as the front-end for kubernetes. The users, management devices, Command line interfaces all talk to the API server to interact with the kubernetes cluster. 

**ETCD Key Store:** ETCD is a distributed reliable key-value store used by kubernetes to store all data used to manage the cluster. Think of it this way, when you have multiple nodes and multiple masters in your cluster, etcd stores all that information on all the nodes in the cluster in a distributed manner. ETCD is responsible for implementing locks within the cluster to ensure there are no conflicts between the Masters.

**Scheduler:** The scheduler is responsible for distributing work or containers across multiple nodes. It looks for newly created containers and assigns them to Nodes.

**Controllers:** The controllers are the brain behind orchestration. They are responsible for noticing and responding when nodes, containers or endpoints goes down. The controllers makes decisions to bring up new containers in such cases.

**Container runtime:** This is the underlying software that is used to run containers. Eg: Docker

**Kubelet:** This is the agent that runs on each node in the cluster. The agent is responsible for making sure that the containers are running on the nodes as expected.

**Kube-proxy:** This takes care of networking within Kubernetes. This allows containers to communicate with each other.

## 1.6. Master vs Worker Nodes

- Master has the kube-apiserver which makes it the master
- Workers have Kubelet which communicates with the kube-apiserver
- Master stores all gathered information in a key value store based on the etcd framework
- Master also has the controller and scheduler

# 2. Core concepts

A Kubernetes definition file always contains four top level fields, 

  ```yaml
  apiVersion:
  kind: 
  metadata:


  spec:

  ```

Some example values for `kind` and `apiVersion` fields:

| Kind       | Version |
|------------|---------|
| POD        | v1      |
| Service    | V1      |
| ReplicaSet | apps/v1 |
| Deployment | apps/v1 |

## Example pod definition

```yaml
apiVersion: v1
kind: Pod
metadata: # This is a dictionary
  name: myapp-pod # string name
  labels: # Can be any key value pair
    app: myapp
    type: front-end
spec: # This is a dictionary
  containers: # This is a list/array
    - name: nginx-container
      image: nginx
```

Once this file is created and saved [pod-definition.yml](simple-tests/pod-definition.yml) you can create this by running `kubectl create -f pod-definition.yml`

You can get a list of running pods by doing `kubectl get pods` and get a detailed description of the pod by doing `kubectl describe pod myapp-pod`. 


# 3. Exam registration

**Check lecture 5 for a discount code for 15% discount when registering for the CKAD or CKA exams**

# 4. References

1 [CKA with practice tests - Udemy](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests)

2 [CKAD - Udemy](https://www.udemy.com/course/certified-kubernetes-application-developer/)