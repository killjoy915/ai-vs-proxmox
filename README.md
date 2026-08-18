# AI vs Proxmox

Companion guides, commands, configurations, and documentation for the **AI vs Proxmox** YouTube series.

This repository contains the public build documentation for the projects featured on the channel.

The goal is to make each project reproducible while keeping private credentials, IP addresses, passwords, tokens, and other sensitive information out of the public repository.

---

## Episodes

### Episode 5 — qBittorrent + NordVPN

**AI vs Proxmox: Can AI Build qBittorrent with NordVPN?**

Build a qBittorrent download server on Proxmox with torrent traffic routed through NordVPN using Gluetun.

Guide:

[Episode 5 — qBittorrent + NordVPN](./episode-05-qbittorrent-nordvpn/)

Includes:

* Proxmox deployment guidance
* Docker and Docker Compose
* qBittorrent
* Gluetun
* NordVPN OpenVPN configuration
* VPN verification
* Download directory setup
* Troubleshooting
* Security notes

---

## Upcoming Documentation

Additional AI vs Proxmox episode guides will be added over time, including:

* Samba File Server
* Plex GPU Transcoding
* Scrypted
* Additional Proxmox and homelab projects

---

## Repository Structure

```text
ai-vs-proxmox/
│
├── README.md
├── LICENSE
│
└── episode-05-qbittorrent-nordvpn/
    ├── README.md
    ├── docker-compose.yml
    ├── .env.example
    └── .gitignore
```

Each episode will eventually have its own folder containing the relevant commands, configuration examples, troubleshooting steps, and security notes.

---

## Security

Configuration examples in this repository are intended to be sanitized before publication.

Never publish:

* Passwords
* VPN credentials
* API keys
* Access tokens
* Private keys
* Authentication cookies
* Recovery codes
* SSH private keys
* Other secrets that could provide access to a system or account

Example configuration files should use placeholder values instead of real credentials.

---

## About the Series

**AI vs Proxmox** explores whether AI can plan, build, troubleshoot, and validate real homelab projects running on Proxmox.

Each project is documented here so viewers can follow the build without having to copy commands from a YouTube video or dig through long comment threads.

