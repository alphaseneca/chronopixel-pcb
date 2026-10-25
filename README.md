# ChronoPixel PCB 🔌📟

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![KiCad](https://img.shields.io/badge/KiCad-PCB%20Design-blue.svg)](https://kicad.org)
[![Version](https://img.shields.io/badge/Release-v0.1.0-orange.svg)](kiforge/)

Hardware repository for **ChronoPixel**, containing the KiCad schematics, PCB layout, manufacturing outputs, and assembly files.

---

## 📸 3D Board Previews

<div align="center">
  <img src="kiforge/chronopixel-pcb_v0.1.0_3d_front.png" alt="ChronoPixel PCB Front 3D Render" width="48%" />
  <img src="kiforge/chronopixel-pcb_v0.1.0_3d_back.png" alt="ChronoPixel PCB Back 3D Render" width="48%" />
</div>

---

## ⚡ Hardware Architecture & Key Components

| Subsystem | Component | Package / Footprint | Description / Role |
| :--- | :--- | :--- | :--- |
| **Microcontroller** | **ESP-12F** | SMD Module | ESP8266 Wi-Fi SoC with on-board PCB antenna |
| **Display Drivers** | **2× MAX7219EWG** | SOIC-24 | Serially interfaced LED matrix drivers |
| **LED Matrices** | **2× SZ421288N/64** | Through-hole | Red/Amber/Green 8×8 LED dot matrix units |
| **Motion Sensor** | **BMI323** | LGA-14 | Bosch 6-axis IMU (accelerometer + gyroscope) via I2C |
| **Power-Path / Charger** | **BQ24075RGTR** | QFN-16 | Dynamic power-path management & Li-Ion battery charger |
| **Voltage Regulator** | **TLV73333PDBVR** | SOT-23-5 | High-efficiency 3.3V 300mA low-dropout regulator |
| **USB Interface** | **USB Type-C (16-pin)** | SMD / Hybrid | 5V DC power & battery charging (with CC pulldowns) |
| **Battery Connector** | **SM02B-SRSS-TB** | SMD 1.0mm pitch | 2-pin JST connector for 3.7V Li-Po / Li-Ion battery |
| **Audio Feedback** | **MLT-7525** | SMD 7.5×7.5mm | Active/passive magnetic buzzer for chimes and alarms |
| **Programming Header** | **BM06B-SRSS-TB** | SMD 1.0mm pitch | 6-pin programming interface (UART, RST, GPIO0, 3V3, GND) |
| **Control Switch** | **B3U-1000P** | Ultra-small SMD | Hardware input / wake tactile switch |

---

## 🔋 Power Architecture

- **Dual Power Support**: Runs seamlessly off USB Type-C or a 3.7V rechargeable Li-Ion / Li-Po cell.
- **Dynamic Power-Path Management (TI BQ24075)**: 
  - When USB is connected, the system loads are powered directly from the input while simultaneously charging the battery.
  - Automatically supplements system current from the battery if the input current limit is exceeded.
  - Instantaneous switchover to battery power when USB is disconnected.
- **Clean 3.3V Rail**: Powered by a low-noise TI TLV73333 LDO to ensure reliable operation for both the ESP8266 and the sensitive BMI323 IMU.

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