# HVM PANEL

**HVM PANEL** is a full-featured web-based LXC/Incus VPS management panel.  
Create, manage, and monitor containers with a modern UI, real-time stats, web console, port forwarding, snapshots, Discord bot, and a complete REST API.

**Version:** 8.0-PRO

---

## Features

### Core
- LXC / Incus based VPS (not Docker)
- Multi-node support (local + remote node agents)
- Web-based terminal (LXC exec primary, SSH fallback)
- Live resource stats (CPU, RAM, disk, network)
- Port forwarding with auto SSH forward for console
- Snapshots & restore
- Bandwidth quota monitoring
- Auto-suspend / expiration
- User sharing & admin controls

### Security & Access
- Flask-Login + optional 2FA
- Per-VPS root password management
- Role-based access (user / admin / main admin)
- API key authentication for REST API & node agents

### Extra
- Discord bot (full panel control via slash commands)
- Comprehensive REST API + interactive docs
- Email notifications (SMTP)
- Maintenance mode
- License client support

---

## Requirements

### Panel Host
- Linux (recommended) or Windows
- Python 3.10+
- LXC / LXD / Incus installed and working (`lxc` command available)
- Storage pool (default: `default`)

### Remote Nodes
- Same LXC/Incus requirement
- Node agent (`node.py`) running
- API key configured

---

## Quick Start

### 1. Clone & Install

```bash
git clone https://github.com/YOUR_USERNAME/hvm-panel.git
cd hvm-panel

python3 -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 2. Run the Panel

```bash
python hvm.py
```

Default: `http://0.0.0.0:5000`

On first run the panel creates its SQLite database and default settings.

### 3. Login

- Open the panel in your browser
- Register the first account (becomes main admin) or use the default setup flow

---

## Configuration

Main settings can be changed from **Admin → Settings** or via environment variables:

| Variable | Default | Description |
|----------|---------|-------------|
| `PANEL_NAME` | `HVM PANEL` | Brand name |
| `PANEL_VERSION` | `8.0-PRO` | Version string |
| `DEFAULT_STORAGE_POOL` | `default` | LXC storage pool |
| `HOST` / `PORT` | `0.0.0.0` / `5000` | Bind address |

Optional: create a `.env` file or export variables before starting.

---

## Node Agent (Remote Nodes)

On every remote node:

```bash
# Copy node.py + requirements and install deps
pip install -r requirements.txt

# Run the agent
python node.py
```

Then add the node in **Admin → Nodes** with its URL and API key.

The node agent provides:
- Command execution (`lxc ...`)
- Container shell / PTY relay for web console
- Health & stats

---

## Web Console

The VPS console prefers **direct `lxc exec`** (no SSH, no port-forward needed).

- **Local node** → panel spawns PTY + `lxc exec`
- **Remote node** → node-agent `/api/container/shell/open` relay

SSH via port-forward is kept as automatic fallback.

---

## Discord Bot

Located in `bot/`.

```bash
cd bot
pip install -r requirements.txt
# Edit config.py → set Discord token + panel API key
python bot.py
```

Supports VPS control, user management, nodes, and system commands via slash commands.

---

## REST API

Base path: `/api/v1`

- Interactive docs available in the panel (Admin → API)
- Authenticate with API key header
- Full coverage: users, VPS, nodes, ports, snapshots, stats, etc.

---

## Project Structure

```
hvm v8/
├── hvm.py                 # Main panel application
├── node.py                # Node agent
├── api.py                 # REST API blueprint / docs
├── live_stats_manager.py  # Real-time stats
├── license_client.py      # License handling
├── requirements.txt
├── ecosystem.config.js    # PM2 config (optional)
├── bot/                   # Discord bot
│   ├── bot.py
│   ├── config.py
│   └── cogs/
├── static/                # Assets, uploads, JS
└── templates/             # Jinja2 templates
```

---

## Production Tips

- Use a process manager (PM2, systemd, or Hypercorn)
- Put the panel behind Nginx / Caddy with HTTPS
- Keep node-agents updated when upgrading the panel (especially console endpoints)
- Regularly back up the SQLite database
- Restrict node-agent ports with a firewall (API key still required)

Example with PM2:

```bash
pm2 start ecosystem.config.js
```

---

## Tech Stack

- **Backend:** Flask, Flask-SocketIO, Flask-Login
- **Containers:** LXC / LXD / Incus
- **Realtime:** Socket.IO + background stats threads
- **SSH fallback:** Paramiko
- **Frontend:** Jinja2 + xterm.js + modern CSS
- **Bot:** discord.py

---

## License

Proprietary / All rights reserved.  
Contact the author for licensing.

---

## Credits

Built as a complete LXC VPS control panel with web console, multi-node support, and automation tools.
