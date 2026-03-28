# Home Media Server

Media server stack running on Debian 13 with Docker.

## Services

| Service | Port | Purpose |
|---------|------|---------|
| Jellyfin | 8096 | Media player (with Intel QSV hardware transcoding) |
| Jellyseerr | 5055 | Media request manager (Seerr v3.1.0) |
| Radarr | 7878 | Movie management |
| Sonarr | 8989 | TV show management |
| Lidarr | 8686 | Music management |
| Prowlarr | 9696 | Indexer manager |
| qBittorrent | 8080 | Torrent client |
| Bazarr | 6767 | Subtitle management (Romanian + English) |
| Portainer | 9443 | Docker management UI |

## Additional System Services

- **Cockpit** (port 9090) - Server management UI
- **Cloudflared** - Cloudflare tunnel
- **Tailscale** - VPN/mesh networking

## Hardware

- **CPU**: Intel i5-6500T @ 2.50GHz
- **GPU**: Intel HD Graphics 530 (QSV transcoding)
- **Storage**: NVMe 477GB (OS) + SATA SSD 477GB (media at /media/storage)

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
- Bazarr -> Radarr, Sonarr (auto subtitle download)
- Jellyseerr -> Jellyfin, Radarr, Sonarr (media requests)

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
  docker-compose.yml
```
