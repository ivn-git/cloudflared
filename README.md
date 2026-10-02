# Cloudflare Tunnel (`cloudflared`) for Entware (ARM64 / aarch64)

Automated lightweight build of `cloudflared` binary and Entware init script for ARM64 (`aarch64`) systems.

---

## Quick Installation

Run the following command in your router's Entware terminal to install `wget-ssl` and the latest package in one step:

```bash
opkg update && opkg install wget-ssl https://github.com/ivn-git/cloudflared/releases/latest/download/cloudflared_aarch64-3.10.ipk
```

> **Note:** The package automatically installs the executable to `/opt/bin/cloudflared` and the service script to `/opt/etc/init.d/S99cloudflared`.

---

## Service Management (`S99cloudflared`)

Use the init script to control the daemon:

- **Start service:** `/opt/etc/init.d/S99cloudflared start`
- **Stop service:** `/opt/etc/init.d/S99cloudflared stop`
- **Restart service:** `/opt/etc/init.d/S99cloudflared restart`
- **Check status:** `/opt/etc/init.d/S99cloudflared status`
- **Update binary:** `/opt/etc/init.d/S99cloudflared update`

---

## Updating

This project uses its own update mechanism through the `S99cloudflared` init script.

To check for and install the latest optimized ARM64 build, run:

```bash
/opt/etc/init.d/S99cloudflared update
```

The update script:

1. Checks the latest release published by this project.
2. Compares the MD5 checksum with the currently installed binary.
3. Downloads the latest optimized `aarch64` binary if an update is available.
4. Replaces the installed binary.
5. Restarts the `cloudflared` service.

The update command requires an HTTPS-capable `wget`. If necessary, install it with:

```bash
opkg update && opkg install wget-ssl
```

> **Warning:** Do not use the built-in `cloudflared update` command.
>
> Do not enable automatic updates with `--autoupdate-freq`.
>
> The built-in updater downloads the original Cloudflare release binary. That binary is not built using this project's ARM64 build configuration, optimizations, or UPX compression. Using the built-in updater will replace the optimized binary with the original unoptimized version.

The supported update command for this project is:

```bash
/opt/etc/init.d/S99cloudflared update
```

---

## Configuration & Cloudflare Setup

Before starting the service, you must authenticate and create `/opt/etc/cloudflared/config.yml`.

For detailed official guides on login, tunnel creation, and routing:
👉 [Official Cloudflare Tunnel Documentation](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/)

### Quick Setup Steps:

1. **Authenticate with Cloudflare:**
   ```bash
   cloudflared tunnel login
   ```

2. **Create a Tunnel:**
   ```bash
   cloudflared tunnel create <TUNNEL_NAME>
   ```

3. **Configure the Tunnel:**
   Create and edit `/opt/etc/cloudflared/config.yml` with your Tunnel ID, credentials file path, and ingress rules.

4. **Start the Service:**
   ```bash
   /opt/etc/init.d/S99cloudflared start
   ```

## License and Disclaimer

* Original Software: This project is a derivative distribution of the official cloudflared client developed by Cloudflare. All original rights, trademarks, and upstream logic belong exclusively to Cloudflare.
* Project License: This repository and the automated compilation workflow are distributed under the Apache License 2.0, in full compliance with the upstream repository licensing terms.
* Warranty: This software is provided "as is", without warranty of any kind, express or implied. Use it at your own risk.

