---
title: Selected Technical Projects
description: AI agents, Kubernetes, Ceph backups, printer integration and hardware automation.
published: true
date: 2026-09-24T21:36:08.511Z
tags: projects, ai, infrastructure, automation
editor: markdown
dateCreated: 2026-09-24T21:36:08.511Z
---

# Selected Technical Projects

## Kubernetes and GitOps homelab

I design and maintain a self-hosted environment for Kubernetes, distributed storage, networking, AI integrations and infrastructure automation. The current platform runs Talos Kubernetes with FluxCD and Gitea; `k3s-fluxcd` is the retained configuration-repository name.

The hardware spans three MS-01 Proxmox/Ceph systems, six Lenovo workers, Unraid bulk storage and a Supermicro backup tier with eight 4 TB disks and LTO-8 tape. Ansible and Terraform support infrastructure work, while Git-based validation and reconciliation make application changes repeatable.

See the [current services, hardware and architecture](/homelab), including dated availability notes.

## Containerized AI coding-agent platform

Built and maintained a persistent OpenHands-based engineering environment for working with local Git repositories. Integrated CephFS workspace storage, Docker sandbox execution and browser-based development, with configurable providers including OpenAI, Claude, Ollama, llama.cpp and OpenRouter-compatible endpoints.

This describes a built project; the current running-service inventory is maintained separately on the [homelab page](/homelab).

## CephFS / RBD backup container

Built a containerized backup platform for filesystem and block-device protection. Automated CephFS `.snap` snapshots and RBD snapshots/diffs, with full and incremental exports, retention, pruning, checksums, verification, logs, archival and notifications. Environment-variable configuration makes the tooling reusable across deployments.

The current lab also uses PBS disk and tape workflows. Project capabilities and current verified backup coverage are separate; see the [backup documentation](https://wiki.nicholstech.org/home/storage/backups).

## BambuBuddy — multi-printer and multi-slicer integration

Forked and extended a Python-based printing platform beyond Bambu Lab hardware. Added Snapmaker U1 integration through Moonraker and support for simultaneous Bambu and Snapmaker slicing backends. Extended filament inventory, live feeder management, multi-printer workflows and upstream/dependency synchronization.

## Lenovo IPMI GPU fan control

Developed a containerized thermal-management service that reads NVIDIA GPU temperatures through `nvidia-smi` and controls server fan speeds through IPMI using a configurable fan curve. The project combines GPU telemetry, Linux, monitoring and hardware automation.

[Profile](/home) · [Professional experience](/experience) · [Current homelab](/homelab)
