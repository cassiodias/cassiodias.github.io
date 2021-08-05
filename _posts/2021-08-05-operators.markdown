---
layout: post
title:  "Kubernetes Operators"
date:   2021-07-12 20:41
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


![Kubernetes: Resources + Controllers](https://www.redhat.com/cms/managed-files/styles/wysiwyg_full_width/s3/image3_35_0.png?itok=zAn9Qoa-)

_[source: Red Hat](https://www.redhat.com/en/blog/operators-over-easy-introduction-kubernetes-operators)_