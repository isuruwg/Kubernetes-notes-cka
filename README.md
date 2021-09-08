# TOC <!-- omit in toc -->

- [1. Introduction](#1-introduction)
  - [1.1. Course notes from Udemy course](#11-course-notes-from-udemy-course)
  - [1.2. Nodes (Minions)](#12-nodes-minions)
  - [1.3. Cluster](#13-cluster)
  - [1.4. Master](#14-master)
  - [1.5. Other components](#15-other-components)
  - [1.6. Master vs Worker Nodes](#16-master-vs-worker-nodes)
- [2. Core concepts](#2-core-concepts)
  - [2.1. Example pod definition](#21-example-pod-definition)
- [3. Tips for exam](#3-tips-for-exam)
  - [3.1. Use tmux](#31-use-tmux)
    - [3.1.1. Setup tmux](#311-setup-tmux)
    - [3.1.2. Common tmux commands](#312-common-tmux-commands)
- [4. Exam registration](#4-exam-registration)
- [5. References](#5-references)

# 1. Introduction

This repo contains my notes and experiments for the CKA and CKAD exams.

## 1.1. Course notes from Udemy course

The course notes from the [Udemy CKA course](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests) is available as a Git submodule in this repo.

This Git submodule is a fork from [this repo](https://github.com/kodekloudhub/certified-kubernetes-administrator-course) which I have slightly changed with my own notes.

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

## 2.1. Example pod definition

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

# 3. Tips for exam

## 3.1. Use tmux

[tmux](https://github.com/tmux/tmux) comes pre-installed during the exam. 

For preparing for the exam, you can install using `sudo apt install tmux` [[ref](https://github.com/tmux/tmux/wiki/Installing)]

### 3.1.1. Setup tmux

```bash
# create ~/.tmux.conf
vi ~/.tmux.conf
```

Add the following to the `tmux.conf` file: 

```conf
# To enable mouse scroll in tmux pane
set -g mouse on

# Improve colors
set -g default-terminal 'screen-256color'

# Set scrollback buffer to 10000
set -g history-limit 10000

# Customize the status line
set -g status-fg  green
set -g status-bg  black

# Change the default prefix from C-b to C-z
# set -g prefix C-z
# unbind C-b
```

### 3.1.2. Common tmux commands

```bash
# starting session 
tmux

# split pane horizontally
(Ctrl+b) + %
# Press Ctrl+b together -> let go of the two keys -> press %

# Split pane vertically
(Ctrl+b) + "
```
```bash
# move between different windows
(Ctrl+b) + Arrow keys 

# Closing panes
# type exit or do: 
Ctrl+d

# Creating new windows:
(Ctrl+b)+c

# Switch to previous window
(Ctrl+b)+p

# Switch to next window
(Ctrl+b)+n

# Detach current session (detaching a session will leave what you're doing still running in the background)
(Ctrl+b)+d

# Pick which session to detach
(Ctrl+b)+D

# view sessions
tmux ls

# attach session
# The 0 here would be the session name (the part before the colon in tmux ls)
tmux attach-session -t 0

# Create session with name
tmux new -s session_name
```

Some other window and pane shortcuts

```bash
Ctrl+b c # Create a new window (with shell)
Ctrl+b w # Choose window from a list
Ctrl+b 0 # Switch to window 0 (by number )
Ctrl+b , # Rename the current window
Ctrl+b % # Split current pane horizontally into two panes
# Split current pane vertically into two panes
Ctrl+b " 
```
```bash
Ctrl+b o # Go to the next pane
Ctrl+b ; # Toggle between the current and previous pane
Ctrl+b x # Close the current pane
```

# 4. Exam registration

**Check lecture 5 for a discount code for 15% discount when registering for the CKAD or CKA exams**

# 5. References

1 [CKA with practice tests - Udemy](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests)

2 [CKAD - Udemy](https://www.udemy.com/course/certified-kubernetes-application-developer/)