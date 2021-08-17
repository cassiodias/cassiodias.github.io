---
layout: post
title:  "KUBERNETES CHEAT SHEET"
date:   2021-09-17 15:30
categories: generic
---

* Do not remove this line (it will not be displayed)
{:toc}


# VIEWING RESOURCE INFORMATION

## Nodes

````
kubectl get no
kubectl get no -o wide
kubectl describe no
kubectl get no -o yaml
kubectl get node --selector=[label_name]
kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="ExternalIP")].address}'
kubectl top node [node_name]
```

## Pods

```
kubectl get po
kubectl get po -o wide
kubectl describe po
kubectl get po --show-labels
kubectl get po -l app=nginx
kubectl get po -o yaml
kubectl get pod [pod_name] -o yaml --export
kubectl get pod [pod_name] -o yaml --export > nameoffile.yaml
kubectl get pods --field-selector status.phase=Running
```

## Namespaces


```
kubectl get ns
kubectl get ns -o yaml
kubectl describe ns
```

## Deployments

```
kubectl get deploy
kubectl describe deploy
kubectl get deploy -o wide
kubectl get deploy -o yaml
```

## Services

```
kubectl get svc
kubectl describe svc
kubectl get svc -o wide
kubectl get svc -o yaml
kubectl get svc --show-labels
```

## Daemonsets

```
kubectl get ds
kubectl get ds --all-namespaces
kubectl describe ds [daemonset_name] -n [namespace_name]
kubectl get ds [ds_name] -n [ns_name] -o yaml
```

## Events

```
$ kubectl get events
$ kubectl get events -n kube-system
$ kubectl get events -w
```

## logs

```
kubectl logs [pod_name]
kubectl logs --since=1h [pod_name]
kubectl logs --tail=20 [pod_name]
kubectl logs -f -c [container_name] [pod_name]
kubectl logs [pod_name] > pod.log
```

## Service accounts

```
kubectl get sa
kubectl get sa -o yaml
kubectl get serviceaccounts default -o yaml > ./sa.yaml
kubectl replace serviceaccount default -f ./sa.yaml
```

## Replicasets

```
kubectl get rs
kubectl describe rs
kubectl get rs -o wide
kubectl get rs -o yaml
```

## Roles

```
kubectl get roles --all-namespaces
kubectl get roles --all-namespaces -o yaml
```

## Secrets

```
kubectl get secrets
kubectl get secrets --all-namespaces
kubectl get secrets -o yaml
```

## Configmaps

```
kubectl get cm
kubectl get cm --all-namespaces
kubectl get cm --all-namespaces -o yaml
```

## Ingress

```
kubectl get ing
kubectl get ing --all-namespaces
```

## Persistent volumes

```
kubectl get pv
kubectl describe pv
```

## Persistent volume claims

```
kubectl get pv 
kubectl describe pvc
```

## Storage class

```
kubectl get sc
kubectl get sc -o yaml
```

## Multiple resources

```
kubectl get svc, po
kubectl get deploy, no
kubectl get all
kubectl get all --all-namespaces
```

# CHANGING RESOURCE ATTRIBUTES

## taint

```
kubectl taint [node_name] [taint_name]
```

## Labels

```
kubectl label [node_name] disktype=ssd
kubrectl label [pod_name] env=prod
```

## Cordon/uncordon

```
kubectl cordon [node_name]
kubectl uncordon [node_name]
```

## drain

```
kubectl drain [node_name]

```

## Nodes/pods

```
kubectl delete node [node_name]
kubectl delete pod [pod_name]
kubectl edit node [node_name]
kubectl edit pod [pod_name] 
```

## Deployments/namespaces

```
kubectl edit deploy [deploy_name]
kubectl delete deploy [deploy_name]
kubectl expose deploy [deploy_name] --port=80 --type=NodePort
kubectl scale deploy [deploy_name] --replicas=5
kubectl delete ns 
kubectl edit ns [ns_name]
```

## Services

```
kubectl edit svc [svc_name]
kubectl delete svc [svc_name]
```

# Daemonsets

```
kubectl edit ds [ds_name] -n kube-system
kubectl delete ds [ds_name]
```

## Service accounts

```
kubectl edit sa [sa_name]
kubectl delete sa [sa_name]
```

## Annotate

```
kubectl annotate po [pod_name] [annotation]
kubectl annotate no [node_name]
```

# ADDING RESOURCES

## Creating a pod

```
kubectl create -f [name_of_file]
kubectl apply -f [name_of_file]
kubectl run [pod_name] --image=nginx --restart=Never
kubectl run [pod_name] --generator=run-pod/v1 --image=nginx
kubectl run [pod_name] --image=nginx --restart=Never
```

## Creating a service

```
kubectl create svc nodeport [svc_name] --tcp=8080:80
```

## Creating a deployment

```
kubectl create -f [name_of_file]
kubectl apply -f [name_of_file]
kubectl create deploy [deploy_name] --image=nginx
```

## Interactive pod

```
kubectl run [pod_name] --image=busybox --rm -it --restart=Never -- sh
```

## Output YAML to a file

```
kubectl create deploy [deploy_name] --image=nginx --dry-run -o yaml > deploy.yaml
kubectl get po [pod_name] -o yaml --export > pod.yaml
```

## getting help

```
kubectl -h 
kubectl create -h
kubectl run -h
kubectl explain deploy.spec
```

## API calls

```
kubectl get --raw /apis/metrics.k8s.io/
```

## Cluster info

```
kubectl config 
kubectl cluster-info
kubectl get componentstatuses
```
