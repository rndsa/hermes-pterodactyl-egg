# Hermes Agent — Pterodactyl Panel Egg

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform: Pterodactyl](https://img.shields.io/badge/Platform-Pterodactyl%20Panel-007ACC.svg)](https://pterodactyl.io/)
[![Standard: PTDL v2](https://img.shields.io/badge/Standard-PTDL__v2-blue.svg)](https://pterodactyl.io/)
[![Runtime: Python 3.11](https://img.shields.io/badge/Runtime-Python%203.11%20%7C%20Playwright-3776AB.svg)](https://python.org)
[![Author: ren](https://img.shields.io/badge/Author-ren%20(%40rskl411_)-ff70a6.svg)](https://instagram.com/rskl411_)

Custom Pterodactyl Panel Egg configuration and container toolchain engineered for deploying autonomous AI agents, Telegram gateways, and headless browser automation runners ([Hermes Agent by Nous Research](https://github.com/NousResearch/Hermes-Agent)) inside isolated server containers.

---

## 🎯 Architecture & Capabilities

Deploying autonomous agent gateways inside game server control panels like Pterodactyl requires specific environment setups that standard eggs do not provide:

* **Dual Execution Modes**: Supports running directly via standard Pterodactyl startup arguments (`python3 -m hermes_cli.main --profile <profile> gateway run`) or delegating execution to a custom bash script (`/home/container/start.sh`).
* **Multi-Image Compatibility**: Works out of the box with official Python yolks (`ghcr.io/ptero-eggs/yolks:python_3.11`), Playwright headless browser images, and custom super-root Docker environments with passwordless sudo.
* **Persistent Profiles & Memory**: Seamlessly maps `HERMES_HOME=/home/container/.hermes` ensuring sessions, skills, cronjobs, and SQLite databases persist across server restarts and node migrations.
* **Network & Gateway Resiliency**: Designed for low-latency HTTP/WebSocket relays, reverse proxy routing, and direct Telegram Bot API connectivity.

---

## 📦 Egg Configuration Details

| Field | Configuration Value |
|---|---|
| **Egg Format** | PTDL_v2 JSON Specification |
| **Default Image** | `ghcr.io/ptero-eggs/yolks:python_3.11` |
| **Alternative Images** | `ptero-playwright:python_3.11`, `ptero-hermes-root:latest` |
| **Process Stop Signal** | `^C` (SIGINT graceful shutdown) |
| **Startup Indicator** | `Gateway is running` |
| **Config File** | `egg-hermes-agent.json` |

---

## 🚀 Deployment Guide

### Step 1: Import Egg to Pterodactyl Panel
1. Login to your Pterodactyl Admin Control Panel (e.g. https://your-panel-domain.com/admin).
2. Navigate to **Nests** &rarr; select or create a Nest (e.g. `AI Agents` or `Generic`).
3. Click **Import Egg** in the top right corner.
4. Upload `egg-hermes-agent.json` from this repository.
5. Set the associated Docker images and save the egg.

### Step 2: Create Server Instance
1. Go to **Servers** &rarr; **Create New**.
2. Assign server name, owner account, and node allocation (Memory: minimum 1024 MB, CPU: 100%+).
3. Under **Nest Configuration**, choose the imported `Hermes Agent` egg.
4. Select your preferred Docker image:
   * `ghcr.io/ptero-eggs/yolks:python_3.11` — Lightweight standard Python runtime.
   * `ptero-hermes-root:latest` — Full toolchain with passwordless sudo, Playwright Chromium, Node.js, and debugging utilities.
5. Fill in the environment variables:
   * `HERMES_PROFILE`: Profile name to load (default: `default`).
   * `STARTUP_MODE`: Startup command mode (default: `gateway run`).
   * `TELEGRAM_BOT_TOKEN`: (Optional) Bot token from @BotFather if not placed directly in `.env`.
6. Click **Create Server**.

### Step 3: Initialize Agent Files
Once the server container is provisioned:
1. Open the server console and wait for initial initialization.
2. Place your `config.yaml`, `.env`, and skill packages into `/home/container/.hermes/`.
3. If using custom startup logic, copy `start.sh.example` to `/home/container/start.sh` and make it executable:
   ```bash
   chmod +x /home/container/start.sh
   ```
4. Start the server and monitor console logs for gateway readiness.

---

## 🛠️ Building Custom Super-Root Docker Image

For full browser automation (Playwright/Puppeteer) and root-level diagnostic tools within the container, build the provided Dockerfile on your Wings daemon node:

```bash
cd docker
docker build -t ptero-hermes-root:latest .
```

Ensure your Wings `config.yml` allows using locally tagged images or push the built image to your container registry.

---

## 📄 License & Credits

- **License**: MIT License
- **Author**: [ren](https://instagram.com/rskl411_) &bull; GitHub: [@rndsa](https://github.com/rndsa)
- **Copyright**: (c) 2026 ren. All rights reserved.
