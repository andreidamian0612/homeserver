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

## Hardware

- **CPU**: Intel i5-6500T @ 2.50GHz
- **GPU**: Intel HD Graphics 530 (QSV transcoding)
- **Storage**: NVMe 477GB (OS + media, single drive)
- **Zigbee**: Nabu Casa SkyConnect v1.0 (USB)

> **Note**: Original SATA SSD (512GB) died 2026-04-01 with "access beyond end of device" errors. All media now on NVMe. `/media/storage/` is a regular directory, not a separate mount.

## Directory Structure

```
/media/storage/          <- on NVMe (single drive setup)
  movies/                <- Radarr imports here, Jellyfin reads
  tv/                    <- Sonarr imports here, Jellyfin reads
  music/                 <- Lidarr imports here, Jellyfin reads
  downloads/             <- qBittorrent downloads here

/opt/mediaserver/
  config/                <- All service configs
  docker-compose.yml
  setup.log
  apikeys.txt
  CLAUDE.md
```

## Setup

1. Copy `.env.example` to `.env` and adjust paths
2. Create media directories:
   ```bash
   mkdir -p /media/storage/{movies,tv,music,downloads}
   ```
3. Start services:
   ```bash
   docker compose up -d
   ```
4. Configure each service via its web UI
