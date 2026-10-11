# homelab

## Introduction

This repo contains all the configuration as well as documentation for my homelab.

The purpose of my homelab is to learn and have fun.

Self hosting using Kubernetes requires me to think about security, scalability,
maintenance and deployment strategies.

## Cluster Architecture

Local kind cluster defined in [kind-cluster.yaml].

## Bootstrap

### Cluster

```sh
kind create cluster --config kind-cluster.yaml
```

### Apps

#### Website

```sh
k apply -f apps/website/namespace.yaml apps/website/deployment.yaml
```

---

- Note: every note in my K8s cluster will receive its own namespace

## Destroy

```sh
kind delete clusters phils-cluster
```

## Next Steps

- [ ] create a 3 node managed Kubernetes cluster as described in "The Kubernetes book" by Nigel Poulton
- [ ] create a 3 node VM based cluster where I manually install kubeadm
