# Gate Alarm — ESP-WROOM-32U + ESPHome + Home Assistant

A smart gate alarm system built on **ESP-WROOM-32U** with **ESPHome**, integrating natively into **Home Assistant**. Designed for residential use — detects gate open/close events and triggers alerts, automations, and sirens through Home Assistant.

> **Author:** skiid777
> **Board:** ESP-WROOM-32U DevKitC
> **Framework:** ESPHome + Arduino

---

## Overview

This project turns a simple magnetic contact sensor into a fully featured smart gate alarm. It runs locally — no cloud, no subscriptions, no third-party servers. Everything happens on your home network through Home Assistant.

The system is built in versions, starting from a minimal sensor and progressively adding alarm logic, LED indicators, diagnostics, and hardware protection.

---

## Hardware for v1

| Component | Role |
|---|---|
| ESP-WROOM-32U DevKitC + external antenna | Main microcontroller |
| Magnetic contact sensor (NC) | Gate open/close detection |

---

## Features

- Real-time gate open/close detection
- Fully local — no cloud dependency

---

## How it integrates with Home Assistant

```
Magnetic sensor (GPIO4)
        ↓
ESP-WROOM-32U (ESPHome firmware)
        ↓ WiFi — local network
Raspberry Pi (Home Assistant)
        ↓
Automations → siren, push notifications, camera
        ↓
HA app on your phone
```

---

## Roadmap

| Version | Status | Description |
|---|---|---|
| v1 | ✅ POC | **Proof of concept** — minimal, stable baseline. Magnetic sensor reporting open/closed state to HA in real time. Not a final product — foundation for future versions. |
| v2 | 🔄 Planned | Alarm logic — false alarm window, warning phase, auto-reset |
| v3 | 🔄 Planned | Hardware diagnostics — CPU temp, RAM, WiFi signal |
| v4 | 🔄 Planned | Offline mode, hardware error auto-restart |
| v5 | 🔄 Planned | Supercapacitor power backup detection and more |


## License

MIT License — free to use, modify and distribute.
Keep original author credits.

---

<p align="center">
Built with ESPHome · Runs on ESP-WROOM-32U · Integrates with Home Assistant
</p>
