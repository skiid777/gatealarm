# ESP Gate Alarm — v1 POC

> **v1 is the simplest, most stable version.** It does one thing and does it well — detects gate open/close and reports to Home Assistant. Nothing more.

> **Author:** skiid777  
> **Board:** ESP32  
> **Version:** v1 — Proof of Concept

----
## Requirements
- ESP32 board
- Magnetic contact sensor (kontaktron, or smth NC)
- 5V power supply or soldered power connection
- Home Assistant instance on local network
- ESPHome addon installed in Home Assistant
----

## What it does

- Connects to WiFi
- Reads a magnetic contact sensor (kontaktron, NC type) on GPIO4
- Reports `open` or `closed` to Home Assistant in real time
- Shows whether ESP is online in HA

---

## Wiring

```
ESP32              Kontaktron
────────────────────────────────
GPIO4   ────────── Wire 1
GND     ────────── Wire 2
```

> No external resistor needed — internal pull-up is used (`pullup: true` in config).  
> NC type: magnet near = closed, magnet away = open.

---

## False alarm protection

Two debounce filters prevent false triggers from vibration or contact bounce:

```yaml
filters:
  - delayed_on: 200ms
  - delayed_off: 200ms
```

Gate must be open for **200ms continuously** before reporting. Short opens from wind or vibration are ignored.

---

## secrets.yaml

`secrets.yaml` keeps your WiFi credentials and API keys separate from the main code — so you can safely share or publish your code without exposing sensitive data.

ESPHome replaces every `!secret` reference with the real value before compiling. The ESP never sees the word "secret" — it gets the real value baked into firmware.

```yaml
wifi_ssid: "YourNetworkName"
wifi_password: "YourPassword"
api_key: ""
ota_password: "gate123"
```

> ⚠️ Never commit `secrets.yaml` to GitHub. Add it to `.gitignore`.



## Home Assistant

<details>
<summary><strong>What you see in HA — click to expand</strong></summary>

| Entity | Type | Description |
|---|---|---|
| Gate | Binary sensor (garage_door) | `open` or `closed` in real time |
| Gate Online | Binary sensor | whether ESP is connected |

**Example automation:**

Settings → Automations → Create:
```
Trigger:  Gate → changes to "open"
Action:   siren.turn_on → your siren
```

All through HA UI — no code needed.

</details>

---

## License

MIT License — free to use, modify and distribute.  
Keep original author credits.

---

<p align="center">
v1 POC &nbsp;·&nbsp; Built with ESPHome &nbsp;·&nbsp; Runs on ESP32 &nbsp;·&nbsp; Integrates with Home Assistant
</p>
