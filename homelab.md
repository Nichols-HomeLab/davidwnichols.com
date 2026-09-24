---
title: Homelab — Kubernetes, GitOps and Storage
description: My current self-hosted services, hardware, Kubernetes platform and backup architecture.
published: true
date: 2026-09-24T21:36:08.511Z
tags: homelab, infrastructure
editor: markdown
dateCreated: 2026-09-24T21:36:08.511Z
---

# Nichols Homelab

The lab runs self-hosted applications on **Kubernetes with Talos Linux**, with **FluxCD** reconciling deployments from Gitea. Proxmox provides virtualization, the MS-01 Ceph cluster supplies shared block and filesystem storage, Unraid holds bulk files, and a separate Supermicro system provides the PBS disk-backup and tape tier.

The repository is still named **`k3s-fluxcd`**, reflecting the platform's history. The current nodes run Talos Kubernetes; the repository name does not mean they run the K3s distribution. Docker Swarm and the custom HiveMind controller belong to an earlier phase of the lab.

## Platform at a glance

| Layer | Current design |
| --- | --- |
| Application runtime | Nine Talos Kubernetes nodes: three control-plane nodes and six workers |
| GitOps | FluxCD, Kustomize and Helm; Gitea `Nichols-HomeLab/k3s-fluxcd`, branch `main` |
| Virtualization | Three MS-01 Proxmox hosts, plus the standalone Supermicro backup host |
| Cluster storage | External Ceph integrated with Kubernetes through Rook and CSI; RBD and CephFS |
| Bulk storage | Tower / Unraid for media, files, games and retained data |
| Backup storage | Supermicro NAS with 8 × 4 TB HDDs in RAIDZ3, plus a mirrored special vdev |
| Tape | PBS-managed LTO-8 tape backup tier |
| Networking | Cilium, MetalLB, Traefik, cert-manager and Technitium DNS |
| Access | Authentik for centralized authentication; application-specific access policies |
| Operations | Prometheus, Grafana, VictoriaMetrics, Uptime Kuma, ntfy and supporting monitoring tools |

## Hardware and storage roles

The **three MS-01 systems** remain the primary Proxmox and Ceph tier. A private Thunderbolt/USB4 ring carries storage traffic between them. The **Lenovo M700, M900 and Neo systems** provide the six current Kubernetes workers.

**Tower** is the bulk-storage server. Its NFS-backed volumes hold larger application datasets such as media, cloud files and games. Kubernetes mounts these through persistent volumes, with the NFS CSI driver available for the retained static exports.

The **Supermicro NAS** is a separate backup system. Its eight 4 TB disks provide **32 TB of raw decimal disk capacity**. The RAIDZ3 data-vdev layout leaves a nominal five disks' worth, approximately **20 TB before filesystem overhead and reserves**; this is not a free-space measurement. The PBS guest uses the `backup` ZFS pool and the `local-hdd` datastore. Tape provides another backup tier beyond disk.

## How a change reaches an application

1. Update the application's manifests in `Nichols-HomeLab/k3s-fluxcd` on Gitea.
2. Validate the Kustomize or Helm configuration and commit the change.
3. Push the approved change to `main`.
4. Flux fetches the revision and reconciles the affected resources.
5. Check reconciliation, workload readiness and the application's behavior.

Talos nodes are immutable. Kubernetes and `talosctl` manage them; host package managers, systemd and a host Docker daemon are not their operating model. Ansible remains useful for Proxmox and other supported hosts, with infrastructure automation in `HomeLab`.

## Snapshot and limits

Checked **September 24, 2026**: all nine Kubernetes nodes were Ready, running Kubernetes **v1.36.1** and Talos **v1.13.8**. Flux had fetched Gitea `main` at `908c162c4cba06d2c64c0eb0f4fb2786ae1b0f1d`. Some application and dependency reconciliations were not Ready, so this is an architecture description rather than a claim that every service is healthy.

The Supermicro PBS guest was stopped when checked following the September 23–24 storage-controller incident. The saved incident record reports that the HBA remained absent after a host restart. The disk and tape tiers describe the installed design; current backup availability and integrity require recovery verification.



## Running services

Observed September 24, 2026. These groups had ready Kubernetes workload replicas at the check; readiness is not an end-to-end login, playback or restore test.

| Area | Deployed services with ready replicas |
| --- | --- |
| Media and requests | Plex, Jellyfin, Seerr, Tautulli, Jellystat, Kavita, Shelfmark |
| Media automation | Sonarr, Radarr, Prowlarr, Bazarr, Agregarr, Maintainerr, Profilarr, Pulsarr, Kometa |
| Downloads and music | qBittorrent, SABnzbd, Unpackerr, Flood, Bitmagnet, SpotDL |
| Files, photos and documents | Nextcloud, Collabora, Immich, Paperless-ngx, Paperless AI, Picsur, Copyparty, Overleaf |
| AI and tools | Open WebUI, Bifrost, Tabletop AI DJ, Omni Tools, Morphos, Open Terminal |
| Development and access | Gitea, Termix, PrivateBin, Vaultwarden, Wiki.js, Terrakube |
| Printing and home systems | BambuBuddy, Orca slicer API, Spoolman, ESPHome, Frigate, UpSnap |
| Personal utilities | Homebox, Wallos, SparkyFitness, ChangeDetection, speed-test tools |
| Games | Valheim Plus, GameVault |
| Operations | Authentik, Grafana, Prometheus, VictoriaMetrics, Uptime Kuma/AutoKuma components, ntfy |

At the same check, **RomM, OMP web and Loki had no ready replicas**. **MeshCentral, SearXNG and Wizarr were scaled to zero** in their observed Deployments. These are dated observations rather than a permanent availability statement. Older references to active OpenHands or Minecraft instances should be treated as project/history descriptions unless a current deployment is verified.


## What I build and learn here

The lab is a working environment for application delivery, infrastructure automation, AI integrations and recovery engineering. Projects include Kubernetes deployment automation, CephFS/RBD backups, AI coding-agent environments, BambuBuddy integrations and monitoring across software and hardware.

Infrastructure changes are kept in Git, validated and checked against the running environment. The setup provides practical experience with distributed storage, stateful applications, networking, observability and the limits of automation during hardware failures.

See the [detailed homelab wiki](https://wiki.nicholstech.org/home/homelab/overview), [hardware documentation](https://wiki.nicholstech.org/home/hardware) and [backup records](https://wiki.nicholstech.org/home/storage/backups). Return to [my profile](/home) or explore [selected projects](/projects).
