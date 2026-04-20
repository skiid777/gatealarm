# Gate Alarm v1 — Proof of Concept not ready for normal use

> This is the v1 POC release of the Gate Alarm project. It is a minimal, stable baseline — not a final product. The goal of v1 is to validate the hardware setup and Home Assistant integration before adding alarm logic, LEDs, diagnostics, and hardware protection in future versions.

> **Author:** skiid777
> **Board:** ESP-WROOM-32U DevKitC + external 2.4G antenna
> **Framework:** ESPHome + Arduino
> **Version:** v1 — POC

---

## What this version does

- Connects to WiFi
- Reads a magnetic contact sensor (NC) on GPIO4
- Reports `open` or `closed` state to Home Assistant in real time
- Shows whether the ESP is online in HA

Nothing more. No alarm logic, no LEDs, no automations built in.

---
All of the above come in future versions — see the main [README.md](README.md) for the full roadmap.
---

## Hardware

| Component | Role |
|---|---|
| ESP-WROOM-32U DevKitC + external antenna | Main microcontroller |
| Magnetic contact sensor (NC) | Gate open/close detection |
| 5V power supply | Stable power at gate |

No LEDs, no relays, no additional components needed for v1.

---

## Wiring

```
ESP-WROOM-32U     Magnetic sensor
─────────────────────────────────
GPIO4   ────── Signal wire
GND     ────── GND wire
```

Internal pull-up resistor used (`pullup: true`) — no external resistor needed.

---

## Pin assignment

| GPIO | Role |
|---|---|
| GPIO4 | Magnetic contact sensor (NC) |

---

## How it works

```
Magnetic sensor (GPIO4)
        ↓
ESP-WROOM-32U reads pin state
        ↓ WiFi — local network
Raspberry Pi (Home Assistant)
        ↓
HA dashboard shows "open" or "closed"
        ↓
Your automations decide what to do next
```

The sensor is **NC (normally closed)**:
- Magnet near sensor = circuit closed = GPIO LOW = **gate closed**
- Magnet away = circuit open = GPIO HIGH = **gate open**

`inverted: true` converts this so HA sees it correctly.

---

## False alarm protection

```yaml
filters:
  - delayed_on: 200ms
  - delayed_off: 200ms
```

Gate must be open for 200ms continuously before reporting. Short opens from wind or vibration are ignored.

---

## Installation

### 1. Install ESPHome addon in Home Assistant

Settings → Add-ons → Add-on store → **ESPHome** → Install → Start

### 2. Create secrets file

ESPHome dashboard → top right → **Secrets**:

```yaml
wifi_ssid: "YourNetworkName"
wifi_password: "YourPassword"
api_key: ""
ota_password: "gate123"
```

### 3. Create new device

ESPHome → **+ New device** → Edit → paste `v1 code ESP-WROOM-32U.yaml` → Save

### 4. Flash

Connect ESP via USB → ESPHome → **Install** → **Plug into this computer**

After first flash — all future updates wirelessly, no USB needed.

### 5. Add to Home Assistant

HA will auto-discover the device:

Settings → Integrations → ESPHome → Configure

---

## What you see in Home Assistant

| Entity | Type | Description |
|---|---|---|
| Gate | Binary sensor (garage_door) | `open` or `closed` |
| Gate Online | Binary sensor | ESP connection status |

---

## Creating automations in HA

Settings → Automations → Create:

```
Trigger:  Gate → changes to "open"
Action:   siren.turn_on → your siren
```

All through HA UI — no code needed.


## Why secrets.yaml is separate

Keeps credentials out of the code so you can safely publish to GitHub. ESPHome replaces every `!secret` with the real value before compiling — the ESP gets the real value baked into firmware.

---

## License

MIT License — free to use, modify and distribute.
Keep original author credits.

---

<p align="center">
v1 POC · Built with ESPHome · Runs on ESP-WROOM-32U · Integrates with Home Assistant
</p>
