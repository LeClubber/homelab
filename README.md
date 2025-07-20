# 🏠 Homelab Infrastructure

A comprehensive self-hosted infrastructure setup using Docker Compose with Traefik reverse proxy and Authentik authentication.

## 📋 Table of Contents

- [🏗️ Architecture Overview](#️-architecture-overview)
- [🚀 Services](#-services)
  - [🔗 Core Infrastructure](#-core-infrastructure)
  - [🔐 Authentication & Security](#-authentication--security)
  - [📊 Monitoring & Management](#-monitoring--management)
  - [📁 Storage & Files](#-storage--files)
  - [🛠️ Development & Collaboration](#️-development--collaboration)
  - [🍽️ Lifestyle & Entertainment](#️-lifestyle--entertainment)
- [🌐 Access URLs](#-access-urls)
- [📚 Documentation](#-documentation)

## 🏗️ Architecture Overview

This homelab uses a modern containerized approach with:

- **Reverse Proxy**: Traefik v3.3.5 for routing and SSL termination
- **Authentication**: Authentik for Single Sign-On (SSO) across services
- **Networking**: Docker networks with `traefik-proxy` as the main external network
- **SSL/TLS**: Custom certificates stored in `/traefik/certs/`
- **Domain**: All services accessible via `*.my.home.com`

## 🚀 Services

### 🔗 Core Infrastructure

#### Traefik (Reverse Proxy)
- **Image**: `traefik:v3.3.5`
- **Purpose**: HTTP/HTTPS reverse proxy and load balancer
- **Access**: `traefik.my.home.com`
- **Features**:
  - Automatic SSL/TLS termination
  - Docker service discovery
  - HTTP to HTTPS redirect
  - Authentik middleware integration

#### Whoami (Test Service)
- **Image**: `traefik/whoami`
- **Purpose**: Test service for proxy functionality
- **Access**: `whoami.my.home.com`

### 🔐 Authentication & Security

#### Authentik
- **Image**: `ghcr.io/goauthentik/server:2024.12.3`
- **Purpose**: Identity provider and SSO solution
- **Access**: `authentik.my.home.com`
- **Components**:
  - PostgreSQL database
  - Redis cache
  - Forward authentication for Traefik

#### Vault
- **Image**: `hashicorp/vault:1.18`
- **Purpose**: Secrets management
- **Access**: `vault.my.home.com`
- **Features**:
  - File-based storage
  - Web UI enabled
  - IPC_LOCK capability for secure memory

### 📊 Monitoring & Management

#### Portainer
- **Image**: `portainer/portainer-ce:2.26.1`
- **Purpose**: Docker container management
- **Access**: `portainer.my.home.com`
- **Features**: Docker GUI, container monitoring, stack management

#### Grafana
- **Image**: `grafana/grafana:11.5.1`
- **Purpose**: Monitoring and observability dashboards
- **Access**: `grafana.my.home.com`
- **Plugins**: grafana-clock-panel pre-installed

#### Dockge
- **Image**: `louislam/dockge:1.4.2`
- **Purpose**: Docker Compose stack management
- **Access**: `dockge.my.home.com`
- **Features**: Stack directory mounted at `/homelab`

#### Dash (DashDot)
- **Image**: `mauricenino/dashdot:latest`
- **Purpose**: System monitoring dashboard
- **Access**: `dash.my.home.com`
- **Features**: Real-time system metrics, privileged access to host

#### Homarr
- **Image**: `ghcr.io/homarr-labs/homarr:latest`
- **Purpose**: Homelab dashboard and service launcher
- **Access**: `homarr.my.home.com`
- **Features**: Docker integration, customizable dashboard

### 📁 Storage & Files

#### Nextcloud
- **Image**: `nextcloud`
- **Purpose**: Self-hosted cloud storage and collaboration
- **Access**: `nextcloud.my.home.com`
- **Components**:
  - PostgreSQL database
  - File storage volume
  - Admin account configured

#### FileBrowser
- **Image**: `filebrowser/filebrowser`
- **Purpose**: Web-based file manager
- **Access**: `filebrowser.my.home.com`
- **Features**: File upload/download, directory browsing

### 🛠️ Development & Collaboration

#### GitLab
- **Image**: `gitlab/gitlab-ce:17.7.3-ce.0`
- **Purpose**: Git repository management and CI/CD
- **Access**: `gitlab.my.home.com`
- **Features**:
  - OpenID Connect integration with Authentik
  - Auto sign-in configured
  - SSL client certificate verification
  - GitLab Runner support (config in `runner-config/`)

#### DokuWiki
- **Image**: `dokuwiki/dokuwiki:2024-02-06b`
- **Purpose**: Wiki and documentation platform
- **Access**: `wiki.my.home.com`
- **Configuration**:
  - PHP timezone: Europe/Paris
  - Memory limit: 256M
  - Upload limit: 128M

#### Draw.io
- **Image**: `jgraph/drawio:26.0.9`
- **Purpose**: Diagramming and flowchart tool
- **Access**: `drawio.my.home.com`

### 🍽️ Lifestyle & Entertainment

#### Mealie
- **Image**: `ghcr.io/mealie-recipes/mealie:v2.6.0`
- **Purpose**: Recipe management and meal planning
- **Access**: `mealie.my.home.com`
- **Features**:
  - PostgreSQL database
  - OIDC authentication with Authentik
  - Memory limit: 1GB
  - Signup disabled (admin-managed)

#### Plex Media Server
- **Image**: `lscr.io/linuxserver/plex:latest`
- **Purpose**: Media streaming server
- **Access**: Host network mode (direct access)
- **Configuration**:
  - Media directory: `/mnt/nas/videos`
  - Timezone: Europe/Paris
  - PUID/PGID: 1000

## 🌐 Access URLs

All services are accessible via HTTPS with custom domain `my.home.com`:

| Service | URL | Purpose |
|---------|-----|---------|
| Traefik | `https://traefik.my.home.com` | Reverse proxy dashboard |
| Authentik | `https://authentik.my.home.com` | Authentication portal |
| Portainer | `https://portainer.my.home.com` | Container management |
| Nextcloud | `https://nextcloud.my.home.com` | Cloud storage |
| GitLab | `https://gitlab.my.home.com` | Git repositories |
| Grafana | `https://grafana.my.home.com` | Monitoring dashboards |
| Vault | `https://vault.my.home.com` | Secrets management |
| Homarr | `https://homarr.my.home.com` | Homelab dashboard |
| Mealie | `https://mealie.my.home.com` | Recipe management |
| DokuWiki | `https://wiki.my.home.com` | Documentation wiki |
| Draw.io | `https://drawio.my.home.com` | Diagramming tool |
| FileBrowser | `https://filebrowser.my.home.com` | File management |
| Dockge | `https://dockge.my.home.com` | Stack management |
| Dash | `https://dash.my.home.com` | System monitoring |
| Whoami | `https://whoami.my.home.com` | Test service |

## 📚 Documentation

### Quick Start

1. **Prerequisites**:
   - Docker and Docker Compose installed
   - Custom SSL certificates in `traefik/certs/`
   - Environment files configured for each service

2. **Deployment**:
   ```bash
   # Deploy core infrastructure first
   cd traefik && docker compose up -d
   cd ../authentik && docker compose up -d
   
   # Deploy other services
   cd ../portainer && docker compose up -d
   cd ../nextcloud && docker compose up -d
   # ... repeat for other services
   ```

3. **Network Setup**:
   - Create external network: `docker network create traefik-proxy`
   - Ensure DNS resolution for `*.my.home.com` points to your server

### Key Features

- **Single Sign-On**: Most services integrate with Authentik for unified authentication
- **SSL Everywhere**: All services use HTTPS with custom certificates
- **Service Discovery**: Traefik automatically discovers and routes to Docker services
- **Scalable Architecture**: Each service is containerized and independently manageable
- **Monitoring Ready**: Grafana and system monitoring dashboards available

### Security Notes

- All services are protected behind Authentik authentication middleware
- Custom SSL certificates provide secure communication
- Vault provides centralized secrets management
- Regular updates managed through version pinning in compose files

---

*Last updated: July 2025*

