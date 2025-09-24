# 🏠 VEKA Homelab: Selfhosted Private Cloud & Content Creation Hub

<div align="center">

![Homelab Status](https://img.shields.io/badge/Status-Active%20Development-green)
![Services](https://img.shields.io/badge/Services-3%2F15-blue)
![Uptime](https://img.shields.io/badge/Uptime-Local%20Development-yellow)
![Domain](https://img.shields.io/badge/Domain-veka.gg-purple)

*Gaming PC turned enterprise homelab for content creation and self-hosting*

[📖 Documentation](#documentation) • [🚀 Getting Started](#getting-started) • [📊 Services](#services) • [🛠️ Hardware](#hardware) • [🤝 Contributing](#contributing)

</div>

---

## 🎯 Project Vision

VEKA Homelab transforms a gaming PC into a comprehensive content creation and self-hosting platform. Built around **veka.gg**, this setup supports streaming, web development, photo management, and privacy-focused alternatives to cloud services.

### ✨ Core Focus Areas

- 🎮 **Content Creation**: Dual-PC streaming setup with NDI/RTMP
- 🌐 **Web Development**: Hosting SvelteKit sites and GitHub projects  
- 📷 **Photo Management**: Self-hosted Google Photos alternative
- 🎬 **Media Server**: Personal Netflix with automated downloads
- 🏠 **Home Automation**: Smart home control and monitoring
- 🔒 **Privacy First**: All data stays on your hardware

---

## 📊 Current Services

| Service | Status | URL | Purpose |
|---------|--------|-----|---------|
| 🐳 **Portainer** | ✅ Active | `portainer.veka:9000` | Container management |
| 🛡️ **Pi-hole** | ✅ Active | `pihole.veka:8080` | DNS + Ad blocking |
| 🖼️ **Immich** | 🔧 Setup | `immich.veka:2283` | AI photo management |
| 💾 **TrueNAS** | ✅ Active | `truenas.veka` | Storage management |

### 🚧 In Development

- **VEKA Website** - Main veka.gg SvelteKit site
- **OBS Headless** - Streaming infrastructure 
- **Nginx Proxy Manager** - Reverse proxy with SSL
- **Minecraft Server** - Gaming with friends

### 📋 Planned Services

- **Jellyfin** - Media server (`media.veka.gg`)
- **Nextcloud** - File sync (`cloud.veka.gg`)
- **Home Assistant** - Smart home hub
- **Vaultwarden** - Password manager
- **Grafana + Prometheus** - Monitoring and metrics
- **Paperless-ngx** - Document management
- **Ollama** - Local AI models

---

## 🛠️ Hardware Stack

### **Main Server: Gaming PC Repurposed**
```yaml
CPU:          Intel i7-7700K (4c/8t, Kaby Lake)
Motherboard:  ASUS ROG Strix Z270E Gaming
RAM:          32GB+ DDR4 (expandable)
GPU:          GTX 1080 Ti (available for transcoding)
Storage:      1TB HDD + 4TB drives
Network:      Gigabit LAN (2.5G/10G ready)
Platform:     Proxmox VE
Power:        Gaming PC PSU (~400W under load)
```

### **Storage Strategy**
```yaml
Boot Drive:   1TB HDD (Proxmox host)
Primary:      4TB drives (Docker/VM storage)
Backup:       Cloud sync transition in progress
Future:       Local NAS expansion planned
Features:     ZFS snapshots, compression
```

### **Network Infrastructure**
```yaml
Router:       ISP provided (OPNSense planned)
Domain:       veka.gg + subdomain strategy
VPN:          Tailscale for remote access
Proxy:        Nginx Proxy Manager (planned)
DNS:          Pi-hole with custom .veka domains
```

---

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│                Gaming PC - Proxmox VE                  │
│                  Host System                           │
├─────────────────────┬───────────────────────────────────┤
│   TrueNAS SCALE     │      Docker LXC                  │
│   Storage Manager   │      Service Host                │
│                     │                                  │
│ ┌─────────────────┐ │ ┌──────────────────────────────┐ │
│ │ Storage Pools   │ │ │ Core Services:               │ │
│ │ ├─ media/       │ │ │ ├─ Portainer                 │ │
│ │ ├─ photos/      │ │ │ ├─ Pi-hole                   │ │
│ │ ├─ documents/   │ │ │ ├─ Immich                    │ │
│ │ └─ backups/     │ │ │ └─ VEKA sites...            │ │
│ └─────────────────┘ │ └──────────────────────────────┘ │
├─────────────────────┼───────────────────────────────────┤
│   Arch Linux VM     │      OBS Headless VM            │
│   (Web Hosting)     │      (Content Creation)         │
└─────────────────────┴───────────────────────────────────┘
```

---

## 🎯 Project Roadmap

### ✅ Phase 1: Foundation (Complete)
- [x] Proxmox VE setup on gaming PC
- [x] TrueNAS SCALE for storage management
- [x] Docker container environment (LXC)
- [x] Basic networking and DNS with Pi-hole

### 🚧 Phase 2: Content Creation (In Progress)
- [ ] **VEKA Website Hosting** (veka.gg SvelteKit)
- [ ] **OBS Headless Setup** (dual-PC streaming)
- [ ] **NDI Integration** (laptop → homelab)
- [ ] **RTMP Streaming** (mobile IRL streams)
- [x] **Immich Photo Management** (family photos)

### 📋 Phase 3: Media & Automation (Planned)
- [ ] **Jellyfin Media Server** (movies/TV)
- [ ] **Nextcloud File Sync** (cloud replacement)
- [ ] **Home Assistant** (smart home hub)
- [ ] **Minecraft Server** (mc.veka.gg)

### 🚀 Phase 4: Advanced Features (Future)
- [ ] **Monitoring Stack** (Grafana + Prometheus)
- [ ] **Ollama AI Models** (local LLM hosting)
- [ ] **OPNSense Router** (network upgrade)
- [ ] **Dashcam App** (future project)

---

## 🌐 Domain Strategy

### **Subdomain Planning**
```yaml
veka.gg           # Main website (SvelteKit)
cloud.veka.gg     # Nextcloud file sync
photos.veka.gg    # Immich photo management
media.veka.gg     # Jellyfin media server
mc.veka.gg        # Minecraft server
stream.veka.gg    # OBS/streaming dashboard
home.veka.gg      # Home Assistant
dash.veka.gg      # Grafana monitoring
```

### **Access Strategy**
- **Local Network**: Custom .veka domains via Pi-hole
- **Remote Access**: Tailscale VPN for security
- **Public Services**: Selective exposure via reverse proxy
- **Development**: Local testing environment

---

## 🚀 Getting Started

### **Prerequisites**
- Gaming PC with virtualization support
- Multiple storage drives (4TB+ recommended)
- Stable internet connection
- Domain name (optional but recommended)

### **Quick Start**
```bash
# Clone this repository
git clone https://github.com/VEKAgg/homelab.git
cd homelab

# Review hardware compatibility
cat docs/hardware-requirements.md

# Follow installation guide
docs/installation/proxmox-setup.md
```

---

## 📂 Repository Structure

```
homelab/
├── 📁 docs/                    # Comprehensive documentation
│   ├── hardware-conversion.md  # Gaming PC → Homelab guide
│   ├── streaming-setup.md      # OBS and content creation
│   ├── network-config.md       # Domain and proxy setup
│   ├── installation/           # Step-by-step guides
│   └── troubleshooting.md      # Common issues & solutions
├── 📁 docker-compose/          # Service deployment files
│   ├── media-stack/            # Jellyfin + ARR services
│   ├── content-creation/       # OBS, streaming tools
│   ├── web-hosting/            # VEKA sites and projects
│   ├── immich/                 # Photo management
│   ├── pihole/                 # DNS + Ad blocking
│   └── monitoring/             # Grafana + Prometheus
├── 📁 scripts/                 # Automation scripts
│   ├── stream-setup.sh         # OBS automation
│   ├── backup-configs.sh       # Configuration backup
│   ├── deploy-site.sh          # Website deployment
│   └── update-services.sh      # Bulk service updates
├── 📁 configs/                 # Service configurations
│   ├── nginx/                  # Reverse proxy configs
│   ├── obs/                    # Streaming configurations
│   ├── grafana/                # Monitoring dashboards
│   └── networking/             # Network setup files
└── 📄 README.md               # This file
```

---

## 🔧 Key Technologies

| Technology | Purpose | Why Chosen |
|------------|---------|------------|
| **Proxmox VE** | Hypervisor | Enterprise features, web management |
| **TrueNAS SCALE** | Storage OS | ZFS reliability, data protection |
| **Docker** | Containerization | Easy deployment, isolation |
| **Pi-hole** | DNS/Ad Block | Network-wide ad blocking |
| **Portainer** | Container UI | Visual Docker management |
| **Immich** | Photo Management | Privacy-focused Google Photos alternative |

---

## 🎬 Content Creation Features

### **Dual-PC Streaming Setup**
- **Main PC**: Gaming/content creation workstation
- **Homelab**: OBS encoding and streaming backend
- **Connection**: NDI over gigabit network
- **Backup**: RTMP ingest from mobile (IRL streams)

### **Streaming Infrastructure**
```yaml
OBS Headless:     Proxmox VM for encoding
NDI Source:       Laptop → homelab processing
RTMP Ingest:      Mobile → homelab (IRL streams)
Output Targets:   Twitch, YouTube, custom RTMP
Monitoring:       Real-time stream health
Bitrate Control:  Adaptive for mobile connections
```

### **Web Development Platform**
- **SvelteKit Sites**: Main veka.gg development
- **GitHub Integration**: Automated deployment pipelines
- **Local Testing**: .veka domains for development
- **SSL Certificates**: Let's Encrypt automation

---

## 💡 Key Features & Highlights

### 🔐 Privacy & Security
- **Zero Cloud Dependency**: All services run locally
- **Network Isolation**: Containerized services with security profiles
- **Ad Blocking**: Network-wide ad and tracker blocking
- **Local DNS**: Custom domain resolution without external dependencies

### 🚀 Performance
- **Gaming Hardware**: GTX 1080 Ti provides excellent transcoding power
- **ZFS Storage**: Advanced file system with compression and caching
- **Dedicated Resources**: Isolated containers with resource limits
- **Local Processing**: Sub-100ms latency for streaming workflows

### 🛠️ Management
- **Web Interfaces**: All services accessible via clean web UIs
- **Container Orchestration**: Docker Compose for easy deployment
- **Automated Deployments**: Git-based CI/CD for websites
- **Custom Domains**: Professional .veka.gg branding

---

## 📈 Performance Targets

### **Streaming Performance**
- **Encoding**: 1080p60 @ 6000kbps minimum
- **Latency**: <100ms for local NDI processing
- **Uptime**: 99%+ availability for live streaming
- **Quality**: Hardware-accelerated H.264/H.265

### **Web Hosting Performance**
- **Response Time**: <200ms for veka.gg
- **Uptime**: 99.9% availability target
- **SSL Rating**: A+ on SSL Labs
- **CDN**: Future Cloudflare integration

### **Current Utilization**
- **CPU Usage**: ~30% average during streaming
- **Memory Usage**: 20GB/32GB+ utilized
- **Storage I/O**: Fast local disk access
- **Network**: Gigabit LAN with room for 2.5G/10G upgrade

---

## 🌟 Why This Setup?

### **vs. Traditional Homelabs**
- ✅ **Content Creation Focus**: Built for streaming and web development
- ✅ **Gaming Hardware Reuse**: GTX 1080 Ti provides excellent transcoding
- ✅ **Professional Branding**: Custom veka.gg domain integration
- ✅ **Dual Purpose**: Gaming PC that doubles as enterprise homelab

### **vs. Cloud Platforms**
- ✅ **Cost Effective**: Repurposed existing gaming hardware
- ✅ **Low Latency**: Local processing for streaming workflows
- ✅ **Full Control**: Custom configurations for specific needs
- ✅ **Privacy**: All content and data stays local
- ✅ **Learning**: Hands-on experience with enterprise technologies

### **vs. Other Gaming PC Conversions**
- ✅ **Enterprise Practices**: Proper virtualization and storage
- ✅ **Professional Services**: Production-ready web hosting
- ✅ **Scalable Architecture**: Easy to expand as needs grow
- ✅ **Documentation Focus**: Comprehensive guides for replication

---

## 📖 Documentation

| Document | Description |
|----------|-------------|
| [🔧 Hardware Conversion](docs/hardware-conversion.md) | Gaming PC to homelab transformation |
| [🎥 Streaming Setup](docs/streaming-setup.md) | OBS headless and NDI configuration |
| [🌐 Network Configuration](docs/network-config.md) | Domain strategy and reverse proxy |
| [🐳 Docker Services](docs/docker-services.md) | Container deployment and management |
| [🔒 Security Guide](docs/security.md) | Hardening and access control |
| [📊 Monitoring Setup](docs/monitoring.md) | Performance tracking and alerts |
| [🔄 Backup Strategy](docs/backup-strategy.md) | Data protection and recovery |
| [🐛 Troubleshooting](docs/troubleshooting.md) | Common issues and solutions |

---

## 🛣️ Documentation Roadmap

### **High Priority Docs**
1. **Hardware Conversion Guide** - Gaming PC to enterprise homelab
2. **Streaming Infrastructure** - OBS headless + NDI setup
3. **Domain Configuration** - veka.gg subdomain strategy
4. **Service Deployment** - Docker container guides

### **Medium Priority Docs**
5. **Network Security** - Firewall and access control
6. **Performance Optimization** - Tuning for content creation
7. **Backup & Recovery** - Data protection strategies
8. **Monitoring & Alerting** - System health tracking

### **Future Documentation**
9. **Advanced Streaming** - Multi-platform automation
10. **AI Integration** - Ollama and local model hosting
11. **Home Automation** - Smart device integration
12. **Scaling Guide** - Growing the homelab ecosystem

---

## 🤝 Contributing

This is a personal homelab project, but I welcome:

### **Community Input**
- 💡 **Suggestions**: Better approaches or service recommendations
- 🐛 **Issue Reports**: Problems with documentation or guides
- 📝 **Documentation**: Improvements and corrections
- 🎥 **Content Ideas**: Streaming and homelab topics

### **Not Accepting**
- Direct code contributions (personal learning project)
- Service requests or custom configurations
- Hardware recommendations for different setups

---

## 🙏 Acknowledgments

### **Inspiration & Resources**
- **r/selfhosted** - Amazing community for self-hosting enthusiasts
- **r/homelabindia** - Local community with great insights
- **TrueNAS Community** - Excellent documentation and support
- **Proxmox Team** - Fantastic virtualization platform

### **Special Thanks**
- **Gaming PC Community** - For showing what's possible with repurposed hardware
- **Immich Team** - Creating the best Google Photos alternative
- **Pi-hole Team** - Network-wide ad blocking made simple
- **Content Creators** - Inspiring the dual-PC streaming approach

---

## 📞 Connect

### **Main Channels**
- 🌐 **Website**: [veka.gg](https://veka.gg) (coming soon)
- 📺 **Streaming**: Twitch/YouTube (future content)
- 💻 **GitHub**: [@VEKAgg](https://github.com/VEKAgg)
- 📧 **Contact**: Via GitHub issues

### **Project Updates**
- ⭐ **Star this repo** for updates and releases
- 👀 **Watch releases** for major milestones
- 📂 **Check docs/** for latest guides and tutorials

---

## 📜 License

MIT License - feel free to adapt concepts for your own homelab!

---

## 🏁 Final Notes

This homelab represents the journey of transforming gaming hardware into enterprise infrastructure while maintaining focus on content creation and learning. It's designed to be:

- **Practical**: Real-world tested solutions for content creators
- **Educational**: Step-by-step learning of enterprise technologies  
- **Scalable**: Architecture that grows with expanding needs
- **Personal**: Tailored for individual creativity and projects

Whether you're a gamer looking to repurpose hardware, a content creator seeking better infrastructure, or a homelab enthusiast wanting to try something different, I hope this documentation provides valuable insights!

---

<div align="center">

**Built with ❤️ for content creators and homelab enthusiasts**

*Turning gaming hardware into enterprise infrastructure, one container at a time*

[![Star History Chart](https://api.star-history.com/svg?repos=VEKAgg/homelab&type=Date)](https://star-history.com/#VEKAgg/homelab&Date)

**Last Updated**: September 24, 2025

</div>