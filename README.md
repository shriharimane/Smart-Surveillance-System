# Smart IoT Surveillance System 📷🚨
### ESP32-CAM + PIR Sensor | Motion Detection · Telegram Alerts · Live Streaming

A low-cost, energy-efficient IoT security system that detects motion, captures images, sends instant Telegram alerts, and streams live video — all over Wi-Fi. Built around the ESP32-CAM module and an HC-SR501 PIR sensor with deep sleep for minimal power consumption.

---

## Overview

This system stays in deep sleep until the PIR sensor detects motion. On detection, the ESP32-CAM wakes up, captures an image, sends a real-time alert with the photo to a Telegram bot, and makes a live video feed available via browser. Once done, it returns to low-power mode. The result is an event-driven surveillance system that runs efficiently on battery power.

---

## System Architecture

### Block Diagram

```
[7.4V Battery Pack (2× 18650)]
            ↓
    [Buck Converter → 5V]
       ↙           ↘
[ESP32-CAM]     [PIR Sensor]
     ↑                ↓
     └── GPIO 3 ← Motion Signal
            ↓
    [Wi-Fi / Internet]
       ↙        ↘
[Telegram Bot]  [Live Stream (Browser)]
```

### Circuit Connections

| ESP32-CAM Pin | PIR Sensor (HC-SR501) |
|---|---|
| 5V | VCC |
| GND | GND |
| GPIO 3 | OUT |

> Two 18650 batteries provide 7.4V input. A buck converter steps this down to a stable 5V for both the ESP32-CAM and PIR sensor. The PIR OUT pin connects to GPIO 3 to trigger a wake-up interrupt from deep sleep.

### System Flowchart

```
Start
  └─▶ Initialize ESP32-CAM & PIR sensor
        └─▶ Enter deep sleep (low-power mode)
              └─▶ PIR detects motion?
                    ├─ No  → Stay in deep sleep
                    └─ Yes → Wake ESP32-CAM
                                └─▶ Capture image / video
                                      └─▶ Send Telegram alert with image
                                            └─▶ Start live stream (optional)
                                                  └─▶ Return to deep sleep
```

---

## Hardware Requirements

| Component | Purpose | Est. Cost (INR) |
|---|---|---|
| ESP32-CAM | Microcontroller + camera module | ₹600 |
| PIR Sensor (HC-SR501) | Motion detection | ₹100 |
| FTDI Converter | Programming the ESP32-CAM | ₹400 |
| 18650 Batteries (×2) | Power supply (7.4V) | — |
| Buck Converter | Voltage regulation (7.4V → 5V) | ₹200 |
| Power Adapter | Alternate power source | included |
| Jumper Wires & Breadboard | Circuit connections | ₹150 |
| **Total** | | **₹1,450** |

---

## Software Requirements

| Tool / Library | Purpose |
|---|---|
| Arduino IDE / VS Code (PlatformIO) | Code writing and flashing |
| ESP32 Core Libraries | GPIO, timers, Wi-Fi support |
| `esp32-camera` library | Camera capture and control |
| `WiFi.h` | Network connectivity |
| `HTTPClient.h` | Telegram API communication |
| PIR Interrupt code | Motion-based wake-up from deep sleep |

---

## Getting Started

### 1. Flash the ESP32-CAM

Connect the ESP32-CAM to your computer via an FTDI converter:

| FTDI | ESP32-CAM |
|---|---|
| TX | RX (U0R) |
| RX | TX (U0T) |
| GND | GND |
| 5V | 5V |
| — | IO0 → GND (flash mode) |

Open the project in **Arduino IDE** or **PlatformIO**, select the `AI Thinker ESP32-CAM` board, and flash the firmware.

> Remove the IO0 → GND jumper after flashing and reset the board.

### 2. Configure credentials

Edit the following in the sketch before flashing:

```cpp
const char* ssid       = "YOUR_WIFI_SSID";
const char* password   = "YOUR_WIFI_PASSWORD";
const String botToken  = "YOUR_TELEGRAM_BOT_TOKEN";
const String chatID    = "YOUR_TELEGRAM_CHAT_ID";
```

### 3. Create a Telegram Bot

1. Open Telegram and search for **@BotFather**
2. Send `/newbot` and follow the prompts
3. Copy the **bot token** into the sketch
4. Start a chat with your bot and get your **chat ID** via `https://api.telegram.org/bot<token>/getUpdates`

### 4. Power and run

Assemble the circuit, power via battery pack or adapter, and the system will begin monitoring immediately.

---

## Features

- **Motion-triggered wake-up** — PIR interrupt pulls ESP32 out of deep sleep only when needed
- **Instant image capture** — ESP32-CAM captures a photo on every motion event
- **Telegram alerts** — photo sent directly to your Telegram chat in real time
- **Live video streaming** — accessible via browser on the local Wi-Fi network
- **Deep sleep between events** — significantly reduces power draw for battery operation
- **Low cost** — full build under ₹1,500

---

## Results

| Feature | Status |
|---|---|
| Motion detection & image capture | ✅ Working |
| Telegram notification with photo | ✅ Working |
| Live browser video stream | ✅ Working |
| Deep sleep + wake-up cycle | ✅ Working |

---

## Future Enhancements

- [ ] Face recognition using ESP-WHO framework
- [ ] Object classification with TinyML on-device
- [ ] Two-way audio communication
- [ ] Solar-powered operation for outdoor deployment
- [ ] GSM / LoRa fallback for areas without Wi-Fi

---

## References

- [Espressif ESP32-CAM Datasheet](https://www.espressif.com)
- [HC-SR501 PIR Sensor Documentation](https://www.datasheet4u.com)
- [Arduino Core for ESP32](https://github.com/espressif/arduino-esp32)
- [ESP32 Camera GitHub Repository](https://github.com/espressif/esp32-camera)
- [Telegram Bot API Documentation](https://core.telegram.org/bots/api)

---

## License

MIT — free to use and adapt with attribution.
