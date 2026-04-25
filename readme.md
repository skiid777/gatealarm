# Gate Alarm — ESP32 + ESPHome + Home Assistant

A smart gate alarm system built on **ESP32** with **ESPHome**, integrating natively into **Home Assistant**. Designed for residential use — detects gate open/close events and triggers alerts, automations, and sirens through Home Assistant.

> **Author:** skiid777
> **Board:** ESP-WROOM-32 DevKit
> **Framework:** ESPHome + Arduino

---

## Overview

This project turns a simple magnetic contact sensor into a fully featured smart gate alarm. It runs locally — no cloud, no subscriptions, no third-party servers. Everything happens on your home network through Home Assistant.

The system is built in versions, starting from a minimal sensor and progressively adding alarm logic, LED indicators, diagnostics, and hardware protection.

---

## Hardware

| Component | Role |
|---|---|
| ESP32 DevKit (ESP-WROOM-32) | Main microcontroller |
| Magnetic contact sensor (NC) | Gate open/close detection |
| 4x LED (green, blue, yellow, red) | System status indicators |
| Supercapacitor (10F / 5.5V) | Power backup on mains cut |
| Diode Schottky 1N5817 | Protects supercap discharge direction |
| 5V power supply | Stable power at gate |

---

## Features

- Real-time gate open/close detection
- Multi-phase alarm timeline with false alarm protection
- 4-LED status system — instant visual feedback
- WiFi diagnostics with push notifications
- Hardware health monitoring (CPU temp, RAM, WiFi signal)
- Unexpected restart detection via hardware register
- Police light effect on critical errors
- Auto-restart after hardware failure (45s)
- Auto-reset after gate closes (60s timer)
- One-time-use alarm model — closing gate does not reset alarm
- Supercapacitor backup — alerts even when power is cut
- Fully local — no cloud dependency

---

## How it integrates with Home Assistant

```
Magnetic sensor (GPIO4)
        ↓
ESP32 (ESPHome firmware)
        ↓ WiFi — local network only
Raspberry Pi (Home Assistant)
        ↓
Automations → siren, push notifications, camera
        ↓
HA app on your phone — anywhere in the world
```

---

## Why ESPHome?

| | ESPHome | Arduino IDE |
|---|---|---|
| Code | YAML | C++ |
| HA integration | Automatic | Manual (200+ lines) |
| OTA updates | WiFi, no USB | USB only |
| Time to working sensor | 5 minutes | Hours |

---

## Roadmap

| Version | Status | Description |
|---|---|---|
| v1 | ✅ POC | **Proof of concept** — minimal, stable baseline. Magnetic sensor reporting open/closed to HA. Not a final product. |
| v2 | 🔄 Planned | Alarm logic — false alarm window, warning phase, auto-reset |
| v3 | 🔄 Planned | 4 LED indicators — full visual status system |
| v4 | 🔄 Planned | Hardware diagnostics — CPU temp, RAM, WiFi signal |
| v5 | 🔄 Planned | Offline mode, police effect, auto-restart |
| v6 | 🔄 Planned | Supercapacitor power backup detection |

---

## File structure

```
gate-alarm/
├── gate_alarm_v1.yaml       ← v1 ESPHome config (POC)
├── secrets.yaml             ← credentials (never commit!)
├── README.md                ← this file
└── README_v1.md             ← detailed v1 documentation
```

> ⚠️ Never commit `secrets.yaml` to GitHub. Add it to `.gitignore`:
> ```
> secrets.yaml
> ```

---

## License

MIT License — free to use, modify and distribute.
Keep original author credits.

---

<p align="center">
Built with ESPHome &nbsp;·&nbsp; Runs on ESP32 &nbsp;·&nbsp; Integrates with Home Assistant
</p>
