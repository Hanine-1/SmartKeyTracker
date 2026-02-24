# ESP32-SmartKeyTracker
A smart key tracker that ensures you never leave home without your keys, using an ESP32 to deliver real time speaker alerts and phone notifications, with weekly statistics on your usage pattern.

## 1. Overview
The system detects the presence of keys and door motion to trigger:
- Real time audible alerts via speaker
- Phone notifications via WiFi
- Weekly usage statistics tracking key-carrying behavior

The system is built around the ESP32 microcontroller.

  
## 2. Problem Statement

Forgetting essential items such as keys is a common daily issue.  
Existing solution (Rfid)

This project aims to design a local embedded solution that:
- Detects absence of keys when entering and leaving
- Provides immediate feedback
- Collects behavioral data for usage analysis


## 3. System Objectives

The system:

- Detect whether keys are in their designated holder and alerts their absense and present
- Trigger an audible alert within 2 seconds of door opening detection
- Send a phone notification if keys are not detected
- Log timestamped key presence events
- Generate weekly usage statistics


## 4. Learning Goals

- ESP32 WiFi/Bluetooth configuration
- Interrupt-based event detection
- State machine firmware design
- Power management strategies
- Modular embedded software architecture
- Data logging and statistics processing


## 5. High-Level System Architecture

### Hardware Components (Draft)

- ESP32 microcontroller
- FSR (force sensitive resistor) — passive, no power calculation needed
- Reed switch — passive, no power calculation needed
- TPA751 audio amplifier — 3.3V, 2.5mA
- Speaker
- TP4056 LiPo charger module
- LiPo battery — at least 3600mAh
- 3.3V voltage regulator

### Software Modules (Planned)

- Sensor Interface Layer
- Presence Detection Logic
- Alert Manager
- Communication Module (WiFi/BLE)
- Data Logger
- Statistics Engine


## 6. System States (Initial Concept)

- INIT
- IDLE
- KEYS_PRESENT
- KEYS_MISSING
- ALERT_ACTIVE
- DATA_LOGGING

Firmware will be implemented using a state machine architecture.

---

## 7. Open Technical Questions

- Local server vs cloud backend?
- Should statistics be computed locally or remotely?
- Battery-powered or always plugged in?
- Use MQTT or HTTP for notifications?

---

## 8. Future Extensions

- Mobile app integration
- OTA firmware updates
- Multi-user support
- Low-power deep sleep optimization
