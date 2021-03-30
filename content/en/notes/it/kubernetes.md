---
title: Kubernetes
linktitle: Kubernetes
toc: true
type: book
weight: 2
---

All stuff related to Kubernetes.
The stuff thing everybody wants to run but a few people know about what it actually does.
It is a damn complex thing.
Heard it be called the operating system for the cloud.

## Find Kubernetes resources that have been deprecated

This is a very simple utility to help users find deprecated Kubernetes apiVersions in their code repositories and their helm releases.

[pluto](https://github.com/FairwindsOps/pluto/)

## Installing Kubernetes

- kubeadm

## Monitoring

Typical monitoring stack

- Prometheus
- Grafana
- alertmanager

### prometheus-am-executor

A small binary which will let you run scripts on alerts by alertmanager

[prometheus-am-executor](https://github.com/imgix/prometheus-am-executor)

## Ingress Controller

### nginx-ingress-controller

There are 2 NGINX Ingress Controllers which do not share the same features and config!

#### NGINX Ingress Controller by the Kubernetes community

- Repo
- Helm-Chart
- Docs

#### NGINX Ingress Controller by NGINX INC

- Repo
- Helm-Chart
- Docs

## Kubernetes Package and Resource management

- [Helm - The package manager for Kubernetes](https://helm.sh/)
- [Artifact Hub](https://artifacthub.io/) Former Helm Hub which is deprecated
- [chartmuseum git repo](https://github.com/helm/chartmuseum) Hosting your own helm-repos

## learning Resources

- Kubernetes edx course
- Kubernetes course udemy

## Useful Tools for daly operation

- [kubectx & kubens](https://github.com/ahmetb/kubectx) easy cluster and namespace context switching with shell promt support 
- [stern](https://github.com/wercker/stern) Tail and follow multiple pods on Kubernetes and multiple containers within the pod
- [kubetail](https://github.com/johanhaleby/kubetail) Bash script that enables you to aggregate (tail/follow) logs from multiple pods into one stream. This is the same as running "kubectl logs -f " but for multiple pods.

## Certifcate management

- [cert-manager website](https://cert-manager.io/)
- [cert-manager git repo](https://github.com/jetstack/cert-manager)
- [cert-manager helm chart](https://artifacthub.io/packages/helm/jetstack/cert-manager)


## Running kubernetes

[Reserve Compute Resources for System Daemons](https://kubernetes.io/docs/tasks/administer-cluster/reserve-compute-resources)

[Node Allocatable Resources](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/node/node-allocatable.md)

[Configure Out of Resource Handling](https://kubernetes.io/docs/tasks/administer-cluster/out-of-resource/)

[Container runtimes](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)

https://github.com/kubernetes/kubeadm/issues/1394
https://github.com/systemd/systemd/issues/8645


cgroup list:

`systemctl status`
`systemd-cgls`
