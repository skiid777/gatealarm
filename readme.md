# 🚨 Gate Alarm — ESP32-C6 + ESPHome

A smart gate alarm system built on **ESP32-C6** with **ESPHome**, integrating natively into **Home Assistant**. Features a 3-LED status system, a multi-phase alarm timeline with false-alarm protection, hardware diagnostics, and a **one-time-use** security model — once triggered, only an ESP restart resets the system.

> **Author:** skiid777  
> **Board:** ESP32-C6 DevKitC-1  
> **Framework:** ESPHome + ESP-IDF  
> **Version:** v4

---

## 📋 Table of Contents

- [Features](#-features)
- [Hardware](#-hardware)
- [Wiring](#-wiring)
- [LED Logic](#-led-logic)
- [Alarm Timeline](#-alarm-timeline)
- [Diagnostics](#-diagnostics)
- [Home Assistant Events](#-home-assistant-events)
- [Installation](#-installation)
- [Secrets File](#-secrets-file)
- [Security Design](#-security-design)
- [File Structure](#-file-structure)

---

## ✨ Features

- **3-LED status system** — green (status), yellow (WiFi), red (alarm)
- **Multi-phase alarm** — false alarm window → warning → alarm
- **One-time-use model** — closing the gate does NOT reset the alarm
- **Police light effect** — green/red alternating on critical hardware errors
- **Offline mode** — ESP stops monitoring gate on critical error
- **Hardware diagnostics** — CPU temp, free RAM, WiFi signal, uptime
- **Unexpected restart detection** — via `esp_reset_reason()`, zero flash writes
- **Push notifications** — via Home Assistant `notify.mobile_app`
- **Native HA integration** — events, binary sensors, sensors, restart button

---

## 🔩 Hardware

| Component | Description |
|---|---|
| ESP32-C6 DevKitC-1 | Main microcontroller |
| Reed switch (NC) | Gate open/close detection |
| Green LED + 330Ω resistor | Status / PWR indicator |
| Yellow LED + 330Ω resistor | WiFi status indicator |
| Red LED + 330Ω resistor | Alarm / error indicator |
| 5V power supply | Stable power at gate |

---

## 🔌 Wiring

```
ESP32-C6          Component
─────────────────────────────────────────
GPIO7   ────────► Reed switch (NC)
                  Other leg → GND

GPIO10  ────────► 330Ω ──► Green LED ──► GND
GPIO2   ────────► 330Ω ──► Yellow LED ──► GND
GPIO8   ────────► 330Ω ──► Red LED ──► GND

3.3V    ────────► 330Ω ──► Power LED ──► GND  (always on, no GPIO needed)
GND     ────────► Common ground
```

> **Note:** The reed switch uses the ESP32's internal pull-up resistor (`pullup: true` in config). No external pull-up needed.

---

## 💡 LED Logic

Three LEDs, each with a dedicated role:

| LED | GPIO | Role |
|---|---|---|
| 🟢 Green | GPIO10 | System status / heartbeat |
| 🟡 Yellow | GPIO2 | WiFi connection |
| 🔴 Red | GPIO8 | Alarm / hardware error |

### Visual reference

👉 **[View animated LED logic →](gate_alarm_led_logic.html)**

Open `gate_alarm_led_logic.html` in any browser — all LED states are animated in real-time, no server required.

### State table

| 🟢 Green | 🟡 Yellow | 🔴 Red | State | Description |
|:---:|:---:|:---:|---|---|
| slow blink | OFF | OFF | **Boot** | ESP starting up |
| fast blink | OFF | OFF | **WiFi connecting** | Scanning for network (200ms) |
| slow blink | solid | OFF | **All OK** | WiFi up, gate closed |
| slow blink | solid | OFF | **Silence 0–2s** | Gate open, false alarm window |
| slow blink | solid | solid | **Warning 2–4s** | Gate still open, last chance |
| sync blink | solid | sync blink | **🚨 ALARM** | Alarm triggered, event sent to HA |
| alt. blink | OFF | alt. blink | **🚔 OFFLINE** | Critical HW error — police effect |
| slow blink | fast blink | OFF | **Weak WiFi** | Push only, ESP stays online |
| slow blink | solid | OFF | **Unexpected restart** | Push only, normal operation |

> **Sync blink** = green and red blink simultaneously (400ms)  
> **Alt. blink** = green and red alternate (150ms) — police effect

---

## ⏱ Alarm Timeline

```
t = 0ms       Gate opens
              └─ 50ms debounce filter applied (contact bounce)

t = 0 → 2s   FALSE ALARM WINDOW
              └─ ESP waits silently
              └─ If gate closes here → nothing happens, back to OK

t = 2s        WARNING PHASE BEGINS
              └─ Red LED turns solid
              └─ 2 more seconds to close gate

t = 2s → 4s  WARNING
              └─ Gate still open
              └─ If gate closes here → alarm state persists (one-time-use)

t = 4s        🚨 ALARM TRIGGERS
              └─ Green + red blink together (400ms)
              └─ Event fired: esphome.gate_alarm
              └─ HA automation triggers siren

gate closes   GATE CLOSED — ALARM PERSISTS
              └─ Closing the gate does NOT reset the system
              └─ LEDs keep blinking
              └─ Event fired: esphome.gate_closed (info only)

restart only  SYSTEM RESET
              └─ Physical power cycle OR "Gate Restart" button in HA
              └─ Returns to boot sequence
```

---

## 🩺 Diagnostics

The ESP32-C6 continuously monitors its own hardware health and reports to Home Assistant.

| Metric | Threshold | Action |
|---|---|---|
| CPU Temperature | > 50°C | Push notification + **offline mode** |
| Free RAM | < 20 KB | Push notification + **offline mode** |
| WiFi Signal | < −80 dBm | Push notification only |
| Unexpected restart | crash / brownout / watchdog | Push notification on boot |

### Offline mode

When CPU overheating or RAM exhaustion is detected:

1. `offline_mode` flag is set to `true`
2. Gate monitoring is completely disabled
3. Police light effect activates (green ↔ red at 150ms)
4. Yellow LED turns off
5. Push notification sent to phone
6. **Only an ESP restart exits offline mode**

> Offline mode uses `esp_reset_reason()` from ESP-IDF to detect crash causes — this reads a hardware register and performs **zero flash writes**, meaning no flash wear.

### Entities visible in HA

| Entity | Type | Update |
|---|---|---|
| Gate CPU Temperature | Sensor (°C) | every 30s |
| Gate Free RAM | Sensor (KB) | every 60s |
| Gate WiFi dBm | Sensor (dBm) | every 60s |
| Gate WiFi % | Sensor (%) | every 60s |
| Gate Uptime | Sensor (s) | every 60s |
| Gate IP | Text sensor | on change |
| Gate SSID | Text sensor | on change |
| Gate | Binary sensor (garage_door) | real-time |
| Gate Online | Binary sensor | real-time |

---

## 📡 Home Assistant Events

| Event | Fired when | Suggested HA action |
|---|---|---|
| `esphome.gate_alarm` | Gate open > 4s | Trigger siren automation |
| `esphome.gate_closed` | Gate physically closes | Info only — alarm persists |
| `notify.mobile_app` | Weak WiFi / HW error / crash | Push to phone |

### Example HA automation (trigger siren)

```yaml
automation:
  - alias: "Gate Alarm — Trigger Siren"
    trigger:
      - platform: event
        event_type: esphome.gate_alarm
    action:
      - service: siren.turn_on
        target:
          entity_id: siren.your_siren_entity
```

### Example HA automation (restart ESP to reset alarm)

```yaml
automation:
  - alias: "Gate Alarm — Reset via HA"
    trigger:
      - platform: event
        event_type: esphome.gate_closed
    condition:
      - condition: state
        entity_id: binary_sensor.gate_online
        state: "on"
    action:
      - service: button.press
        target:
          entity_id: button.gate_restart
```

---

## 🛠 Installation

### 1. Flash ESPHome

Open **Chrome** or **Edge** and go to:

```
https://web.esphome.io
```

1. Connect ESP32-C6 via USB
2. Click **Connect** → select the USB port
3. Click **Install** → select **ESP32-C6**
4. Enter your WiFi credentials when prompted
5. ESP connects to your network automatically

### 2. Add to ESPHome dashboard

1. In Home Assistant → **Settings → Add-ons → ESPHome**
2. Click **+ New device** → **Take over**
3. Paste the contents of `gate_alarm_v4_en.yaml`
4. Make sure `secrets.yaml` is configured (see below)
5. Click **Install**

### 3. Add to Home Assistant

After flashing, HA will auto-discover the device:

**Settings → Integrations → ESPHome → Configure**

---

## 🔑 Secrets File

Create `secrets.yaml` in your ESPHome config directory:

```yaml
wifi_ssid: "YOUR_WIFI_NAME"
wifi_password: "YOUR_WIFI_PASSWORD"
api_key: "generated_automatically_by_esphome"
ota_password: "choose_any_password_eg_gate123"
```

> The `api_key` is generated automatically when you first create the device in the ESPHome dashboard. You can also generate one manually at [esphome.io/components/api](https://esphome.io/components/api.html).

---

## 🔐 Security Design

### One-time-use alarm model

This system is designed to behave like a commercial door/window alarm sensor — two states only:

- **State 1** — gate closed, system armed ✅
- **State 2** — alarm triggered 🚨

Once the alarm triggers, **no action at the gate can reset it**. An intruder cannot silence the alarm by simply closing the gate. The only reset method is a full ESP restart, which requires either:

- Physical access to the ESP power supply
- The **"Gate Restart"** button in Home Assistant (accessible remotely via Tailscale or Nabu Casa)

### False alarm protection

Three layers of protection against false triggers:

| Layer | Protection |
|---|---|
| 50ms debounce filter | Eliminates contact bounce (reed switch chatter) |
| 2s false alarm window | Short opens (wind, vibration) are ignored |
| 2s warning phase | Intentional delay before alarm fires |

---

## 📁 File Structure

```
gate-alarm/
├── gate_alarm_v4_en.yaml     # Main ESPHome configuration
├── secrets.yaml              # WiFi credentials and API keys (not in git!)
├── gate_alarm_led_logic.html # Animated LED logic documentation
└── README.md                 # This file
```

> ⚠️ **Never commit `secrets.yaml` to git.** Add it to `.gitignore`:
> ```
> secrets.yaml
> ```

---

## 📄 License

MIT License — feel free to use, modify and distribute.  
Keep original author credits.

---

<p align="center">
  Built with ESPHome · Runs on ESP32-C6 · Integrates with Home Assistant
</p>
