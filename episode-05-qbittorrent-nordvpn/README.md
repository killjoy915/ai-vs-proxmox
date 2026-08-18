# AI vs Proxmox: Can AI Build qBittorrent with NordVPN?

This repository contains the companion guide for Episode 5 of the **AI vs Proxmox** YouTube series.

In this episode, AI is used to build a qBittorrent server on Proxmox with torrent traffic routed through NordVPN.

## What This Build Uses

- Proxmox VE
- LXC container
- Docker
- Docker Compose
- qBittorrent
- Gluetun
- NordVPN

## Guide Contents

This guide will include:

- Proxmox container setup
- Docker installation
- Directory creation
- qBittorrent configuration
- NordVPN / Gluetun configuration
- Docker Compose configuration
- Starting the services
- VPN IP verification
- qBittorrent Web UI access
- Troubleshooting
- Security notes

## Security Notice

Never copy private credentials directly from another person's configuration.

Replace all example values with your own, including:

- VPN usernames
- VPN passwords
- API keys
- IP addresses
- passwords
- private keys
- tokens
- hostnames
- storage paths where appropriate

Any configuration published in this repository should use sanitized example values.

## YouTube

Video:

**AI vs Proxmox: Can AI Build qBittorrent with NordVPN?**

YouTube link will be added after publication.

## Prerequisites

Before starting, you will need:

* A working Proxmox VE server
* A Linux LXC container or VM
* Docker installed
* Docker Compose installed
* A NordVPN account with manual OpenVPN service credentials
* A storage location for downloads

This guide assumes the qBittorrent application is being deployed inside a Linux container or VM running on Proxmox.

---

## Create the Project Directory

Create the project directory:

```bash
mkdir -p /opt/qbittorrent-vpn
cd /opt/qbittorrent-vpn
```

Create the qBittorrent configuration directory:

```bash
mkdir -p config
```

---

## Create the Download Directory

This example uses:

```text
/mnt/downloads
```

Create it if it does not already exist:

```bash
mkdir -p /mnt/downloads
```

You may replace this path with your own local disk, NFS mount, Samba mount, or other storage location.

The important part is that the path used here must match the volume mapping in `docker-compose.yml`.

Example:

```yaml
volumes:
  - /mnt/downloads:/downloads
```

---

## Create the Environment File

Copy the example environment file:

```bash
cp .env.example .env
```

Edit the real environment file:

```bash
nano .env
```

Replace:

```env
OPENVPN_USER=YOUR_NORDVPN_SERVICE_USERNAME
OPENVPN_PASSWORD=YOUR_NORDVPN_SERVICE_PASSWORD
```

with your own NordVPN manual OpenVPN service credentials.

Do not commit your real `.env` file to GitHub.

The included `.gitignore` is configured to exclude it.

---

## Start qBittorrent and Gluetun

From inside:

```text
/opt/qbittorrent-vpn
```

start the stack:

```bash
docker compose up -d
```

Check the containers:

```bash
docker compose ps
```

Both containers should show as running:

```text
gluetun
qbittorrent
```

---

## Check the VPN Connection

Review the Gluetun logs:

```bash
docker logs gluetun --tail=50
```

You should see that Gluetun successfully connected to a NordVPN OpenVPN server.

Verify the public IP from inside Gluetun:

```bash
docker exec gluetun wget -qO- https://ipinfo.io/ip
```

The IP returned here should be the VPN exit IP, not your normal home internet public IP.

You can compare it against the public IP shown from another device on your normal network.

---

## Verify qBittorrent Uses the VPN

Because qBittorrent uses:

```yaml
network_mode: "service:gluetun"
```

it shares Gluetun's network stack.

This means qBittorrent does not have its own direct network connection.

Its torrent traffic is routed through the Gluetun VPN container.

If Gluetun is stopped, qBittorrent loses its network path.

This provides VPN kill-switch style protection.

---

## Open the qBittorrent Web Interface

Open a browser and go to:

```text
http://YOUR_CONTAINER_IP:8080
```

Example:

```text
http://192.168.1.100:8080
```

Use your own Proxmox container or VM IP address.

---

## Find the Temporary qBittorrent Password

On first startup, qBittorrent may generate a temporary administrator password.

Check the logs:

```bash
docker logs qbittorrent
```

Look for a message similar to:

```text
The WebUI administrator username is: admin
The WebUI administrator password was not set.
A temporary password is provided for this session.
```

Use:

```text
Username: admin
```

and the temporary password shown in the log.

After logging in, immediately change the password from the qBittorrent Web UI.

---

## Recommended Download Folders

Inside qBittorrent, configure your paths under:

```text
Tools
→ Options
→ Downloads
```

Example configuration:

```text
Default Save Path:
/downloads/completed

Keep incomplete torrents in:
/downloads/incomplete

Automatically add torrents from:
/downloads/watch
```

Create those folders on the host:

```bash
mkdir -p /mnt/downloads/completed
mkdir -p /mnt/downloads/incomplete
mkdir -p /mnt/downloads/watch
```

Because `/mnt/downloads` is mapped to `/downloads`, qBittorrent will see these as:

```text
/downloads/completed
/downloads/incomplete
/downloads/watch
```

---

## Basic Verification

Check running containers:

```bash
docker compose ps
```

Check Gluetun's external IP:

```bash
docker exec gluetun wget -qO- https://ipinfo.io/ip
```

Check recent VPN logs:

```bash
docker logs gluetun --tail=40
```

Check recent qBittorrent logs:

```bash
docker logs qbittorrent --tail=40
```

If both containers are running, the VPN reports a different public IP, and the qBittorrent Web UI loads correctly, the basic deployment is working.

---

## Security Notes

Never publish:

* NordVPN service credentials
* `.env` files containing passwords
* API keys
* authentication tokens
* private keys
* cookies or session tokens
* SSH private keys

Before posting screenshots, videos, configuration files, or terminal output publicly, review them for credentials and other access-related information.

The `.env.example` file in this repository contains placeholders only.

---

## Stopping the Stack

Stop the containers:

```bash
docker compose down
```

Start them again:

```bash
docker compose up -d
```

Restart them:

```bash
docker compose restart
```

---

## Updating the Containers

Pull updated images:

```bash
docker compose pull
```

Recreate the stack using the new images:

```bash
docker compose up -d
```

Remove unused Docker image layers afterward if desired:

```bash
docker image prune
```
