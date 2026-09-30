# Hermes Agent — Pterodactyl Egg

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform: Pterodactyl](https://img.shields.io/badge/Platform-Pterodactyl%20Panel-007ACC.svg)](https://pterodactyl.io/)
[![Standard: PTDL v2](https://img.shields.io/badge/Standard-PTDL__v2-blue.svg)](https://pterodactyl.io/)
[![Runtime: Python 3.11](https://img.shields.io/badge/Runtime-Python%203.11%20%7C%20Playwright-3776AB.svg)](https://python.org)
[![Supervisor: psvc](https://img.shields.io/badge/Supervisor-psvc%20(No--Systemd)-00d992.svg)]()
[![Author: ren](https://img.shields.io/badge/Author-ren%20(%40rskl411_)-ff70a6.svg)](https://instagram.com/rskl411_)

> Production-grade Pterodactyl Panel Egg engineered for deploying autonomous AI agents ([Hermes Agent by Nous Research](https://github.com/NousResearch/Hermes-Agent)) inside isolated server containers.

---

## ⚡ Kelebihan & Fitur Utama

Arsitektur egg ini dirancang khusus untuk mengatasi keterbatasan umum container game server pada Pterodactyl:

* **99% VPS Capability Parity**:
  * Menghadirkan kapabilitas setara 99% VPS penuh di dalam sandbox container Pterodactyl (interactive TUI shell, continuous background daemon, supervisor proses multi-layanan, otomasi browser headless, dan public port tunneling tanpa dependensi alokasi port panel).
* **Dual-Engine Execution Mode (`start.sh`)**:
  * **Foreground Console**: Web terminal panel langsung memuat CLI interaktif resmi Hermes Agent (Caduceus ASCII art, live status bar, chat langsung, dan eksekusi tools real-time).
  * **Background Gateway**: Daemon polling Telegram aktif 24/7 di latar belakang dengan proteksi auto-restart loop (`/home/container/gateway.log`).
* **Built-in Auto-Installer**:
  * Begitu server di-create di panel, script instalasi otomatis mengunduh binary `cloudflared` (tunneling port publik tanpa perlu alokasi port panel) dan supervisor `psvc`.
* **Pengganti Systemd (`psvc` Supervisor)**:
  * Manajer proses ringan pengganti `systemd`/`pm2` untuk container non-root. Mendukung monitoring otomatis, auto-restart jika crash, dan auto-boot via `/home/container/services.json`.
* **Headless Browser & Root Ready**:
  * Mendukung image `ptero-hermes-root:latest` (passwordless sudo, Node.js, Playwright Chromium, dev & network utilities). Dilengkapi flag mitigasi memory shared (`--disable-dev-shm-usage`) untuk otomasi browser anti-crash.
* **Variabel Startup Web Panel**:
  * Seluruh konfigurasi penting (`HERMES_PROFILE`, `STARTUP_MODE`, `TELEGRAM_BOT_TOKEN`, `TELEGRAM_ALLOWED_USERS`) dapat diatur langsung dari tab Startup tanpa perlu edit file `.env` manual.

---

## ⚠️ Kekurangan & Batasan Sistem

Sebagai transparansi teknis, berikut beberapa batasan operasional pada environment container Pterodactyl:

* **Non-Kernel Systemd**: Container Pterodactyl tidak memiliki PID 1 systemd native; seluruh background daemon dan sub-proses bergantung pada supervisor `psvc` atau script subshell.
* **Shared Memory (`/dev/shm`) Limit**: Jika node Wings menggunakan alokasi RAM kecil (<1 GB), rendering tab Playwright Chromium yang terlalu banyak dapat memicu OOM (disarankan alokasi RAM minimal 2–4 GB).
* **Ephemeral Tunnel Hostnames**: Fitur `psvc tunnel <port>` menggunakan quick tunnel Cloudflare (`*.trycloudflare.com`) secara default. Untuk domain permanen, diperlukan konfigurasi tunnel token Cloudflare tersendiri.

---

## 🚀 Cara Pakai & Panduan Instalasi

### 1. Import Egg ke Panel Pterodactyl
1. Buka Admin Panel Pterodactyl (e.g. `https://pt.domain.com/admin`).
2. Masuk ke menu **Nests** &rarr; pilih nest tujuan (misal: `Generic` atau buat nest baru `AI Agents`).
3. Klik tombol **Import Egg** di pojok kanan atas.
4. Pilih file **`egg-hermes-agent.json`** dari repository ini.
5. Simpan egg dan pastikan Docker images terdaftar.

### 2. Buat Server Instance
1. Masuk ke menu **Servers** &rarr; **Create New**.
2. Alokasikan resource (Rekomendasi: CPU 200%+, RAM minimum 2048 MB, Disk 10 GB+).
3. Pada bagian **Nest Configuration**, pilih egg **Hermes Agent**.
4. Pilih Docker Image:
   * `ptero-hermes-root:latest` *(Sangat Disarankan)*: Mendukung Playwright, passwordless sudo, dan tools sistem lengkap.
   * `ghcr.io/ptero-eggs/yolks:python_3.11`: Image standar Python resmi yolks.
5. Klik **Create Server** dan tunggu hingga instalasi container selesai.

### 3. Konfigurasi Startup Variables
Buka server di panel client &rarr; tab **Startup**:

| Variabel | Deskripsi | Default | Opsi Nilai |
|---|---|---|---|
| `HERMES_PROFILE` | Profile Hermes yang dimuat | `default` | `default`, `bot2`, custom |
| `STARTUP_MODE` | Mode eksekusi utama | `interactive` | `interactive`, `gateway_only`, `bash` |
| `TELEGRAM_BOT_TOKEN` | Token Bot Telegram (@BotFather) | *Kosong* | String token bot |
| `TELEGRAM_ALLOWED_USERS` | Whitelist ID Telegram | *Kosong* | ID angka Telegram (koma untuk multi-user) |

### 4. Menjalankan Server
1. Masuk ke tab **Console** &rarr; klik tombol **Start**.
2. Web terminal akan langsung menampilkan CLI interaktif Hermes Agent.
3. Bot Telegram di background akan langsung aktif menerima pesan dari user yang di-whitelist.

---

## 🛠️ Manajemen Background Process via `psvc`

Gunakan perintah `psvc` langsung di web console panel untuk mengelola daemon tambahan:

```bash
# Menampilkan status seluruh service
psvc status

# Menjalankan service baru di background
psvc start my-api "node server.js" --cwd /home/container/api

# Mengecek log service
psvc logs my-api -f

# Membuka port internal ke publik via Cloudflare Tunnel
psvc tunnel 3000
```

---

## 📄 Lisensi & Kontributor

* **Lisensi**: MIT License
* **Author**: [ren](https://instagram.com/rskl411_) • GitHub: [@rndsa](https://github.com/rndsa)
* Seluruh hak cipta dilindungi (c) 2026 ren.
