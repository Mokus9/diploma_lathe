# 🔧 Compact CNC Lathe for Children's Workshop

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Русский](https://img.shields.io/badge/Язык-Русский-green.svg)](README.md)
[![English](https://img.shields.io/badge/Language-English-blue.svg)](README_EN.md)
[![ESP32](https://img.shields.io/badge/Platform-ESP32-red.svg)]()
[![PETG](https://img.shields.io/badge/Material-PETG-orange.svg)]()

> An open-source compact CNC lathe for machining soft materials in children's educational workshops. All structural parts are reproducible on any standard desktop 3D printer.

**Master's Thesis** | NUST MISIS | 15.04.02 Technological Machines and Equipment | 2026

Author: Maxim Shikhov  
Supervisor: A.G. Tavitov

---

## 📸 Photos

> 📁 All photos are in [`/photos`](/photos)

| Prototype assembled | Spindle assembly | Tool milling process |
|--------------------|-----------------|---------------------|
|<img width="2560" height="1920" alt="photo_2026-06-22_22-59-06" src="https://github.com/user-attachments/assets/5d93dd02-8663-429f-baf7-cbf13ef79664" />|<img width="1920" height="2560" alt="photo_2026-06-22_22-59-10" src="https://github.com/user-attachments/assets/a5ef0366-fc95-40a4-8ced-222a79672aed" />|<img width="1920" height="2560" alt="photo_2026-06-22_22-05-56" src="https://github.com/user-attachments/assets/e3791462-7856-4793-afbd-009d055ddf96" />|

---

## 🎯 Why This Project

Lathe work — the historical foundation of engineering thinking — remains inaccessible to young children: real lathes are dangerous (high RPM, 220V mains, flying metal chips). Existing solutions split into two extremes: scaled-down "adult" machines that inherit all the dangers, and toy imitations with no real function.

This lathe fills the gap:

- ✅ **7–12V** battery power — no mains voltage in the work zone
- ✅ Machines **paraffin candles** — safe, affordable, visually engaging
- ✅ Fully **reproducible**: structural parts are 3D printed
- ✅ Open control system on **ESP32 + GRBL** — study it, modify it
- ✅ Standard **G-code** — same language as industrial CNC
- ✅ **~$45–70 total cost** — affordable for any school or maker space

---

## ⚙️ Technical Specifications

| Parameter | Value |
|-----------|-------|
| X-axis — distance between chucks | 140 mm |
| Y-axis — tool carriage travel | 40 mm |
| Max spindle speed | 915 RPM |
| Recommended working range | 400–700 RPM |
| Positioning resolution | 0.01 mm |
| Positioning accuracy | ±0.05 mm |
| Supply voltage | 7–12V DC (battery) |
| Structural material | PETG (FDM printing) |
| Linear guides | Aluminium tubes Ø10 mm |
| Linear bearings | Printed PETG sliding bearings |
| Spindle drive | DC 12V, GT2 belt drive |
| Axis control | ESP32 + TB6612FNG |
| Spindle shaft | Standard bolt with cone |
| Bearings | 608 (×2 in spindle housing) |
| Safety cover | Transparent polycarbonate 3 mm |
| Primary workpiece | Paraffin candles |

---

## 🗂️ Repository Structure

```
/
├── README.md                   # Documentation (Russian)
├── README_EN.md                # Documentation (English)
├── LICENSE                     # MIT License
│
├── cad/                        # 3D models
│   └── lathe_v1.3dm            # Full assembly (Rhino 3D)
│
├── stl/                        # Print-ready STL files
│   ├── spindle_body.stl        # Spindle housing
│   ├── front_cone.stl          # Front cone (drive chuck)
│   ├── rear_cone.stl           # Rear cone (live centre)
│   ├── carriage.stl            # Tool carriage platform
│   ├── linear_bearing.stl      # Printed linear bearing
│   ├── tool_holder.stl         # Tool holder
│   ├── frame.stl               # Frame / bed
│   └── cover_posts.stl         # Safety cover posts
│
├── firmware/                   # ESP32 firmware
│   ├── main.ino                # Main sketch (GRBL-based)
│   └── firmware_setup.md       # Flashing guide
│
├── electronics/                # Electronics
│   ├── schematic.pdf           # Circuit schematic
│   └── bom_electronics.md      # Components list
│
├── docs/                       # Documentation
│   ├── assembly.md             # Step-by-step assembly guide
│   ├── safety.md               # Safety guidelines
│   └── gcode_examples/         # Example G-code programs
│
└── photos/                     # Photos
    ├── overview/               # Assembled prototype
    ├── spindle/                # Spindle assembly details
    └── toolmaking/             # HSS tool milling process
```

---

## 🛒 Bill of Materials (BOM)

| # | Component | Qty | Est. cost |
|---|-----------|-----|-----------|
| 1 | NEMA17 Stepper Motor | 2 | ~$7 total |
| 2 | DC Motor 12V ~915 RPM | 1 | ~$5 |
| 3 | ESP32 + custom PCB | 1 | ~$4 |
| 4 | TB6612FNG driver | 1 | (on PCB) |
| 5 | PWM controller + buttons | 1 set | ~$4 |
| 6 | 7–12V Battery | 1 | ~$7 |
| 7 | Aluminium tube Ø10mm, 1m | 1 | ~$2 |
| 8 | 608 Bearings + fasteners | set | ~$4 |
| 9 | Polycarbonate 3mm (cover) | 1 sheet | ~$2 |
| 10 | GT2 belt + pulleys | 1 set | ~$3 |
| 11 | PETG filament | ~1 kg | ~$20 |

**Estimated total: ~$45–70 USD**  
**Replacement of any printed part: ~$1–2** (reprint 50–100g part)

---

## 🖨️ 3D Printing Settings

All structural parts printed in **PETG**:

| Parameter | Value |
|-----------|-------|
| Nozzle temp | 230–245 °C |
| Bed temp | 70–85 °C |
| Layer height | 0.2 mm |
| Infill | 40–60 % |
| Infill pattern | Grid / Gyroid |
| Print speed | 40–60 mm/s |
| Perimeters | 3–4 walls |
| Cooling | Moderate 20–50 % |
| Supports | Minimal |

> ⚠️ **Spindle housing** must be printed **vertically** — the bearing bore axis must align with layer deposition direction.

---

## 🔌 Control System

### Architecture — Three independent circuits

```
[G-code from PC / tablet]
          │
          ▼  Wi-Fi (no cable needed)
      [ESP32]
      │      │
      │      ▼
      │  [TB6612FNG] ──► [NEMA17 · X-axis]
      │  [TB6612FNG] ──► [NEMA17 · Y-axis]
      │
      └──► [PWM] ──► [DC Motor 12V · spindle]

[Emergency stop button] ──► cuts all power simultaneously
```

### Quick Start

```bash
# 1. Clone the repo
git clone https://github.com/m112521/mini-cnc-lathe.git

# 2. Open in Arduino IDE
# File: firmware/main.ino

# 3. Install ESP32 board support
# Boards Manager → esp32 by Espressif Systems

# 4. Select ESP32 Dev Module, choose port → Upload
```

Full guide: [`firmware/firmware_setup.md`](firmware/firmware_setup.md)

---

## 🔨 Assembly (9 steps)

Full guide: [`docs/assembly.md`](docs/assembly.md)

1. Print all parts in PETG
2. Press-fit 608 bearings into spindle housing
3. Assemble spindle: bolt → cone → GT2 pulley
4. Build the frame: insert aluminium tubes, check parallelism
5. Install lead screws and stepper motors
6. Install tailstock (live centre cone on bearing)
7. Mount tool carriage on printed linear bearings
8. Wire electronics per schematic, install polycarbonate cover
9. Flash firmware, calibrate axes, set Home position

---

## 🪚 Cutting Tool

The cutting tool was hand-made by **milling HSS (High Speed Steel)**. Photos: [`/photos/toolmaking`](/photos/toolmaking).

> For paraffin and wax, any tool with a large rake angle works — cutting forces are just a few newtons.

---

## 🛡️ Safety

Standards: **GOST 28139-89** and new **GOST R "Educational Equipment"** (effective June 2026).  
The new standard specifies **7–12V** as safe voltage for primary school equipment — this lathe complies.

| Category | Measures |
|----------|----------|
| Mechanical | ≤915 RPM · 3mm polycarbonate cover · emergency stop |
| Electrical | 7–12V DC · no 220V · no exposed contacts |
| Fire | No heaters · PETG and PC non-flammable · paraffin chips safe |
| Ergonomic | Noise ≤60 dB · visibility through cover · belt dampens vibration |

---

## 🎓 Educational Scenarios

| Level | What the child does |
|-------|---------------------|
| Beginner | Runs a ready-made program, watches the machining |
| Intermediate | Edits G-code parameters, evaluates the result |
| Advanced | Writes custom programs, explores ESP32 firmware |
| Project | Prints all parts and builds the lathe from scratch |

**Sample paraffin workpieces:** cylinders, spheres, chess pieces, miniature vases, stepped shaft, cone.

---

## 📄 License

**MIT** — free to use, copy, modify and distribute, including in educational institutions.  
See [LICENSE](LICENSE).

---

## 📬 Contact

Maxim Shikhov — NUST MISIS, 2026  
[Issues](../../issues) · [Pull Requests](../../pulls)
