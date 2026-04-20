# Gate Alarm v1 — ESP-WROOM-32U + ESPHome

A minimal smart gate sensor built on **ESP-WROOM-32U** with **ESPHome**, integrating natively into **Home Assistant**. This is version 1 — the simplest possible foundation. It detects whether the gate is open or closed and reports the state to Home Assistant in real time.

> **Author:** skiid777
> **Board:** ESP-WROOM-32U DevKitC-1
> **Framework:** ESPHome + Arduino
> **Version:** v1 — minimal

---

## What it does

- Connects to WiFi
- Reads a magnetic contact sensor (reed switch / magnetic contactor) on GPIO4
- Reports `open` or `closed` state to Home Assistant instantly
- Shows whether the ESP is online in HA

Nothing more, nothing less. No LEDs, no alarm logic, no automations built in. Those come in later versions.

---

## How it works

```
Magnetic sensor (GPIO4)
        ↓
ESP-WROOM-32U reads pin state
        ↓ WiFi (local network)
Raspberry Pi running Home Assistant
        ↓
HA dashboard shows "open" or "closed"
        ↓
Your automations decide what to do next
```

The sensor is **NC (normally closed)**:
- Magnet near sensor = circuit closed = GPIO LOW = **gate closed**
- Magnet away from sensor = circuit open = GPIO HIGH = **gate open**

`inverted: true` in the config converts this so HA sees it correctly.

---

## Hardware

| Component | Details |
|---|---|
| ESP-WROOM-32U DevKitC-1 | Main microcontroller |
| Magnetic contact sensor (NC) | Gate open/close detection |
| 5V power supply | Stable power at the gate |

No LEDs, no relays, no additional components needed for v1.

---

## Wiring

```
ESP-WROOM-32U        Magnetic sensor
─────────────────────────────────
GPIO4   ────── Signal wire
GND     ────── GND wire
```

The internal pull-up resistor is used (`pullup: true`) — no external resistor needed.

---

## Pin assignment

| GPIO | Role |
|---|---|
| GPIO4 | Magnetic contact sensor (NC) |

---

## False alarm protection

Two debounce filters prevent false triggers from vibration or contact bounce:

```yaml
filters:
  - delayed_on: 200ms   # gate must be open for 200ms before reporting
  - delayed_off: 200ms  # gate must be closed for 200ms before reporting
```

Short opens (wind, vibration) under 200ms are completely ignored.

---

## Installation

### 1. Install ESPHome addon in Home Assistant

Settings → Add-ons → Add-on store → search **ESPHome** → Install → Start

### 2. Create secrets file

In ESPHome dashboard → top right corner → **Secrets** → add:

```yaml
wifi_ssid: "YourNetworkName"
wifi_password: "YourPassword"
api_key: ""
ota_password: "gate123"
```

> `api_key` will be generated automatically on first compile — leave it empty for now.

### 3. Create new device

In ESPHome dashboard → **+ New device** → give it a name → click **Edit** → paste the code from `gate_alarm_v1.yaml` → **Save**

### 4. Flash for the first time

Connect ESP-WROOM-32U via USB to your computer → in ESPHome click **Install** → **Plug into this computer**

> After first flash, all future updates can be done **wirelessly** — no USB needed.

### 5. Add to Home Assistant

After flashing, HA will auto-discover the device:

**Settings → Integrations → ESPHome → Configure**

---

## What you see in Home Assistant

| Entity | Type | Description |
|---|---|---|
| Gate | Binary sensor (garage_door) | `open` or `closed` in real time |
| Gate Online | Binary sensor | whether ESP is connected |

---

## Creating automations in HA

The sensor reports state — what happens next is up to your automations.

**Example — trigger siren when gate opens:**

Settings → Automations → Create automation:
```
Trigger:  Gate → changes to "open"
Action:   siren.turn_on → your siren entity
```

All done through the HA UI — no code needed.

---

## File structure

```
gate-alarm/
├── gate_alarm_v1.yaml   ← ESPHome configuration
├── secrets.yaml         ← WiFi credentials (never commit this!)
└── README.md            ← this file
```

> Add `secrets.yaml` to `.gitignore` — never push it to GitHub:
> ```
> secrets.yaml
> ```

---

## Why secrets.yaml is a separate file

The `secrets.yaml` file keeps sensitive data (WiFi password, API keys) separate from the main code. This means you can share or publish your code without exposing your credentials.

When ESPHome compiles the code, it replaces every `!secret` reference with the actual value from `secrets.yaml` before building the firmware. The ESP never sees the word "secret" — it gets the real value baked into the firmware.

```
gate_alarm_v1.yaml          secrets.yaml
!secret wifi_ssid     →     wifi_ssid: "YourNetwork"
!secret wifi_password →     wifi_password: "YourPassword"
```

---
