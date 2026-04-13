# Home Server - /opt/mediaserver

## Quick Reference
- Docker compose: /opt/mediaserver/docker-compose.yml
- Config: /opt/mediaserver/config/<service>/
- Media: /media/storage/ (movies, tv, music, downloads) - ALL ON NVMe
- Setup log: /opt/mediaserver/setup.log
- API keys: /opt/mediaserver/apikeys.txt
- GitHub: https://github.com/andreidamian0612/homeserver

## Storage
- Single NVMe drive (477GB) for everything (OS + media)
- Old SATA SSD died 2026-04-01 ("access beyond end of device")
- /media/storage/ is a regular directory on NVMe, NOT a separate mount

## Services (11 containers)
- jellyfin:8096, jellyseerr:5055, radarr:7878, sonarr:8989
- lidarr:8686, prowlarr:9696, qbittorrent:8080, bazarr:6767
- mixarr:3010/3443, homeassistant:8123, portainer:9443

## System services (systemd)
- docker, cloudflared, tailscaled

## Credentials
- Server: root/damadama1, dama/damadama1 (sudo NOPASSWD)
- qBittorrent: dama/damadama1
- Portainer: admin/Damadama1@2026
- Jellyfin: dama/Damadama1!

## Hardware
- Intel i5-6500T, HD Graphics 530 (QSV), SkyConnect ZBT-1 Zigbee
- NVMe 477GB (OS + media)

## Volume mounts (hard links)
- Radarr, Sonarr, Lidarr, qBittorrent, Bazarr: /media/storage -> /data
  - Container paths: /data/downloads, /data/movies, /data/tv, /data/music
- Jellyfin: separate mounts (/data/movies, /data/tv, /data/music)
- Hard links enabled: downloads and library share same inode, no double storage

## Key decisions
- x264 preferred over x265 (HD 530 lacks 10-bit HEVC hw decode)
- FLAC preferred for music
- Bazarr: Romanian + English subtitles
- Cloudflare: media.dmnsolutions.com (no Access), requests.dmnsolutions.com (Access + OTP)
