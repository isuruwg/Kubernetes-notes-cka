# TOC <!-- omit in toc -->

- [1. Introduction](#1-introduction)
  - [1.1. Installing for local development](#11-installing-for-local-development)
    - [1.1.1. Install Docker in rootless mode](#111-install-docker-in-rootless-mode)
    - [1.1.2. Install Kubectl](#112-install-kubectl)
    - [1.1.3. Install KIND](#113-install-kind)
  - [1.2. Course notes from Udemy course](#12-course-notes-from-udemy-course)
  - [1.3. Nodes (Minions)](#13-nodes-minions)
  - [1.4. Cluster](#14-cluster)
  - [1.5. Master](#15-master)
  - [1.6. Other components](#16-other-components)
  - [1.7. Master vs Worker Nodes](#17-master-vs-worker-nodes)
- [2. Core concepts](#2-core-concepts)
  - [2.1. Example pod definition](#21-example-pod-definition)
- [3. Tips for exam](#3-tips-for-exam)
  - [3.1. Use tmux](#31-use-tmux)
    - [3.1.1. Setup tmux](#311-setup-tmux)
    - [3.1.2. Common tmux commands](#312-common-tmux-commands)
  - [3.2. Using kubectl to generate YAML files](#32-using-kubectl-to-generate-yaml-files)
  - [Using kubectl explain](#using-kubectl-explain)
  - [3.3. Bookmarks for exam](#33-bookmarks-for-exam)
- [4. Exam registration](#4-exam-registration)
- [5. References](#5-references)

# 1. Introduction

This repo contains my notes and experiments for the CKA and CKAD exams.

## 1.1. Installing for local development

There are [multiple ways](https://brennerm.github.io/posts/minikube-vs-kind-vs-k3s.html) to install Kuberenetes locally for development. Here, I'll use [KIND](https://kind.sigs.k8s.io/docs/user/quick-start/). 

### 1.1.1. Install Docker in rootless mode

TODO: https://docs.docker.com/engine/security/rootless/

### 1.1.2. Install Kubectl

Follow [Install using native package management](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management). 

(There's a snap install option too, and as of 2021-10-07 snap install seems to also install the latest version. However, I used the apt method)


### 1.1.3. Install KIND

TODO###############################################

WAIT: Explore [Install K8S with Vagrant and Ansible](https://kubernetes.io/blog/2019/03/15/kubernetes-setup-using-ansible-and-vagrant/) and/or [Install K8S with Kubesprey](https://kubernetes.io/docs/setup/production-environment/tools/kubespray/)

THIS HAS BEEN MOVED TO PC SETUP REPO AT : https://github.com/isuruwg/pc-setup ####################################################################################

[REF](https://kind.sigs.k8s.io/docs/user/quick-start/)

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64
chmod +x ./kind
mkdir -p ~/KIND/kind_0_11_1/
mv ./kind ~/KIND/kind_0_11_1/
```

Add the following to the `~/.bashrc` (on Ubuntu) to add KIND to your path.

```bash
export PATH="~/KIND/kind_0_11_1:$PATH"
```
Create cluster (The default cluster is named kind): 

```bash
kind create cluster
```

If you want to delete a cluster use `kind delete cluster` command.


## 1.2. Course notes from Udemy course

The course notes from the [Udemy CKA course](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests) is available as a Git submodule in this repo.

This Git submodule is a fork from [this repo](https://github.com/kodekloudhub/certified-kubernetes-administrator-course) which I have slightly changed with my own notes.

## 1.3. Nodes (Minions)

Nodes are machines where Kubernetes is installed in. 

## 1.4. Cluster

A cluster is a set of nodes grouped together

## 1.5. Master

The master is another node with Kubernetes installed in it, and configured as a Master. The master watches
over the nodes in the cluster and is responsible for the actual orchestration of containers on the worker nodes.

## 1.6. Other components

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

## 1.7. Master vs Worker Nodes

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

## 3.2. Using kubectl to generate YAML files

[REF](https://kubernetes.io/docs/reference/kubectl/conventions/)

```bash
# Create an NGINX Pod

kubectl run nginx --image=nginx

# Generate POD Manifest YAML file (-o yaml). Don't create it in the system (--dry-run=client)

kubectl run nginx --image=nginx --dry-run=client -o yaml

# you can use restart=Never flag to make sure what's created by this command is a pod
kubectl run yellow --image=busybox --restart=Never --dry-run=client -o yaml > pod.yaml

# Create a deployment

kubectl create deployment --image=nginx nginx

# Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)

kubectl create deployment --image=nginx nginx --dry-run=client -o yaml

# Generate Deployment YAML file (-o yaml). Don't create it(--dry-run) with 4 Replicas (--replicas=4)

kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml

# Save it to a file, make necessary changes to the file (for example, adding more replicas) and then create the deployment.

kubectl create -f nginx-deployment.yaml

# OR

# In k8s version 1.19+, we can specify the --replicas option to create a deployment with 4 replicas.

kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
```
`--dry-run`: By default as soon as the command is run, the resource will be created. If you simply want to test your command , use the `--dry-run=client` option. This will not create the resource, instead, tell you whether the resource can be created and if your command is right.

`-o yaml`: This will output the resource definition in YAML format on screen.

More info can be also found in [this lecture note](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/learn/lecture/15018998#overview)

You can also get the specification of a currently running pod into a yaml file by doing: `kubectl get pod app -o yaml > app.yaml`

## Using kubectl explain

You can use the `kubectl explain` command to get example yaml snippets. 

eg: 

```bash
kubectl explain pods --recursive | less

# after this is displayed, you can search for things using the forward slash(/). 

# You can also use the grep command
kubectl explain ppods --recursive | grep -A8 envFrom
# The above command displays 8 lines after envFrom
```

You can check [here](https://gist.github.com/glnds/8862214) for a cheatsheet of the `less` command.

## 3.3. Bookmarks for exam

https://kubernetes.io/docs/reference/kubectl/conventions/

# 4. Exam registration

**Check lecture 5 for a discount code for 15% discount when registering for the CKAD or CKA exams**

# 5. References

1 [CKA with practice tests - Udemy](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests)

2 [CKAD - Udemy](https://www.udemy.com/course/certified-kubernetes-application-developer/)