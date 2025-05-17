# Cloudflare Tunnel Podman Container

This README contains instructions and configuration for running a Cloudflare Tunnel Podman container with systemd integration for automatic startup.

## Overview

Cloudflare Tunnel provides a secure way to connect your resources to Cloudflare without exposing public IPs or opening ports on your firewall. This setup uses Podman (instead of Docker) to run the Cloudflare Tunnel client (`cloudflared`) in a container.

## Prerequisites

- A Linux system with Podman installed
- `sudo` or root access
- A Cloudflare account with a registered domain
- A valid Cloudflare Tunnel token

## Installation and Setup

### 1. Create and Run the Cloudflare Tunnel Container

```bash
podman run -d --name cloudflared-tunnel --restart unless-stopped \
cloudflare/cloudflared:latest tunnel run --token <TOKEN-HERE>
```

Replace `<TOKEN-HERE>` with your actual Cloudflare Tunnel token.

### 2. Test the Container

Start the container manually to verify it works:

```bash
podman stop cloudflared-tunnel
podman start cloudflared-tunnel
```

### 3. Create a Systemd Service

Create a systemd service file at `/etc/systemd/system/cloudflare.service`:

```ini
[Unit]
Description=Start cloudflare container 
After=network-online.target

[Service]
Type=oneshot
User=root
ExecStart=/bin/bash -c 'sudo /bin/podman start cloudflared-tunnel'
TimeoutStartSec=60

[Install]
WantedBy=multi-user.target
```

### 4. Enable and Start the Service

```bash
systemctl daemon-reload
systemctl enable cloudflare
systemctl start cloudflare.service
```

### 5. Verify Service Status

```bash
systemctl status cloudflare.service
```

## Troubleshooting

If the service doesn't start properly:

1. Check the container status with `podman ps -a`
2. View container logs with `podman logs cloudflared-tunnel`
3. Check systemd logs with `journalctl -u cloudflare.service`

## Additional Resources

- [Cloudflare Tunnel Documentation](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/)
- [Podman Documentation](https://docs.podman.io/)
- [Systemd Documentation](https://www.freedesktop.org/wiki/Software/systemd/)
