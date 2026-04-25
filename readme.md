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

Sometimes a gate is left open accidentally. Sometimes someone opens it without your knowledge. Sometimes you simply want visibility, logs, and peace of mind.

Instead of using expensive commercial systems, this project provides a low-cost, privacy-focused, locally controlled solution built entirely around Home Assistant and ESPHome.

It is designed for:

- residential gate monitoring
- property awareness
- event logging inside Home Assistant
- future alarm automation
- reliable local-only operation

This is not intended to replace a certified professional alarm system.

It is a practical, expandable smart home security layer.

---

# Overview

ESP Gate Alarm starts with a very simple concept:

A magnetic contact sensor detects whether the gate is open or closed.

That state is sent to the **ESP32**, which reports it directly to **Home Assistant** through ESPHome over WiFi.

From there, Home Assistant can handle:

- notifications
- logs
- automation triggers
- sirens
- cameras
- security routines
- future alarm logic

Version 1 focuses only on creating a stable and reliable baseline.

Everything else builds on top of that.

---

# Hardware for v1

| Component | Role |
|---|---|
| ESP32 Dev Board + external antenna | Main controller |
| Magnetic contact sensor (NC) | Gate open/close detection |
| Stable 5V power supply | Permanent power source |
| Outdoor IP65+ enclosure | Weather protection |

### Installation Notes

The ESP will be installed outdoors inside a sealed weather-resistant enclosure mounted near the gate or wall.

Only the magnetic contact sensor wiring exits the enclosure.

Power can come from:

- standard 5V power supply
- USB charger adapter
- custom soldered permanent power solution

External antenna improves WiFi reliability for outdoor installation.

---

# Features (v1)

- Real-time gate open/close detection
- Native Home Assistant integration
- Fully local operation (no cloud)
- WiFi communication through ESPHome
- Simple and reliable NC magnetic sensor logic
- Event visibility inside Home Assistant
- Low-cost and low-maintenance design
- Designed for future expansion

Current state:

Only 2 states matter:

- Gate Open
- Gate Closed

Simple, stable, reliable.

---

# How It Integrates With Home Assistant

```text
Magnetic Contact Sensor (NC)
        ↓
ESP32 (ESPHome firmware)
        ↓ WiFi — local network only
Home Assistant Server
        ↓
Logs + State History + Automations
        ↓
Phone App / Notifications / Future Alarm Logic
