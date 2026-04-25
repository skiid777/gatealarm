# ESP Gate Alarm

![Status](https://img.shields.io/badge/status-active_development-green)
![Platform](https://img.shields.io/badge/platform-ESPHome-blue)
![Integration](https://img.shields.io/badge/Home%20Assistant-native-orange)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

A local-first smart gate monitoring system built on **ESP32** using **ESPHome**, designed for seamless integration with **Home Assistant**.

This project focuses on simple, reliable residential gate state detection using a magnetic contact sensor and WiFi-based local communication — no cloud, no subscriptions, no third-party servers.

Designed as a modular project that grows version by version, starting from a stable Proof of Concept and evolving into a more advanced gate security and automation platform.

> **Author:** skiid777  
> **Project:** ESP Gate Alarm  
> **Board:** ESP32 Dev Board  
> **Framework:** ESPHome + Arduino

---

# Why This Exists

The goal of this project is simple:

To always know whether the entrance gate is open or closed.

Real-life problems this solves:

- gate left open by accident
- unknown gate opening events
- lack of history/logs
- basic property awareness

Instead of expensive commercial systems, this is a low-cost, privacy-focused, fully local solution built around Home Assistant and ESPHome.

Use cases:

- residential gate monitoring
- event logging in Home Assistant
- automation triggers
- future alarm system expansion
- fully local operation

This is not a certified security system replacement.  
It is a flexible smart home security layer.

---

# Overview

System flow:

- magnetic contact sensor detects gate state
- ESP32 reads sensor state
- ESPHome sends state via WiFi
- Home Assistant receives and stores data

From there, HA can:

- show state (Open / Closed)
- log history
- trigger automations
- send notifications
- integrate with cameras and alarms (future)

---

# Hardware for v1

| Component | Role |
|---|---|
| ESP32 Dev Board | Main controller |
| Magnetic contact sensor (NC) | Gate state detection |
| 5V power supply | Permanent power |
| IP65+ enclosure | Outdoor protection |

### Installation notes

- installed outdoors (wall / gate area)
- ESP inside sealed enclosure
- only sensor wires exit box
- external antenna improves signal stability
- power can be:
  - USB adapter
  - soldered 5V PSU
  - custom permanent wiring

---

# Features (v1)

- real-time gate open/close detection
- Home Assistant native integration (ESPHome)
- local-only communication (no cloud)
- simple NC logic (reliable state detection)
- event tracking in HA
- low-cost hardware design
- foundation for future alarm system

Current state model:

- OPEN
- CLOSED

---

# Home Assistant Integration

Flow:

```text
Magnetic Sensor (NC)
        ↓
ESP32 (ESPHome firmware)
        ↓ WiFi (local network)
Home Assistant
        ↓
Logs / Automations / Notifications
