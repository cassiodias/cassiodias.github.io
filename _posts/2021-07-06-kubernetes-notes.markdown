---
layout: post
title:  "Kubernetes notes"
date:   2021-07-06 19:03
categories: generic
---

{:toc}

# Kubernetes notes, the basics

## What? 

* Container orchestration tool created by google
* Manages container application

## Why? 

With the raise of micro-services and containers, how to manage them? Scripts? 

**Here K8s came offering**

* High availability, scalability & disaster recovery
    * Service discovery and load balancing Kubernetes
    * Storage orchestration 
    * Automated rollouts and rollbacks Automatic bin packing
    * Self-healing 
    * Secret and configuration management

## Components

* Worker Node or simply Node: the nodes in a cluster are the machines (VMs, physical servers, etc) that run your applications and cloud workflows
* POD: smallest unit of K8s, abstraction over a container
    * Can run multiple containers but usually runs just one 
    * Are ephemeral
* Virtual Network: each pod gets its own internal IP address	
* Service (SVC): permanent IP address (not connected to a specific POD) to expose application
    * internal and external
    * pods use service to communicate
* External configurations
    * Config map: has external configuration of the apps, like DB_URL (dont use credentials!)	
    * Secret: like config map but for credentials in base 64 format (DB_PASSWORD)
* Ingress: API that manages external access to the services in a cluster, typically HTTP. Ingress may provide load balancing, SSL termination and name-based virtual hosting (ex. NGINX)
    *  |client| --> |ingress| -> [routing rules] -> |service| -> |POD|
* Data store
    * Volumes
        * attach a physical storage on a hard drive to the pod in the local machine or remote (cloud storage)
        * Ephemeral volume types have a lifetime of a pod, but persistent volumes exist beyond the lifetime of a pod
        * Elastic Block Store (EBS) is one example of persisted volume `$ aws ec2 create-volume`, BUT, the nodes on which pods are running must be AWS EC2 instances, those instances need to be in the same region and availability zone as the EBS volume and EBS only supports a single EC2 instance mounting a volume
    * K8S doesnt manage data persistence (no BKP offered)
* Deployment
    * Blueprint of pods
    * Abstraction for pod replication
    * We don't work with pods but deployments
    * What happens if the database died? Data inconsistencies?
        * `StatefulSet` is used to manage in/out syn in the database
        * Too much hassle, put db outside K8s
    * key feature about availability and scalability 
    * Use `labels` to monitor the availability of a sufficient amount of pods
* Labels
    * K8s API Objects are suing labels to connect to other objects
    * `ReplicaSet` monitors the pod label to verify a sufficient amount of Pods is available
    * Labels are automatically set when starting deployments
    * `kubectl label` can be used to manually set a label
* Namespaces
    * namespaces provide isolated env is a k8s environment
    * using namespaces make sense in environments with multiple teams or projects
    * Can use resources quota to divide cluster resources between multiple users
    * names of resources need to be unique within specific namespaces
    * by default, a **default** namespace and a `kube-system` namespace exist
    * `k create ns`, `k get --all-namespaces`
    * `kubectx` switch between namespaces
* Context
    * Consists of the cluster name and namespace that the current user connects to
    * is set in the user settings, use `k config current-context` to show current context
    * if multiple cluster are available to a client, switch context is relevant
    * if multiple namespace exist within a cluster, switching namespace is relevant
    * `kubens` provides a shell script that makes switch between namespaces easier

## Architecture

* Node (or Worker machine)
    * made of
        * Container run-time: needs to be installed in each node (Docker, etc.)
        * Kublet service: is the communication interface between master and node
            * An agent that runs on each node in the cluster. It makes sure that containers are running in a Pod
        * KubeProxy forward the request with low network overhead
* Master Node
    * made of
        * Api server: the cluster gateway, gatekeeper (kubectl endpoint)
        * Scheduler is responsible to decide which node to put the next pod taking into account the available resources on each Node
        * Controller manager is responsible to detect POD state changes (up/down), recover state calling scheduler, watches the shared state of the cluster through the apiserver and makes changes attempting to move the current state towards the desired state
        * etcd (the cluster brain) is a k/v store pods states provide information to controller manager
    * multiple instances of master with distributed etcd
    * Need less CPU, RAM and storage as oppose as worker nodes

## Managing updates and rollbacks

Given `k create deployment rollnginx --image=ngninx:1.8`

* `k rollout history deployment`
* `k edit deployment.apps rollenginx` and change version to 1.15
* `k rollout history deployment` now it shows 2 revisions
* `k rollout history deployment rollnginx -revision=1`
* `k rollout history deployment rollnginx -to-revision=1`

## Creating yaml file from dry-run

`k create deployment whatever-name --image={image_name} --dry-run=client --output=yaml > {file_name}.yaml && vi {file_name}.yaml` 


    
## Sources

* https://kubernetes.io/docs/concepts/