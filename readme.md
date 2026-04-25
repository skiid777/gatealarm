# ESP Gate Alarm

![Status](https://img.shields.io/badge/status-active_development-green)
![Platform](https://img.shields.io/badge/platform-ESPHome-blue)
![Integration](https://img.shields.io/badge/Home%20Assistant-native-orange)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

A local-first smart gate monitoring system built on **ESP32** using **ESPHome**, designed for seamless integration with **Home Assistant**.

It tracks gate open/close state using a magnetic contact sensor and sends updates locally over WiFi. No cloud, no subscriptions, no external services.

> **Author:** skiid777  
> **Project:** ESP Gate Alarm  
> **Board:** ESP32 Dev Board  
> **Framework:** ESPHome + Arduino

---

# Why This Exists

This project exists to solve one simple problem: knowing the gate state at all times.

Real-world use cases:
- gate left open by accident
- unknown or unauthorized opening
- no history of gate events
- basic property monitoring

It is a low-cost, privacy-first, fully local smart home security layer.

Not a certified alarm system replacement — a flexible DIY security/automation base.

---

# Features

## Core (v1)

- real-time gate open/close detection
- magnetic contact sensor (NC)
- ESP32 ESPHome firmware
- Home Assistant native integration
- fully local operation (no cloud)
- simple binary state (OPEN / CLOSED)
- instant state updates via WiFi
- event tracking in Home Assistant
- external antenna support for stability
- low-cost hardware design
- outdoor-ready installation (IP65 enclosure)

---

## Home Assistant Features

- automatic device discovery
- entity exposed as binary sensor
- dashboard integration
- automation triggers
- event history logging
- notification support
- camera/siren integration ready (future use)

---

## Reliability Features (planned / partial)

- WiFi reconnect handling
- ESPHome watchdog support
- boot recovery behavior
- stable state reporting after reboot

---

## Security & Awareness Features (planned)

- gate open too long detection
- night-time intrusion alerts
- activity logging timeline
- suspicious activity detection rules
- multiple gate support (future expansion)

---

## Hardware Features

- ESP32 main controller
- NC magnetic sensor input
- 5V stable power input
- optional USB / direct solder power
- IP65+ outdoor enclosure
- external antenna for range improvement
- minimal wiring (only sensor lines exposed)

---

# System Architecture

```text
Magnetic Contact Sensor (NC)
        ↓
ESP32 (ESPHome)
        ↓ WiFi (local network)
Home Assistant
        ↓
Logs / Automations / Alerts / UI
