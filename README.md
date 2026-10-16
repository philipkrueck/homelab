# homelab

## Introduction

This repo contains all the configuration as well as documentation for my homelab.

The purpose of my homelab is to learn and have fun.

Self hosting using Kubernetes requires me to think about security, scalability,
maintenance and deployment strategies.

## Cluster Architecture

Local kind cluster defined in [kind-cluster.yaml].

## Bootstrap

### Local Cluster

```sh
kind create cluster --config kind-cluster.yaml
```

### Production Cloud Cluster

I've deployed a managed K8s on CloudFleet using Hetzner for the underlying node pool.

See: <https://cloudfleet.ai/docs/introduction/getting-started/>

### Apps

#### Website

```sh
k apply -f apps/website/namespace.yaml
k apply -f apps/website/deployment.yaml
k apply -f apps/website/loadbalancer.yaml
```

**On Local Cluster:**

```sh
k port-forward -n website deployment/website 3000:3000
```

Open [localhost:3000](localhost:3000).

**On Production Cloud Cluster**
Get the External IP of the LoadBalancer service and connect to it via HTTP.

---

- Note: every app in my K8s cluster will receive its own namespace

## Destroy

```sh
kind delete clusters phils-cluster
```

## Next Steps

- [ ] create a 3 node VM based cluster where I manually install kubeadm
