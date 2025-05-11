# homelab

## Introduction

This repo contains all the configuration as well as documentation for my homelab.

The purpose of my homelab is to learn and have fun.

Self hosting using Kubernetes requires me to think about security, scalability, maintenance and deployment strategies.

## GitOps

I aim to apply all changes via GitOps rather than running manual commands on the cluster.

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
        <td><img width="32" src="https://linkding.link/_astro/logo.DkvM5cgj.svg"></td>
        <td><a href="https://linkding.link/">Linkding</a></td>
        <td>My browser-agnostic bookmark manager.</td>
    </tr>
    <tr>
        <td><img width="32" src="https://play-lh.googleusercontent.com/9Qjh1GhRcPcUOQLTt-DdnYV2PS9ENidfvkGZ602QWF36KGvLogzcJwCaKTcWBytVGktP"></td>
        <td><a href="https://www.audiobookshelf.org/">Audiobookshelf</a></td>
        <td>My audio book library.</td>
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
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/cloudflare-zero-trust.png"></td>
        <td><a href="https://developers.cloudflare.com/cloudflare-one/">Cloudflare Tunnels</a></td>
        <td>I don't really need tunnels since my VM has a public IP. But they are amazing if I setup some hardware in my private network.</td>
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
</table>

## Setup

I'm currently running the cluster on 2 unmanaged virtual machines - one master and one worker node provisioned using [kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/).

### Virtual Machine Specs

Both the master and worker node have the same specs:

- RAM = 4GiB
- disk = 120GiB SSD
- CPU = 2 vCPU
- OS = Ubuntu 20.04

### Network Access

Both machines require open SSH access (`TCP:22`).
The master node also requires an open `TCP:6443` firewall rule for the Kube API Server.
Services are exposed on ports `30000`-`40000` so these will be opened as needed.

### Cluster Setup Instructions

**1. Setup Master Node**

1. SSH into Master Node.
2. Login as root: `sudo -i`.
3. Execute the setup script:

```sh
bash <(curl -s https://raw.githubusercontent.com/philipkrueck/homelab/refs/heads/main/setup/install-master.sh)
```

4. Take note of the join command in the output. It should look something like this:

```sh
kubeadm join <masterIP>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>
```

## Future Plans

- Deploy some more cool open source applications like [Homepage](https://github.com/gethomepage/homepage)
- Deploy my [personal website](https://philipkrueck.com) and [solstamp.io](https://solstamp.io)
- Add a back up solution
- Ability to restore all data & state from blob storage
