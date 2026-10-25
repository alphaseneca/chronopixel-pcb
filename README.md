# ChronoPixel PCB ⏳🔌

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![KiCad](https://img.shields.io/badge/KiCad-PCB%20Design-blue.svg)](https://kicad.org)
[![Version](https://img.shields.io/badge/Release-v0.1.0-orange.svg)](kiforge/)

Hardware design repository for **ChronoPixel** — an open-source, gravity-sensing digital sand timer and smart pixel hourglass. Contains the KiCad schematics, PCB layout, manufacturing outputs, and assembly files.

---

## 📸 3D Board Previews

<div align="center">
  <img src="kiforge/chronopixel-pcb_v0.1.0_3d_front.png" alt="ChronoPixel PCB Front 3D Render" width="48%" />
  <img src="kiforge/chronopixel-pcb_v0.1.0_3d_back.png" alt="ChronoPixel PCB Back 3D Render" width="48%" />
</div>

---

## ⏳ How the Hardware Powers the Digital Sand Timer

The PCB is architected specifically around the physical interactions of a traditional hourglass:

1. **Dual Hourglass Chambers (Dual MAX7219 + 2× 8×8 LED Matrices)**:
   The two stacked LED matrices represent the upper and lower bulbs of the hourglass. Individual pixel "sand grains" trickle through the center constriction from the top chamber to accumulate in the bottom chamber.
2. **Gravity & Tilt Tracking (Bosch BMI323 6-Axis IMU)**:
   An onboard 6-axis accelerometer/gyroscope continuously measures the gravity vector. When you turn the device upside down, tilt it, or lay it flat, the physics engine responds in real-time, causing virtual sand grains to tumble and settle naturally.
3. **True Cordless Hourglass Operation (TI BQ24075)**:
   Equipped with autonomous dynamic power-path management and integrated Li-Ion/Li-Po charging. The device can be picked up, flipped, and set down on a desk cordlessly just like a classic sand glass.
4. **Time's Up Audio Alert (MLT-7525 Buzzer)**:
   An onboard SMD buzzer sounds a chime or alarm tone the instant the final grain of sand drops.
5. **Smart Connectivity (ESP-12F Wi-Fi SoC)**:
   Enables setting timer presets (e.g. 25-minute Pomodoro, 5-minute break, tea steeping, custom countdowns) via a local web interface or mobile device.

---

## ⚡ Hardware Architecture & Key Components

| Subsystem | Component | Package / Footprint | Description / Role |
| :--- | :--- | :--- | :--- |
| **Microcontroller** | **ESP-12F** | SMD Module | ESP8266 Wi-Fi SoC with on-board PCB antenna |
| **Display Drivers** | **2× MAX7219EWG** | SOIC-24 | Serially interfaced LED matrix drivers |
| **Hourglass Chambers** | **2× SZ421288N/64** | Through-hole | Stacked 8×8 LED dot matrix modules for sand grains |
| **Motion Sensor** | **BMI323** | LGA-14 | Bosch 6-axis IMU (accelerometer + gyroscope) for gravity tracking |
| **Power-Path / Charger** | **BQ24075RGTR** | QFN-16 | Dynamic power-path management & Li-Ion battery charger |
| **Voltage Regulator** | **TLV73333PDBVR** | SOT-23-5 | High-efficiency 3.3V 300mA low-dropout regulator |
| **USB Interface** | **USB Type-C (16-pin)** | SMD / Hybrid | 5V DC power & battery charging (with CC pulldowns) |
| **Battery Connector** | **SM02B-SRSS-TB** | SMD 1.0mm pitch | 2-pin JST connector for 3.7V Li-Po / Li-Ion battery |
| **Audio Feedback** | **MLT-7525** | SMD 7.5×7.5mm | Magnetic buzzer for alarm chimes when sand runs out |
| **Programming Header** | **BM06B-SRSS-TB** | SMD 1.0mm pitch | 6-pin interface (UART, RST, GPIO0, 3V3, GND) |
| **Control Switch** | **B3U-1000P** | Ultra-small SMD | Hardware input / wake tactile switch |

---

## 🔋 Power Architecture

- **Seamless Cordless & Desktop Use**: Operates cordlessly on a rechargeable 3.7V Li-Po/Li-Ion battery or plugged in over USB Type-C.
- **Dynamic Power-Path Management (TI BQ24075)**: 
  - When USB is connected, system loads are powered directly from the USB supply while simultaneously charging the battery.
  - Automatically supplements system current from the battery if load exceeds USB current limits.
  - Instantaneous, glitch-free switchover to battery power when unseated or picked up.
- **Clean 3.3V Rail**: Powered by a low-noise TI TLV73333 LDO ensuring stable power for the ESP8266 Wi-Fi bursts and low-noise readings from the BMI323 IMU.

---

## 📁 Repository Structure

```text
chronopixel-pcb/
├── chronopixel-pcb.kicad_pro    # KiCad project file
├── chronopixel-pcb.kicad_sch    # Schematic design file
├── chronopixel-pcb.kicad_pcb    # 2-layer PCB layout
├── chronopixel-pcb.kicad_prl    # KiCad local settings
├── .kiforge.json                # KiForge export configuration
└── kiforge/                     # Automated manufacturing & inspection artifacts
    ├── chronopixel-pcb_v0.1.0_gerbers.zip   # Fabrication Gerbers & drill files
    ├── chronopixel-pcb_v0.1.0_bom.csv       # Complete bill of materials
    ├── chronopixel-pcb_v0.1.0_bom_jlc.csv   # JLCPCB formatted BOM (with LCSC C-IDs)
    ├── chronopixel-pcb_v0.1.0_cpl_jlc.csv   # JLCPCB pick-and-place (CPL/POS) file
    ├── chronopixel-pcb_v0.1.0_ibom.html     # Interactive HTML BOM for easy hand assembly
    ├── chronopixel-pcb_v0.1.0_sch.pdf       # Printable schematic PDF
    ├── chronopixel-pcb_v0.1.0.step          # Complete 3D STEP CAD model
    ├── chronopixel-pcb_v0.1.0_3d_front.png  # 3D render front
    └── chronopixel-pcb_v0.1.0_3d_back.png   # 3D render back
```

---

## 🏭 Fabrication & Assembly

All components have been selected with readily available LCSC part numbers for automated SMD assembly via JLCPCB or manual soldering:

- **Order PCB**: Submit `kiforge/chronopixel-pcb_v0.1.0_gerbers.zip` to your preferred PCB fabricator.
- **SMT Assembly**: Upload `kiforge/chronopixel-pcb_v0.1.0_bom_jlc.csv` and `kiforge/chronopixel-pcb_v0.1.0_cpl_jlc.csv`.
- **Manual Hand Assembly**: Open `kiforge/chronopixel-pcb_v0.1.0_ibom.html` in any web browser to highlight component locations and references interactively.

---

## 📜 License

This hardware project is open source and licensed under the [MIT License](LICENSE).