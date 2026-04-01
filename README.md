# Home Server

Home server stack running on Debian 13 with Docker.

## Services

### Media Stack
| Service | Port | Purpose |
|---------|------|---------|
| Jellyfin | 8096 | Media player (Intel QSV hardware transcoding) |
| Jellyseerr | 5055 | Media request manager (Seerr v3.1.0) |
| Radarr | 7878 | Movie management |
| Sonarr | 8989 | TV show management |
| Lidarr | 8686 | Music management |
| Prowlarr | 9696 | Indexer manager |
| qBittorrent | 8080 | Torrent client |
| Bazarr | 6767 | Subtitle management (Romanian + English) |
| Mixarr | 3010/3443 | Music discovery for Lidarr |

### Smart Home
| Service | Port | Purpose |
|---------|------|---------|
| Home Assistant | 8123 | Home automation (Zigbee via SkyConnect ZBT-1) |

### Management
| Service | Port | Purpose |
|---------|------|---------|
| Portainer | 9443 | Docker management UI |

## System Services (systemd)

- **Docker** - container runtime
- **Cloudflared** - Cloudflare tunnel (dmnsolutions.com)
- **Tailscale** - VPN/mesh networking

## Hardware

- **CPU**: Intel i5-6500T @ 2.50GHz
- **GPU**: Intel HD Graphics 530 (QSV transcoding)
- **Storage**: NVMe 477GB (OS) + SATA SSD 477GB (media at /media/storage)
- **Zigbee**: Nabu Casa SkyConnect v1.0 (USB)

## Quality Profiles

### Radarr/Sonarr - "HD Streaming"
- Preferred: x264 1080p (+100 score)
- Accepted: x265 (-50 score)
- Blocked: HDR10, Dolby Vision, Remux (-10000 score)
- Optimized for Intel HD 530 which lacks 10-bit HEVC hardware decoding

### Lidarr - "Music Quality"
- Preferred: FLAC, ALAC (lossless)
- Fallback: MP3-320, MP3-VBR-V0

## Integrations

- Prowlarr -> Radarr, Sonarr, Lidarr (indexer sync)
- qBittorrent -> Radarr, Sonarr, Lidarr (download client)
- Radarr/Sonarr/Lidarr -> Jellyfin (auto library scan on import)
- Bazarr -> Radarr, Sonarr (auto subtitle download: Romanian + English)
- Jellyseerr -> Jellyfin, Radarr, Sonarr (media requests)
- Mixarr -> Lidarr (music discovery)
- Home Assistant -> SkyConnect ZBT-1 (Zigbee coordinator via ZHA)

## Cloudflare Tunnel

- `media.dmnsolutions.com` -> Jellyfin (no Access protection, Jellyfin handles auth)
- `requests.dmnsolutions.com` -> Jellyseerr (Cloudflare Access, email OTP)

## Jellyfin Transcoding

- Hardware acceleration: Intel QSV via VAAPI
- GPU passthrough: /dev/dri
- Hardware encoding: H264, HEVC (8-bit only)
- Low-power encoders: enabled
- 10-bit HEVC/HDR: software fallback (HD 530 limitation)

## Setup

1. Copy `.env.example` to `.env` and adjust paths
2. Create media directories:
   ```bash
   mkdir -p /media/storage/{movies,tv,music,downloads,books}
   ```
3. Create config directory:
   ```bash
   mkdir -p /opt/mediaserver/config
   ```
4. Start services:
   ```bash
   docker compose up -d
   ```
5. Configure each service via its web UI

## Directory Structure

```
/media/storage/
  movies/
  tv/
  music/
  downloads/
  books/

/opt/mediaserver/
  config/
    jellyfin/
    radarr/
    sonarr/
    lidarr/
    prowlarr/
    qbittorrent/
    jellyseerr/
    bazarr/
    mixarr/
    homeassistant/
  docker-compose.yml
```
