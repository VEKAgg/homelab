# 🏠 VEKA Homelab: Self‑Hosted Private Cloud & Content Creation Hub

<div align="center">

![Homelab Status](https://img.shields.io/badge/status-running-brightgreen)
![Services](https://img.shields.io/badge/services-15%2B-blue)
![Uptime](https://img.shields.io/badge/uptime-home--grade%20SLA-lightgrey)
![Domain](https://img.shields.io/badge/domain-veka.gg-orange)

*Gaming PC turned enterprise-style homelab for content creation, self‑hosting, and infra learning.*

📖 Documentation • 🚀 Getting Started • 📊 Services • 🛠️ Hardware • 🔍 Infra Notes

</div>

---

## 🎯 Project Vision

VEKA Homelab transforms a gaming PC into a comprehensive **self‑hosted platform** behind **veka.gg**.  
It’s built to practice real‑world infrastructure: virtualization, storage, container platforms, monitoring, CI/CD, and secure remote access.

### ✨ Core Focus Areas

- 🎮 **Content Creation** – Dual-PC streaming, RTMP ingest, future headless OBS
- 🌐 **Web & APIs** – Hosting SvelteKit (shafaat.net / veka.gg) and microservices
- 📷 **Photo Management** – Self-hosted Google Photos alternative (Immich)
- ☁️ **Private Cloud** – Nextcloud, future document/paperless stack
- 🎬 **Media & Games** – Media server, Pterodactyl for game servers
- 📈 **Monitoring** – Grafana + Prometheus on Proxmox + Docker
- 🔒 **Privacy & Control** – Data on my disks, under my network policies

---

## 📊 Current Services

> **Note:** URLs are illustrative; exact hostnames/ports can change.

| Service | Status | URL / Access | Purpose |
|--------|--------|-------------|---------|
| 🐳 **Portainer** | ✅ Active | `https://portainer.veka` | Docker container management |
| 🧱 **Nginx Proxy Manager** | ✅ Active | `https://proxy.veka` | Reverse proxy + SSL + routing |
| 📡 **Cloudflare Tunnel** | ✅ Active | Cloudflare Zero Trust | Secure ingress for public domains |
| 🖼️ **Immich** | ✅ Active | `https://photos.veka.gg` | AI photo management (Google Photos alt) |
| ☁️ **Nextcloud** | 🔧 In Use | `https://cloud.veka.gg` | File sync and personal cloud |
| 🧮 **Grafana** | ✅ Active | `https://dash.veka.gg` | Dashboards for homelab metrics |
| 📡 **Prometheus** | ✅ Active | Internal | Metrics collection (Proxmox, Docker, TrueNAS) |
| 💾 **TrueNAS SCALE** | ✅ Active | `https://truenas.veka` | ZFS storage (RAID-Z1) for services/data |
| 🧊 **Pi‑hole** | ✅ Active | `https://pihole.veka` | DNS + ad/tracker blocking, `.veka` domains |
| 🌐 **SvelteKit Portfolio** | ✅ Active | `https://shafaat.net` / `https://veka.gg` | Personal portfolio & hub |
| 🤖 **Discord Bot** | ✅ Active | Internal | Community / automation bot (Python) |
| 🎮 **Pterodactyl Panel** | 🔧 Setup | `https://game.veka.gg` | Game server management |
| 🧪 **Test Services** | 🔁 Rotating | Internal | Dev/testing containers, experiments |

### 🚧 In Development

- **OBS Headless VM** – Dedicated encoder for streaming
- **Dedicated “Games” VM** – Factorio / Minecraft via Docker with isolated resources
- **Paperless‑ngx** – Document and receipt management
- **Vaultwarden** – Password manager
- **Home Assistant** – Smart home hub

### 📋 Planned Services

- **Jellyfin** – Media server (`media.veka.gg`)
- **Ollama** – Local AI models
- **Mailcow / Mail solution** – Self-hosted email (experimental)
- **Dashcam / IRL streaming tools** – Future projects

---

## 🛠️ Hardware & Platform

### Main Server: Gaming PC Repurposed

```yaml
CPU:          Intel i7-7700K (4c/8t, Kaby Lake)
Motherboard:  ASUS ROG Strix Z270E Gaming
RAM:          64GB DDR4
GPU:          NVIDIA GTX 1080 Ti (for transcoding/ML/encode)
Storage:
  - Proxmox boot:           SSD/HDD
  - TrueNAS pool:           4 x 4TB HDD (RAID-Z1, ~10TB usable)
Network:      Gigabit LAN (2.5G / 10G ready)
Hypervisor:   Proxmox VE
Power:        Gaming PSU (~400W under load)
```

### Virtualization Layout

- **Proxmox VE Host** – As thin as possible; runs:
  - **TrueNAS SCALE VM** – ZFS pool + SMB/NFS shares
  - **Docker LXC (101, “docker-host”)** – All core services
  - Future VMs (games, headless OBS, misc)

---

## 💾 Storage & TrueNAS Strategy

### ZFS Pool & Shares

TrueNAS SCALE VM with:

- **Pool**: RAID‑Z1 (4×4TB) for redundancy
- **Datasets / SMB shares** (examples):

```text
/mnt/tank/docker-volumes   # Generic Docker volumes
/mnt/tank/documents        # General docs
/mnt/tank/immich-photos    # Immich photo/video library
/mnt/tank/media            # Media / downloads
/mnt/tank/nextcloud        # Nextcloud data
/mnt/tank/pterodactyl      # Game server data
```

(Exposed via SMB/NFS and mounted into Docker host.)

### Storage Policy

- **Local (Docker host)**:
  - Databases (PostgreSQL, MariaDB) – low-latency, no NFS locking issues
  - Config directories and critical metadata
- **TrueNAS (NAS)**:
  - Heavy media (Immich photos, videos)
  - Nextcloud user data
  - Pterodactyl game server files
  - Long‑term backups / archives

### Capacity & Upgrade Plan

- Using **TrueNAS community edition**.
- Current pool is enough for personal workloads.
- Larger expansion (more drives / higher capacity) is deferred until:
  - Used/enterprise disk prices normalize post‑“AI bubble”
  - The platform is stable enough to confidently host data for family & close friends

---

## 🧱 Docker, LXC & Security

### Docker-in-LXC Design

Docker runs inside a Proxmox LXC (101):

- LXC is configured with:
  - `nesting=1`, appropriate cgroup settings
  - AppArmor workarounds: `lxc.apparmor.profile: unconfined`
- Docker daemon configured with:
  - `security-opt: ["apparmor=unconfined"]` to avoid profile load errors in LXC

### Container Roles

- **Core reverse proxy stack**: Nginx Proxy Manager + Cloudflare Tunnel sidecar
- **App stack**: Immich, Nextcloud, SvelteKit site, Discord bot, Pterodactyl, Pi-hole, etc.
- **Monitoring stack**: Prometheus, Grafana, exporters

---

## 🌐 Networking, DNS & Ingress

### Domain & Local DNS

- **Primary domain**: `veka.gg`
- **Local resolution**: Pi‑hole providing `.veka` names for LAN:
  - `pve.veka`, `docker.veka`, `truenas.veka`, etc.
  - Service aliases: `photos.veka`, `cloud.veka`, `dash.veka`, etc.

### External Access Model

- **Cloudflare Tunnel** (Zero Trust):
  - Single `cloudflared` container
  - One tunnel, many hostnames (photos.veka.gg, cloud.veka.gg, etc.)
  - No inbound ports opened on the home router

- **Nginx Proxy Manager**:
  - Terminates HTTPS (Let’s Encrypt certificates)
  - Routes requests to internal services by hostname
  - Central place to manage URLs, SSL, redirects

### Docker Network Planning

- Standardized Docker networks to avoid overlapping subnets across stacks (fixed issues where Pterodactyl + NPM competed for the same ranges).

---

## 📈 Monitoring & Observability

Monitoring is **active**, not just “planned”.

### Prometheus

- Runs in Docker, scrapes:

  - Proxmox node (exporter)
  - Docker LXC (node_exporter)
  - Containers (cAdvisor / service-specific exporters)

### Grafana

- Dashboards for:

  - CPU/RAM/disk usage on Proxmox and Docker LXC
  - TrueNAS pool capacity & health (via collected metrics)
  - Container health (Immich, Nextcloud, NPM, etc.)
  - Traffic and workload trends

- Used to:
  - Diagnose Immich ML spikes and RAM usage
  - Observe Nextcloud scans and disk I/O
  - Confirm stability after changes or updates

---

## 🌐 Web & CI/CD

### Portfolio / Website (shafaat.net / veka.gg)

- **Stack**: SvelteKit 5 + TypeScript + TailwindCSS (Tron / cyberpunk aesthetic)
- **Runtime**: Node adapter in Docker (exposed via Nginx Proxy Manager)
- **Extras**:
  - Nodemailer contact endpoint
  - Brotli + Gzip precompression
  - SEO meta tags, OG/Twitter cards

### CI/CD

- **GitHub Actions**:
  - On push to `main`, build and deploy updated images
  - Run simple health checks against service URL
  - If deploy fails, rollback to previous version
- Used initially for the website, extendable to other apps.

---

## 🧩 Example Architecture Diagram

```text
┌─────────────────────────────────────────────────────────┐
│                     Proxmox VE Host                    │
├─────────────────────┬───────────────────────────────────┤
│ TrueNAS SCALE VM    │   LXC 101: docker-host           │
│ ZFS Storage         │   Docker Engine                  │
│                     │                                   │
│  ZFS Pool (RAID-Z1) │   Core Services:                  │
│  ├─ docker-volumes  │   ├─ Nginx Proxy Manager          │
│  ├─ immich-photos   │   ├─ Cloudflare Tunnel            │
│  ├─ nextcloud       │   ├─ Immich                       │
│  ├─ media           │   ├─ Nextcloud                    │
│  └─ pterodactyl     │   ├─ Grafana + Prometheus         │
│                     │   ├─ Pi-hole                      │
│                     │   ├─ SvelteKit site               │
│                     │   └─ Discord bot / misc services  │
├─────────────────────┴───────────────────────────────────┤
│            Future VMs: Games, OBS Headless, etc.       │
└─────────────────────────────────────────────────────────┘
```

---

## 🎯 Project Roadmap (Reality‑Adjusted)

### ✅ Phase 1: Foundation (Complete)

- [x] Proxmox VE on gaming PC
- [x] TrueNAS SCALE VM with RAID‑Z1 pool
- [x] Docker LXC (101) with AppArmor fixes
- [x] Pi-hole DNS and `.veka` domains
- [x] Cloudflare Tunnel + Nginx Proxy Manager

### ✅ Phase 2: Core Services (Mostly Done)

- [x] Immich for photos (on TrueNAS shares)
- [x] Nextcloud for file sync
- [x] SvelteKit portfolio + GitHub Actions CI/CD
- [x] Grafana + Prometheus monitoring stack
- [x] Discord bot, misc microservices
- [ ] Pterodactyl + game server storage on TrueNAS (in use / tuning)

### 🚧 Phase 3: Media & Streaming

- [ ] OBS headless VM for encoding
- [ ] RTMP ingest pipeline for IRL / mobile streaming
- [ ] Jellyfin media server
- [ ] Dedicated “games” VM for Factorio / Minecraft

### 📋 Phase 4: Automation & AI

- [ ] Home Assistant for smart home
- [ ] Paperless‑ngx for documents
- [ ] Vaultwarden for secrets
- [ ] Ollama and local LLM experiments

---

## 🔧 Key Technologies

| Technology | Role |
|-----------|------|
| **Proxmox VE** | Hypervisor, VM/LXC management |
| **TrueNAS SCALE (Community)** | ZFS storage, NAS, SMB/NFS exports |
| **Docker + Docker Compose** | Container orchestrator for services |
| **Nginx Proxy Manager** | Reverse proxy, SSL, virtual hosts |
| **Cloudflare Tunnel** | Secure external access, no open ports |
| **Pi-hole** | DNS, ad blocking, internal domains |
| **Prometheus + Grafana** | Metrics & dashboards |
| **SvelteKit / Node / PM2** | Web apps and bots |

---

## 🧠 Why This Homelab Matters

This build is not just about “running a few containers”:

- It mirrors patterns from real infrastructure: **storage policy, monitoring, ingress, CI/CD, identity, and segmentation**.
- It supports experimentation without risk: break things here before touching production.
- It’s a portfolio piece for **IT infrastructure / SRE / datacenter roles**, showing:
  - How services are designed, not just installed
  - How observability and reliability are baked in
  - How costs and hardware timing (e.g., AI‑inflated disk prices) influence design decisions

---

## 📞 Connect

- 🌐 **Main Site**: https://shafaat.net (hosted on this homelab)
- 💻 **GitHub**: https://github.com/VEKAgg
- 🔐 **Access**: Remote access via Tailscale + Cloudflare Zero Trust

---

<div align="center">

**Built for learning, reliability, and long‑term ownership of data.**  
*Turning a gaming PC into real infrastructure, one container at a time.*

_Last Updated: June 18, 2026_

</div>
