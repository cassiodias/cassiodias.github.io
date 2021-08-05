---
layout: post
title:  "Kubernetes Operators"
date:   2021-08-05 18:30
categories: generic
---

* Do not remove this line (it will not be displayed)
{:toc}

# Kubernetes Operators

> Operator is a custom Kubernetes controller watching a Custom Resource type and taking
application-specific actions to make reality (status) match the plan (Spec) in that resource.

## Operators Are Software SREs

An Operator is like an automated Site Reliability Engineer for its application. It enco‐
des in software the skills of an expert administrator. 

An Operator can manage a clus‐ter of database servers, for example. It knows the details of configuring and managing
its application, and it can install a database cluster of a declared software version and
number of members. 

An Operator continues to monitor its application as it runs, and can back up data, recover from failures, and upgrade the application over time, automatically. Cluster users employ kubectl and other standard tools to work with Oper‐
ators and the applications they manage, because Operators extend Kubernetes.

## How Operators Work

Operators work by extending the Kubernetes control plane and API. In its simplest
form, an Operator adds an endpoint to the Kubernetes API, called a custom resource
(CR), along with a control plane component (Controller) that monitors and maintains resources
of the new type.

## Kubernetes CRs

CRs are the API extension mechanism in Kubernetes. A custom resource definition
(CRD) defines a CR; it’s analogous to a schema for the CR data. Unlike members of
the official API, a given CRD doesn’t exist on every Kubernetes cluster. CRDs extend
the API of the particular cluster where they are defined. 

CRs provide endpoints for
reading and writing structured data. A cluster user can interact with CRs with
kubectl or another Kubernetes client, just like any other API resource.

# How Operators Are Made

Kubernetes compares a set of resources to reality; that is, the running state of the
cluster. It takes actions to make reality match the desired state described by those
resources. Operators extend that pattern to specific applications on specific clusters.

An Operator is a custom Kubernetes controller watching a CR type and taking
application-specific actions to make reality match the spec in that resource.

Making an Operator means creating a CRD and providing a program that runs in a
loop watching CRs of that kind. What the Operator does in response to changes in
the CR is specific to the application the Operator manages. 

The actions an Operator performs can include almost anything: scaling a complex app, application version
upgrades, or even managing kernel modules for nodes in a computational cluster
with specialized hardware.

<img src="https://www.redhat.com/cms/managed-files/styles/wysiwyg_full_width/s3/image3_35_0.png?itok=zAn9Qoa-"
     style="float: left; margin-right: 10px;"/>

_[source: Red Hat](https://www.redhat.com/en/blog/operators-over-easy-introduction-kubernetes-operators)_

## How Operators Work 2

Operator is composed of the following components: a Custom Resource (The API extended from k8s); A Resource Controller (who does the work). A CRD allows you to extend the Kubernetes API. A CRD needs a Controller to act upon its presence (i.e., CRD instance). Without a controller for your custom resource, then it’s just a stateless object within Kubernetes.

<img src="https://www.redhat.com/cms/managed-files/styles/wysiwyg_full_width/s3/image2_46.png?itok=RpGomNQ6"
     style="float: left; margin-right: 10px;"/>

_[source: Red Hat](https://www.redhat.com/en/blog/operators-over-easy-introduction-kubernetes-operators)_

### How does a controller work? What does it do? What’s inside of a controller?

> The Reconcile

The Resource Controller helps ensure that the current state of a resource matches the desired state of a resource (the Spec). An example of this is if it was specified the number of pods of a resource through an arbitrary specification/attribute, size, on the custom resource instance. If we increase/decrease the value, then we are setting the desired state.

The following cycle takes place when a resource change event occurs:

**Observe/watch:** In this phase, the controller observes the state of the cluster. Typically this is initiated by observing the events on the custom resource instance. These events are usually subscribed from the custom resource controller. Consider this to be similar to a pub/sub mechanism between the resource controller and cluster.

**Analyze:** In this phase, the resource controller compares the current state of the resource instance to the desired state. The desired state is typically reflective of what is specified in the spec attributes of the resource.

**Act/Reconcile:** In this phase, the resource controller performs all necessary actions to make the current resource state match the desired state. This is called reconciliation, and is typically where operational knowledge is implemented (i.e., business/domain logic).

The reconciliation cycle generally runs until the desired state is achieved or until an error is thrown. It is not uncommon to see the reconciliation cycle run repeatedly as it treats each run as an idempotent process.

