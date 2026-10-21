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
k apply -f apps/website/ns.yaml
k apply -f apps/website/
```

#### LittleLink

```sh
k apply -f apps/littlelink/ns.yaml
k apply -f apps/littlelink/
```

#### Gateway API

See: <https://docs.nginx.com/nginx-gateway-fabric/install/helm/>

Install Gateway API CRDs

```sh
kubectl kustomize "https://github.com/nginx/nginx-gateway-fabric/config/crd/gateway-api/standard?ref=v2.8.0" | kubectl apply -f -
```

Install Nginx Controller

```sh
helm install ngf oci://ghcr.io/nginx/charts/nginx-gateway-fabric --create-namespace -n nginx-gateway
```

Deploy Gateway (auto provisioning of cloud load balancer)

```sh
k apply -f infra/gateway/ns.yaml
k apply -f infra/gateway/gtw.yaml
```

**On Local Cluster:**

```sh
k port-forward -n website deployment/website 3000:3000
```

```sh
brew install cloud-provider-kind
sudo cloud-provider-kind --gateway-channel="standard"
```

Open [localhost:3000](localhost:3000).

**On Production Cloud Cluster**
Get the External IP of the LoadBalancer service and connect to it via HTTP.
Should also be able to connect via domain name on HTTP, but not yet on HTTPS.

## Configure TLS

1. Deploy cert manager

See installation instructions: <https://cert-manager.io/docs/installation/helm/>

```sh
helm install cert-manager oci://quay.io/jetstack/charts/cert-manager --version v1.21.2 --namespace cert-manager --create-namespace --set crds.enabled=true --set config.enableGatewayAPI=true
```

2. Configure Let's Encrypt as an issuer

```sh
k apply -f infra/cert-manager/
```

---

- Note: every app in my K8s cluster will receive its own namespace

## Destroy

```sh
kind delete clusters phils-cluster
```

## Next Steps

- [ ] create a 3 node VM based cluster where I manually install kubeadm
- [ ] define all my infrastructure via Terraform or Pulumi
  - [ ] define DNS records via IaC
