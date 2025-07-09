# 🔋 Swappable Battery Management System (BMS) for EVs

Final Year Project - Department of Electronic and Telecommunication Engineering  
**University of Moratuwa**  
📅 June 2025

---

## 📸 Project Preview

<img width="927" alt="Screenshot 2025-07-09 at 10 48 01 AM" src="https://github.com/user-attachments/assets/a1637e3c-0578-4b68-9e60-798c0a6bd3f3" />


---

## 📑 Table of Contents

- [Overview](#overview)
- [Motivation](#motivation)
- [Key Features](#key-features)
- [Methodology](#methodology)
  - [Battery Pack BMS](#1-battery-pack-bms)
  - [Current Limiting Circuit](#2-current-limiting-circuit)
  - [Central Unit](#3-central-unit)
  - [Battery Swapping Station](#4-battery-swapping-station)
  - [Firmware](#5-firmware)
  - [Desktop UI](#6-desktop-ui)
- [Tools & Technologies](#tools--technologies)
- [Gallery](#gallery)
- [Team](#team)
- [License](#license)

---

## 🧠 Overview

We designed a modular, **swappable Battery Management System (BMS)** to reduce EV charging downtime.  
This BMS integrates into a **battery swapping ecosystem** consisting of:

- Smart BMS for individual packs
- Vehicle Central Control Unit
- Battery Docking Station with authentication
- Real-time monitoring via a PC interface

---

## 🚗 Motivation

The long charging times in Electric Vehicles (EVs) cause significant downtime, especially for public transportation (like electric tuk-tuks). This project eliminates this by introducing a **swappable battery system** backed by real-time monitoring, secure communication, and intelligent safety controls.

---

## ✨ Key Features

- ✅ **Active Cell Balancing** (ETA3000 IC)
- 🔒 **SHA-256 Authentication**
- ⚡ **20A Current Limiting Circuit**
- 🔁 **Battery Swapping via RFID**
- 🔧 **PC Interface for Monitoring & Configuration**
- 📡 **CAN Bus Communication**
- 💾 **EEPROM Logging**

---

## 🔧 Methodology

### 1. Battery Pack BMS

Each battery has an embedded smart BMS performing:

- **Voltage, temperature, and current monitoring**
  - Monitoring IC: `BQ79656-Q1`
  - Microcontroller: `STM32F103C8T6`
  - EEPROM for configuration and logs

- **Active Cell Balancing**
  - Inductive balancing via 15x ETA3000 IC circuits
  - Efficient energy redistribution, minimal heat loss

- **Security & Authentication**
  - HMAC-SHA256 authentication with the Central Unit
  - Unique ID and key stored in EEPROM

- **Protections**
  - Over/Under Voltage
  - Over Current
  - Over Temperature
  - High Voltage Imbalance
  - Low SOC / SOH

> ⚙️ Each BMS has a contactor circuit; no discharging allowed outside authorized use.

---

### 2. Current Limiting Circuit

Implemented a **20A current limit** for safe parallel charging:

- **Architecture**:
  - Buck converter topology
  - PWM control via TL494 (v1) / ATTiny85 (v2)
  - Current sensing via INA240
  - Gate control with UCC27511 gate drivers

- **Design Improvements (v2)**:
  - Opto-isolation
  - Dual gate drivers
  - TL071 op-amp voltage buffers
    
<img width="1245" alt="Screenshot 2024-07-05 at 2 38 18 PM" src="https://github.com/user-attachments/assets/9ef19864-b9a1-4bfb-a563-67f42b6af457" />

---

### 3. Central Unit

This unit sits in the **vehicle or docking station**, handling:

- **BMS Authentication via CAN**
- **Data Relay** (SOC, SOH, Temperature) to dashboard
- **Board Versions**:
  - v1: STM32F446 Nucleo Board
  - v2: Custom PCB with STM32F446RE

<img width="1280" alt="Screenshot 2024-07-05 at 12 37 09 PM" src="https://github.com/user-attachments/assets/237ef0db-ac30-4bcc-a3cb-b25a5b5b9f40" />

---

### 4. Battery Swapping Station

Built to support **10 battery slots** using:

- **RFID User Authentication**
- **ESP32 Adapter Board**:
  - Sends REST API requests
  - Verifies user credits in cloud database

- **Battery Interface Units** per slot:
  - Controls solenoid locks
  - Manages slot status (LEDs)
  - Reads battery status via CAN

> Only authenticated, healthy batteries are locked and charged.

<img width="1681" alt="Screenshot 2025-07-09 at 10 36 41 AM" src="https://github.com/user-attachments/assets/d38b36d0-93f4-4d5f-88eb-b9a0a07785ef" />


---

### 5. Firmware

Custom STM32 firmware handles:

- 🔁 **State Machine** for Charging/Discharging
- 🛠️ **Custom CAN Protocol** with headers and float data frames
- 🔒 **SHA-256 Authentication** using TinyCrypt
- 📦 **EEPROM Storage**:
  - Config params
  - 8 recent user logs
- ⚠️ **Warning System**:
  - Sent via CAN for any threshold violation

> IDE: STM32CubeIDE  
> Programmer: ST-Link V2

---

### 6. Desktop UI

A **PyQt5 Windows Application** enables:

- Real-time monitoring of:
  - Cell voltages
  - Temperatures
  - SOC, SOH
- Editing:
  - Warning thresholds (voltage, temp, SOC)
- Display:
  - Device info and recent usage logs

![User interface - windows app](https://github.com/user-attachments/assets/6e71ac6d-c7dd-4f54-8b94-95510bbf0446)

---

## 🛠️ Tools & Technologies

| Category             | Tools & Components                |
|----------------------|-----------------------------------|
| PCB Design           | Altium Designer                   |
| Simulation           | LTSpice, PSpice for TI            |
| Microcontrollers     | STM32F103, STM32F446, ESP32       |
| Programming Tools    | STM32CubeIDE, ST-Link V2          |
| Software Interface   | PyQt5, Qt Designer                |
| Communication        | CAN Bus, RS485, REST API          |
| Authentication       | TinyCrypt, SHA-256                |

---

## 📸 Gallery

- Monitoring PCB
  
  ![3D-view](https://github.com/user-attachments/assets/9803ad84-c9ac-4a58-b047-13cf12760359)

- Central Unit PCB
  
  ![Central Unit PCB](https://github.com/user-attachments/assets/d4334923-9a7a-41f0-954b-15ac86f8a10e)

- Docking Station

  ![Docking station-Adapter Board PCB](https://github.com/user-attachments/assets/24b17df7-cd22-4b36-8bcd-923bb1e2f453)

- Balancing Circuit

  ![Active cell balancer](https://github.com/user-attachments/assets/f7779ea8-abd8-40e6-a1ef-e7bdf4a8155a)

  
- Current Limiter Board

![3D top version 2](https://github.com/user-attachments/assets/a1ab82fc-b685-49ad-9ee5-c0a3fad9be00)
![3D bottom version 2](https://github.com/user-attachments/assets/26307f46-28d1-4a17-b021-d6f377336888)


---

## 👨‍💻 Team

**Group 21 – Final Year Undergraduate Project**  
Department of Electronic and Telecommunication Engineering  
University of Moratuwa

- H.M.S.D. Bandara – 200064C  
- A.D.U. Dilhara – 200128D  
- N.K. Fernandopulle – 200172F  
- R.D.H.C. Weerasingha – 200699C  

**Supervisors**  
- Dr. Suboda Charles  
- Mr. Chamod Madushanka

## Final Demostration Day

![IMG_6048](https://github.com/user-attachments/assets/8e33415e-4997-4f2e-a1b4-ac1de51bad54)

![IMG_6233](https://github.com/user-attachments/assets/7983fad6-ccfe-46fb-8e67-8b29f3dcb953)

![IMG_6259](https://github.com/user-attachments/assets/b432021c-3b2e-4f2c-b8a8-1e62f8da215c)



