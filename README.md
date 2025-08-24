# MediaServer Docker Compose

This repository contains Docker Compose configurations for a complete media server setup with VPN protection.

## 🔧 Environment Setup

### Files Structure
- `docker-compose.yml` - Main compose file (original with variables)
- `docker-compose-with-env.yml` - Updated compose file using environment variables
- `docker-compose-source-of-truth.yml` - Exact replica of currently running containers
- `.env` - Secret environment variables (not committed to git)
- `default.env` - Default non-secret environment variables (safe to commit)
- `.gitignore` - Prevents secrets from being committed

### 🔐 Secret Management

**For Portainer Users:**
Secrets are managed through Portainer's environment variables or secrets feature:
- `PIA_USERNAME` - Private Internet Access username
- `PIA_PASSWORD` - Private Internet Access password  
- `PLEX_CLAIM` - Plex claim token for server setup
- `SERVER_IP` - Server IP address for Plex advertising

**For Local Development:**
Copy values from `default.env` to `.env` and add your secrets:
```bash
cp default.env .env
# Edit .env and add your secret values
```

## 📊 Service Ports

| Service | Port | Description |
|---------|------|-------------|
| Gluetun HTTP Proxy | 9019 | VPN HTTP proxy |
| Gluetun Shadowsocks | 9020/9021 | VPN Shadowsocks proxy |
| Deluge Web UI | 9012 | BitTorrent client (via VPN) |
| Deluge Torrents | 9013 | BitTorrent port (via VPN) |
| Firefox | 9004 | Browser (via VPN) |
| SABnzbd | 9003 | Usenet downloader |
| Komga | 9005 | Comic/manga server |
| Sonarr | 9002 | TV show management |
| Radarr | 9001 | Movie management |
| Prowlarr | 9016 | Indexer management |
| xTeVe | 9009/9010 | IPTV proxy |
| LazyLibrarian | 9017 | Book/magazine management |

## 🚀 Services

### VPN Services
- **Gluetun**: VPN container providing secure connection for other services
- **Deluge**: BitTorrent client running through VPN

### Media Management
- **Plex**: Media server (uses host networking)
- **Sonarr**: TV show management and automation
- **Radarr**: Movie management and automation
- **Prowlarr**: Indexer management for *arr services

### Download Clients  
- **SABnzbd**: Usenet (NZB) downloader
- **Komga**: Comic and manga server
- **LazyLibrarian**: Book and magazine management and automation

### IPTV
- **xTeVe**: IPTV and EPG proxy

## 🔗 Network Configuration

Most services run on the `mediaplex_default` network except:
- **Plex**: Uses host networking for better performance and discovery
- **Deluge**: Shares network with Gluetun VPN container
- **Komga**: Uses default Docker bridge network

## 📁 Directory Structure

```
/volume1/docker/mediaserver/    # Configuration files
├── deluge/                     # Deluge config
├── sabnzbd/                    # SABnzbd config  
├── komga/                      # Komga config
├── plexms/                     # Plex config
├── sonarr/                     # Sonarr config
├── radarr/                     # Radarr config
├── prowlarr/                   # Prowlarr config
├── lazylibrarian/              # LazyLibrarian config
├── dnsForgeXteve/              # xTeVe config
├── guide2go/                   # Guide2go config
└── shared/                     # Shared config between services

/volume1/data/                  # Media and downloads
├── media/                      # Media files for Plex
├── torrents/                   # Torrent downloads
├── usenet/                     # Usenet downloads
├── comics/                     # Comic/manga files
├── books/                      # Book files for LazyLibrarian
├── magazines/                  # Magazine files for LazyLibrarian
└── downloads/                  # General download directory
```

## 🛠️ Usage

1. **Set up environment variables** in Portainer or create `.env` file
2. **Deploy the stack** using your preferred compose file
3. **Access services** via the web interfaces on their respective ports

## ⚠️ Important Notes

- Deluge traffic routes through Gluetun VPN for privacy
- All services use consistent PUID/PGID for file permissions
- Secrets should never be committed to version control
- The `docker-compose-source-of-truth.yml` represents exactly what's currently running

## 🔄 Updates

When updating the configuration:
1. Update the running containers first
2. Use `docker-compose-source-of-truth.yml` as the reference
3. Update other compose files to match
4. Test changes before deploying to production
