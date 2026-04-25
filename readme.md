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

<details>
<summary><strong>v1 requirements — click to expand</strong></summary>

- ESP32 board
- Magnetic contact sensor (kontaktron, NC type)
- 5V power supply or soldered power connection
- Home Assistant instance on local network
- ESPHome addon installed in Home Assistant

</details>

---

## Home Assistant

This project integrates natively with **Home Assistant** via the ESPHome integration. Once flashed, the ESP32 is auto-discovered by HA and all entities are available immediately.

From HA you can:
- See gate state (`open` / `closed`) in real time
- Build automations — siren, push notifications, camera
- Monitor ESP connection status

---


---

## Roadmap

| Version | Status | Description |
|---|---|---|
| v1 | ✅ POC | Minimal simple version for tests |
| v2 | 🔄 Planned | TOP SECRET
| v3 | 🔄 Planned | TOP SECRET
| v4 | 🔄 Planned | TOP SECRET
| v5 | 🔄 Planned | TOP SECRET
| v6 | 🔄 Planned | TOP SECRET

---

## License

MIT License — free to use, modify and distribute.  
Keep original author credits.

---

<p align="center">
Built with ESPHome &nbsp;·&nbsp; Runs on ESP32 &nbsp;·&nbsp; Integrates with Home Assistant
</p>
