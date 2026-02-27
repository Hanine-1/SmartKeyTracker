# ESP32-SmartKeyTracker
A smart key tracker that ensures you never leave home without your keys, using an ESP32 to deliver real time speaker alerts and phone notifications, with weekly statistics on your usage pattern.

## 1. Project Overview
SmartKeyTracker is an intelligent behavioral assistant designed to eliminate "Key Forgetfulness." Unlike a standard alarm, this system uses a Deterministic State Machine to analyze the context of door motion and key presence. It provides real-time feedback (reminders) and logs behavioral patterns to a cloud database for weekly habit analysis.

  
## 2. Problem Statement

Human error leads to two common scenarios:

 1. Leaving: Forgetting keys in the house when exiting.
 2. Arriving: Losing keys inside the house by not returning them to a designated holder.


## 3. System Objectives

The system:

- Detect whether keys are in their designated holder and alerts their absense and present
- Trigger an audible alert within 2 seconds of door opening detection
- Send a phone notification if keys are not detected
- Log timestamped key presence events
- Generate weekly usage statistics


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

## 8. Future Extensions

- Mobile app integration
- OTA firmware updates
- Multi-user support
- Low-power deep sleep optimization


## 9. Product Requirements Document (PRD)

### 9.1 Functional Requirements

| ID | Requirement | Description |
| :--- | :--- | :--- |
| **REQ-01** | **Exit Monitoring** | If `DOOR_OPEN` while `KEY_PRESENT`, trigger a "Take Keys" reminder after $X$ seconds. |
| **REQ-02** | **Entry Monitoring** | If `DOOR_OPEN` while `KEY_ABSENT`, trigger a "Hang Keys" reminder after $Y$ seconds. |
| **REQ-03** | **Data Logging** | Every event must be time-stamped and sent to the ESP32 log for behavior tracking. |
| **REQ-04** | **Non-Blocking** | The system logic must remain active during WiFi transmission (no `delay()` usage). |

### 9.2 User Logic Scenarios (with Memory)


| Scenario | Last Key State | Current Door state | Current Key state | System Action | Goal |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Exiting** | Present | Open | Present | Wait $\rightarrow$ Alarm | User forgot keys while leaving. |
| **Entering** | Absent | Open | Absent | Wait $\rightarrow$ Alarm | User arrived but hasn't hung keys. |
| **Returning**| Absent | Open | Present | Clear Timer / Log | Successful "Return to Base" habit. |
| **Departing**| Present | Open | Absent | Neutral / Log | Successful "Took Keys" habit. |

### 9.3 System Interface (I/O)

| Signal Name | Sensor Type | Logic / Type | Purpose |
| :--- | :--- | :--- | :--- |
| **DOOR_SNS** | Magnetic Reed | Digital (High/Low) | Detects if the door is open or closed. |
| **KEY_PRES** | FSR (Force) | Analog (ADC) | Detects weight of keys in the holder. |
| **ALARM_BUZ** | Piezo Buzzer | PWM (Output) | Audible frequency-based reminders. |
| **WIFI_LOG** | ESP32 Internal | TCP/UDP (Data) | Transmission of event statistics to cloud. |
