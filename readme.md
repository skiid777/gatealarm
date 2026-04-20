# Gate Alarm — ESP32-C6 + ESPHome + Home Assistant

A smart gate alarm system built on **ESP32-C6** with **ESPHome**, integrating natively into **Home Assistant**. Designed for residential use — detects gate open/close events and triggers alerts, automations, and sirens through Home Assistant.

> **Author:** skiid777
> **Board:** ESP32-C6 DevKitC-1
> **Framework:** ESPHome + ESP-IDF

---

## Overview

This project turns a simple magnetic contact sensor into a fully featured smart gate alarm. It runs locally — no cloud, no subscriptions, no third-party servers. Everything happens on your home network through Home Assistant.

The system is built in versions, starting from a minimal sensor and progressively adding alarm logic, LED indicators, diagnostics, and hardware protection.

---

## Hardware

| Component | Role |
|---|---|
| ESP32-C6 DevKitC-1 | Main microcontroller |
| Magnetic contact sensor (NC) | Gate open/close detection |
| 4x LED (green, blue, yellow, red) | System status indicators |
| Supercapacitor (10F) | Power backup on mains cut |
| 5V power supply | Stable power at gate |

---

## Features

- Real-time gate open/close detection
- Multi-phase alarm timeline with false alarm protection
- 4-LED status system — instant visual feedback
- WiFi diagnostics with push notifications
- Hardware health monitoring (CPU temp, RAM, WiFi signal)
- Unexpected restart detection
- Police light effect on critical errors
- Auto-restart after hardware failure
- Auto-reset after gate closes
- One-time-use alarm model — closing gate does not reset alarm
- Supercapacitor backup — alerts even when power is cut
- Fully local — no cloud dependency

---

## How it integrates with Home Assistant

```
Magnetic sensor (GPIO4)
        ↓
ESP32-C6 (ESPHome firmware)
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
| v1 | ✅ Done | **Lightweight** — minimal, stable, production ready. Magnetic sensor reporting open/closed state to HA in real time. No extras, just works. |
| v2 | 🔄 Planned | Alarm logic — false alarm window, warning phase, auto-reset |
| v3 | 🔄 Planned | 4 LED indicators — full visual status system |
| v4 | 🔄 Planned | Hardware diagnostics — CPU temp, RAM, WiFi signal |
| v5 | 🔄 Planned | Offline mode, police effect, auto-restart |
| v6 | 🔄 Planned | Supercapacitor power backup detection |

---

## File structure

```
gate-alarm/
├── gate_alarm_v1.yaml       ← v1 ESPHome config (minimal)
├── secrets.yaml             ← credentials (never commit!)
└── README.md                ← this file
```

---

## License

MIT License — free to use, modify and distribute.
Keep original author credits.

---

<p align="center">
Built with ESPHome · Runs on ESP32-C6 · Integrates with Home Assistant
</p>
