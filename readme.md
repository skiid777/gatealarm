# ESP Gate Alarm

![Version](https://img.shields.io/badge/version-v1--POC-orange)
![License](https://img.shields.io/badge/license-MIT-green)
![ESPHome](https://img.shields.io/badge/built%20with-ESPHome-blue)
![Board](https://img.shields.io/badge/board-ESP32-red)
![HA](https://img.shields.io/badge/requires-Home%20Assistant-41BDF5)

A smart gate alarm system built on **ESP32** with **ESPHome**, integrating natively into **Home Assistant**. Open source, locally hosted, zero cloud dependency. Detects gate open/close events and triggers automations, push notifications and sirens through Home Assistant.

> **Author:** skiid777  
> **Board:** ESP32  
> **Status:** v1 — Proof of Concept  
> **License:** MIT

---

## Overview

This project turns a simple magnetic contact sensor (kontaktron) into a smart gate alarm. Everything runs on your local network — no cloud, no subscriptions, no third-party servers.

Built in progressive versions — v1 is the foundation. Each release adds a new layer on top.

---

## Requirements

- ESP32 board
- Magnetic contact sensor (kontaktron, NC type)
- 5V power supply or soldered power connection
- Home Assistant instance on local network
- ESPHome addon installed in Home Assistant

---

## Home Assistant

This project integrates natively with **Home Assistant** via the ESPHome integration. Once flashed, the ESP32 is auto-discovered by HA and all entities are available immediately.

From HA you can:
- See gate state (`open` / `closed`) in real time
- Build automations — siren, push notifications, camera
- Monitor ESP connection status
- Restart ESP remotely

---

## Installation

<details>
<summary><strong>How to install — click to expand</strong></summary>

### 1. Install ESPHome addon in Home Assistant
Settings → Add-ons → Add-on store → search **ESPHome** → Install → Start

### 2. Set up secrets
ESPHome dashboard → top right → **Secrets** → add:
```yaml
wifi_ssid: "YourNetworkName"
wifi_password: "YourPassword"
api_key: "generate with: openssl rand -base64 32"
ota_password: "gate123"
```

### 3. Create device
ESPHome → **+ New device** → Edit → paste `gate_alarm_v1.yaml` → Save

### 4. Flash ESP32
Connect via USB → ESPHome → **Install** → **Plug into this computer**

### 5. Add to Home Assistant
HA will auto-discover the device:
Settings → Integrations → ESPHome → Configure

> After first flash all updates can be done **wirelessly** — no USB needed.

For full details see [README_v1.md](README_v1.md)

</details>

---

## Roadmap

| Version | Status | Description |
|---|---|---|
| v1 | ✅ POC | Minimal sensor — gate open/closed state in HA |
| v2 | 🔄 Planned | 
| v3 | 🔄 Planned | 
| v4 | 🔄 Planned | 
| v5 | 🔄 Planned |
| v6 | 🔄 Planned | 

---

## License

MIT License — free to use, modify and distribute.  
Keep original author credits.

---

<p align="center">
Built with ESPHome &nbsp;·&nbsp; Runs on ESP32 &nbsp;·&nbsp; Integrates with Home Assistant
</p>
