# homelab

## Introduction

This repo contains all the configuration as well as documentation for my homelab.

The purpose of my homelab is to learn and have fun.

Self hosting using Kubernetes requires me to think about security, scalability, maintenance and deployment strategies.

## GitOps

I aim to apply all changes via GitOps rather than running manual commands on the cluster.

To this end I'm using [FluxCD](https://fluxcd.io/).

## Kubernetes Deployment

I'm currently running the cluster as a [k3s](https://k3s.io/) single node cluster on a Virtual Machine in the cloud.

I plan to go multi-node, multi-cloud in the future, and potentially even setting up hardware at home.

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
        <td>Ingress Controller of choice</td>
    </tr>
    <tr>
        <td><img width="32" src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/cloudflare-zero-trust.png"></td>
        <td><a href="https://developers.cloudflare.com/cloudflare-one/">Cloudflare Tunnels</a></td>
        <td>I don't really need tunnels since my VM has a public IP. Should I switch to my own hardware, it might be nice to use tunnels though.</td>
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

## Future Plans

- Deploy some more cool open source applications like [Homepage](https://github.com/gethomepage/homepage)
- Deploy my [personal website](https://philipkrueck.com) and [solstamp.io](https://solstamp.io)
- Add a back up solution
- Ability to restore all data & state from blob storage
