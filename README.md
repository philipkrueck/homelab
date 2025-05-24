# homelab

## Introduction

This repo contains all the configuration as well as documentation for my homelab.

The purpose of my homelab is to learn and have fun.

Self hosting using Kubernetes requires me to think about security, scalability,
maintenance and deployment strategies.

## GitOps

I aim to apply all changes via GitOps rather than running manual commands
on the cluster.

To this end I'm using [FluxCD](https://fluxcd.io/).

## Deployment

### Apps

End User Applications

<table>
    <tr>
        <th>Logo</th>
        <th>Name</th>
        <th>Description</th>
    </tr>
    <tr>
        <td><img width="32" src="https://avatars.githubusercontent.com/u/122929872?s=48&v=4"></td>
        <td><a href="https://linkding.link/">Homepage</a></td>
        <td>Application dashboard serving as a GUI entrypoint into my homelab.                                                  </td>
    </tr>
    <tr>
        <td><img width="32" src="https://linkding.link/_astro/logo.DkvM5cgj.svg"></td>
        <td><a href="https://linkding.link/">Linkding</a></td>
        <td>Browser-agnostic bookmark manager.</td>
    </tr>
    <tr>
        <td><img width="32" src="https://play-lh.googleusercontent.com/9Qjh1GhRcPcUOQLTt-DdnYV2PS9ENidfvkGZ602QWF36KGvLogzcJwCaKTcWBytVGktP"></td>
        <td><a href="https://www.audiobookshelf.org/">Audiobookshelf</a></td>
        <td>Audio book library.</td>
    </tr>
</table>
...more coming soon

### Infrastructure

Everything needed to run my cluster & deploy my applications

<table>
    <tr>
        <th>Logo</th>
        <th>Name</th>
        <th>Description</th>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/svg/flux-cd.svg"></td>
        <td><a href="https://fluxcd.io/">Flux CD</a></td>
        <td>My GitOps solution of choice.</td>
    </tr>
    <tr>
        <td><img width="32" src="https://raw.githubusercontent.com/docker-library/docs/a6cc2c5f4bc6658168f2a0abbb0307acaefff80e/traefik/logo.png"></td>
        <td><a href="https://traefik.io/traefik/">Traefik</a></td>
        <td>My Ingress Controller of choice.</td>
    </tr>
    <tr>
        <td><img width="32" src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ-0ZLIeiGhCbdGZiRAE6AyF-Zl1f1FgEwVKw&s"></td>
        <td><a href="https://traefik.io/traefik/">cert-manager</a></td>
        <td>Certificate Management. I'm using <a href="https://letsencrypt.org/">Let's Encrypt</a> as a CA.</td>
    </tr>
    <tr>
        <td><img width="32" src="https://www.svgrepo.com/download/477066/lock.svg"></td>
        <td><a href="https://github.com/getsops/sops">SOPS</a></td>
        <td>Encryption of Kubernetes Secrets. So that I can store them in this repo.</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/grafana.svg"></td>
        <td><a href="https://grafana.com/">Grafana</a></td>
        <td>Neat visualization of application and cluster state.</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/prometheus.svg"></td>
        <td><a href="https://prometheus.io/">Prometheus</a></td>
        <td>Monitoring and Alerting. Datafeed for Grafana.</td>
    </tr>
    <tr>
        <td><img width="32" src="https://www.svgrepo.com/download/374041/renovate.svg"></td>
        <td><a href="https://github.com/renovatebot/renovate">Renovate</a></td>
        <td>Automated dependency updates.</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/cloudflare-zero-trust.png"></td>
        <td><a href="https://developers.cloudflare.com/cloudflare-one/">Cloudflare Tunnels</a></td>
        <td>I don't really need tunnels since my VM has a public IP. But they are amazing if I setup some hardware in my private network.</td>
    </tr>
</table>

## Setup

I want my homelab to run on various types of Kubernetes setups.

I'm currently running my cluster on a simple single VM [k3s](https://docs.k3s.io),
but planning to switch to the multi-node setup soon.
I've documented both setup options below.

### Single node k3s setup

Follow the [docs for installation](https://docs.k3s.io/quick-start).

Then make sure the `traefik-config.yaml` doesn't
exist in the static manifests directory created by K3s:

```sh
rm /var/lib/rancher/k3s/server/manifests/traefik-config.yaml
```

This helm chart config will be managed through flux instead (see `infrastructure/controllers/base/traefik`).

I plan to deploy Traefik outside of k3s
to make the homelab more portable to different k8s deployments.

### Multi-node kubeadm setup

This setup requires 2 virtual machines -
one master and one worker node provisioned using [kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/).

#### Virtual Machine Specs

Both the master and worker node have the same specs:

- RAM = 4GiB
- disk = 120GiB SSD
- CPU = 2 vCPU
- OS = Ubuntu 20.04

#### Network Access

Both machines require open SSH access (`TCP:22`).
The master node also requires an open `TCP:6443` firewall rule for the Kube API Server.
Services are exposed on ports `30000`-`40000` so these ports will be opened as needed.

#### Cluster Setup Instructions (Multi-node with `kubeadm`)

##### 1. Setup Master Node

1. SSH into Master Node.
2. Login as root: `sudo -i`.
3. Execute the setup script:

```sh
bash <(curl -s https://raw.githubusercontent.com/philipkrueck/homelab/refs/heads/main/setup/install-master.sh)
```

4. Take note of the join command in the output.
   It will be needed in the next steps and should look similar to this:

```sh
kubeadm join <masterIP>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>
```

##### 2. Setup Worker Node

1. SSH into Worker Node.
2. Login as root: `sudo -i`.
3. Execute the setup script:

```sh
bash <(curl -s https://raw.githubusercontent.com/philipkrueck/homelab/refs/heads/main/setup/install-worker.sh)
```

4. Execute the join command that was displayed
   at the end of the output of the setup script on the master.

##### 3. Verify Node Setup

1. Go back to the master node.
2. Login again: `sudo -i`.
3. Check the nodes using the `k=kubectl` alias:

```sh
❯ k get nodes
NAME                STATUS   ROLES                  AGE     VERSION
homelab-master      Ready    control-plane,master   6d21h   v1.32.3
homelab-worker      Ready    worker                 6d21h   v1.32.3
```

- Now the kube config can copied to the local machine to interact
  with the cluster without SSH access.

## Install Flux

1. Obtain a new Personal Access Token

- Go to GitHub Settings > Developer Settings > Personal Access Tokens
- Generate a new classic token with 'repo' permissions
- Store the token in an environment variable:
  `export GITHUB_TOKEN=<your-token>` or in nushell

```sh
export-env { $env.GITHUB_TOKEN = 'ghp_XXXX' }
```

2. Bootstrap flux on cluster

```sh
flux bootstrap github --owner=philipkrueck --repository=homelab --branch=main --path=./clusters/staging --personal --token-auth
```

3. Provide flux with my age secret key

```sh
cat ~/.age/age.key | k create secret generic sops-age --namespace=flux-system --from-file=age.agekey=/dev/stdin
```

## Secrets

Flux has access to my age secret key.
In order to safely commit secrets to this repo,
I use the following command on the raw secret files:

```sh
sops --age=age1vf5v73hyx36z3y398l2n7pxyhznptpl00kkxnuup4vrtnsjpg5tqcperyn --encrypt --encrypted-regex '^(data|stringData)$' --in-place super-secret.yaml
```

## Future Plans

- Deploy some more cool open source applications
- Deploy my [personal website](https://philipkrueck.com) and [solstamp.io](https://solstamp.io)
- Add a back up solution
- Ability to restore all data & state from blob storage
