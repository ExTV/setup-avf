# setup-avf

AVF VM Setup & Optimization Script for Pixel devices running Android Virt Framework (AVF) with gfxstream / virtio-gpu acceleration.

## What it does

- **DNS** — Sets Google (8.8.8.8) + Cloudflare (1.1.1.1), locks `/etc/resolv.conf`
- **Linger** — Enables systemd user linger so services persist after logout
- **System update** — Full `apt` upgrade, autoremove, clean
- **Legacy cleanup** — Removes Weston and old display shims
- **SSH** — Optional OpenSSH on port 9922 with X11 forwarding
- **GPU / gfxstream** — Writes optimal `VK_ICD_FILENAMES`, `MESA_LOADER_DRIVER_OVERRIDE=zink`, and Wayland env vars to `/etc/environment`; installs missing Mesa/Vulkan packages
- **Optional extras** — Podman + Distrobox + Fastfetch, XFCE4, Sway, dev tools

## Requirements

- Pixel device running an AVF VM (Debian/Ubuntu guest)
- `sudo` access inside the VM
- Internet connectivity

## Usage

### One-liner (curl)

```bash
bash <(curl -fsSL https://raw.githubusercontent.com/ExTV/setup-avf/main/setup-avf)
```

> **ttyd users:** use `bash <(curl ...)` — piping via `curl ... | bash` steals stdin and breaks interactive prompts.

### Or with wget

```bash
bash <(wget -qO- https://raw.githubusercontent.com/ExTV/setup-avf/main/setup-avf)
```

### Manual

```bash
curl -fsSL https://raw.githubusercontent.com/ExTV/setup-avf/main/setup-avf -o setup-avf
chmod +x setup-avf
./setup-avf
```

## After running

1. Reboot: `sudo reboot`
2. Verify GPU: `vulkaninfo 2>/dev/null | grep 'GPU id'`
3. Start XFCE (if installed): `startxfce4`
4. Start Sway (if installed): `sway`
5. SSH in: `ssh -p 9922 <user>@<pixel-ip>`

## Log

Each run saves a timestamped log to `/var/log/avf_setup_<timestamp>.log`.
