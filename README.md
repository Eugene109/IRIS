<img alt="IRIS" src="./docs/logo/logo.png" height="100px">

> A low-cost quadcopter platform designed for autonomous swarm operations.

[![License](https://img.shields.io/badge/License-GNU-blue.svg)](LICENSE)

![Photorealistic render from the back-right-top side of the drone](./docs/images/pr_render6.png)

## Overview
IRIS is centered around a mass-manufacturable low-cost PCB, containing a flight controller and 4 ESCs. Designed with the idea of delivering Skittles autonomously, IRIS is built from the ground-up for autonomous operation in swarms. With a rich sensor set and high degree of customizability, IRIS represents an accessible, low-cost entry into the world of autonomous and swarm drone design.
![Photorealistic render of the front side of the PCB](./docs/images/pr_render4.png)

## Features
 - STM32H743 MCU
 - IMU, Barometer, Magnetometer, GPS
 - Integrated 4-in-1 ESC with sensorless drive
 - OV2640 camera for computer vision
 - 2.4GHz Radio Transciever + nanoELRS header
 - 2S LiPo input, XT60 connector
 - 3D-printed frame

## Table of Contents
- [Design Features](#design-features)
  - [Flight Controller Design](#flight-controller-design)
  - [ESC Design](#esc-design)
  - [Frame Design](#frame-design)
- [Getting Started](#getting-started)
- [PCB Assembly](#pcb-assembly)
- [Bill of Materials](#bill-of-materials)
- [Drone Assembly](#drone-assembly)
- [Firmware](#firmware)
- [Roadmap](#roadmap)

## Design Features

### Flight Controller Design
<p align="center">
  <img alt="Annotated render of the front of the PCB" src="./docs/images/annotated_front.png" width="45%">
&nbsp; &nbsp; &nbsp; &nbsp;
  <img alt="Annotated render of the back of the PCB" src="./docs/images/annotated_back.png" width="45%">
</p>

[Schematic](./pcb/schematics/Flight_Controller_Schematic.svg)

The PCB design features a 4-layer PCB stackup for lowered manufacturing costs, with a (nearly) continuous ground plane for reduced RF interference. The area under the MCU and core sensors also feature a continuous 3.3V power plane. The high voltage and current for the ESCs are routed on large copper pours on the bottom layer, with cross-layer connections connected by suture vias.

The total board dimensions are 3.650" by 2.856", a little larger than a credit card.

The main flight controller MCU is programmable over USB. The 4 ESC MCUs require a serial programmer, with ground, rx and tx exposed on header pins. All MCUs have a dedicated switch for a physical boot selector.

### ESC Design

[Schematic](./pcb/schematics/ESC_Schematics/Motor1_ESC_Schematic.svg)

The ESC design features an STM32F303K8T6 microcontroller, a very powerful MCU that enables accurate back-emf drive even for high kV motors. 

Loosely based off of this open-source design I found,
https://electronoobs.com/eng_arduino_tut91.php, the wiring diagram was very helpful to guide the general layout of my ESC. The control logic is loosely based off of the arduino code, though it had to be completely rewritten to interface with STM32 hardware (really all BLDC motor control logic is the same, but nice understandable code was helpful).

### Frame Design
The frame design merges swooping curves and sharp geometric angles in a retrofuturistic visual style. Inspired by the Theme Building at LAX, the frame arms are each constructed with 3 catenary curves, with the bottom arches merging together to form the cradle for the battery.
![Photorealistic render from the back-left side of the drone](./docs/images/pr_render5.png)

At the top of the frame, a little cradle with a minimalist IRIS logo debossed reserves space for a small collection of Skittles, easily dumped out by flipping the drone.

![CAD render of the integrated frame + PCB](docs/images/CAD_render.png)

The drone frame is constructed in two pieces and held together by 8 M2 screws. The bottom half contains a battery mount and 4 9x9mm motor mounts, while the top half clamps the PCB down. The top cover shields the bulk of the PCB while leaving the high-current ESC MOSFET sections exposed for cooling, and is bolted down using 8 10mm long M2 screws and heat set inserts on the bottom half.

## Getting Started

### Prerequisites

- EasyEDA Pro (recommended)
- KiCAD 10.0 (broken DRC rules)
- Fusion 360
- Blender 5.0+ ([pcb2blender](https://github.com/30350n/pcb2blender) extension required for textures)

### For KiCAD Users:
This project was made in the free software EasyEDA Pro, and the design is native to that software. The PCB design has been converted to a KiCAD project for easier access to the design, but there may be some errors that have occurred during the import process, in particular trace clearances and zone fill rules.
> NOTE: The KiCAD imported project relies on 3D models and footprints from LCSC, which should be imported using [easyeda2kicad.py](https://github.com/uPesy/easyeda2kicad.py) as follows:
> `easyeda2kicad --full --lcsc_id C19702 C29266 C53084459 C602037 C23630 C100042 C19666 C95841 C86295 C7171 C1644 C106245 C47023104 C98732 C2856805 C52016392 C76891 C7427089 C6807998 C784395 C19268133 C98220 C2907028 C2906920 C163475 C60491 C106235 C105871 C2907044 C2909315 C53447012 C628051 C1850418 C83291 C478483 C7421519 C118318 C2071056 C2054944 C481371 C90770 C49446790 C2892669 C1985532 C2965508 C114409 C7431054 C19273151 C709357 --output `**`<full path to pcb/kicad folder>`**`\libs\lcsc_import_lib --project-relative --overwrite `

### Repository Structure
All of the files for the flight controller + ESC board are contained in `pcb/`, with fabrication files in `pcb/fabrication/`. All fabrication files are to [JLCPCB](https://jlcpcb.com/) specifications. All part numbers are from [LCSC](https://www.lcsc.com/).

The frame CAD files can be found in the `frame/` directory, with (metric) STL and 3MF exports ready for 3D printing. The cover and bottom were designed in the same Fusion360 Part, and the IRIS_Assembly.f3z is the total assembly including parts I did not design.

All of the renders on the documentation were created using Blender 5.1, and render files can be found in `docs/render/`.

## PCB Assembly

### PCB ComponentBill of Materials
[Full BOM](./pcb/fabrication/BOM.md)

Total cost from LCSC: **$103.21**
[LCSC Quote](./pcb/fabrication/LCSC_BOM_export.xls)

### Assembly

The [PCB gerbers](./pcb/fabrication/gerbers.zip) can be manufactured by JLCPCB with the simple 4-layer PCBA economical service. Two-sided automated assembly is quite expensive however, so first prototypes will be assembled manually using solder stencils. PCB components can be ordered from LCSC using the [LCSC BOM](./pcb/fabrication/BOM_BOard1_PCB1.xlsx).
> NOTE: The large copper pours are connected directly to a lot of the pads, manual soldering will require the use of a hot air rework station and powerful soldering iron

## Bill of Materials

| Part | Description | Link | Price |
| --- | ----------- | ------------ | ----- |
| FC+ESC | 4-layer PCB A Economic, HASL(leaded) finish, 2 stencils w/ framework | [JLCPCB](./pcb/fabrication/jlcpcb_quote.png) | 29.23 |
| FC+ESC | IRIS PCB Components | [LCSC](./pcb/fabrication/LCSC_BOM_export.xls) | 103.21 |
| Frame | IRIS Frame | [3D Print](./frame/3mf) | - |
| Battery | 1000 mAh 2S LiPo with XT60 connector| [Admiral](https://motionrc.com/products/admiral-1000mah-2s-7-4v-30c-lipo-battery-with-xt60-connector-epr10002x6) | 9.99 |
| Motors | 4x 8000 kV 1103 Brushless DC | [AliExpress](https://www.aliexpress.us/item/3256808118156099.html) | 23.99 |
| Propellers | Gemfan 2512 2.5Inch 3-Blade 1.5mm Hole Propeller - 4CW+4CCW | [Pyrodrone](https://pyrodrone.com/products/gemfan-2512-2-5inch-3-blade-1-5mm-2mm-hole-propeller-4cw-4ccw) | 4.99 |
| Camera | OV2640 with SCCB cable | [Arducam](https://www.arducam.com/ov2640-2-0-mp-mega-pixels-1-4-cmos-image-sensor-sccb-interface-camera-module.html) | 6.99 |
| Heat-set inserts | M2x2.5mm, 3.5mm OD Knurled Brass Threaded Heat Set Inserts | [Rusty Bolt Shop](https://ebay.us/m/7A3cL3) | 1.70 |
| Frame screws | M2x10mm screws | [The Rusty Bolt Shop](https://ebay.us/m/wMPMiQ) | 2.02 |
| Motor mount screws | M2x8mm screws | [The Rusty Bolt Shop](https://ebay.us/m/UnEsMj) | 2.56 |
| Vibration dampening foam | 3M Double Coated Urethane Foam Tape 4056 | [Newegg](https://www.newegg.com/p/2VW-0006-00155?item=9SIA5D52U89288) | 14.99 |

Total: $199.67
(minus shipping)

## Drone Assembly
1. Print the frame in two sections
2. Attach heat-set inserts with a soldering iron on the base frame
3. Cut 4056 foam to shape and attach to the PCB contact areas on both sides of the frame 
4. Insert the assembled PCB between the two frame sections and bolt down all 8 M2x10mm screws
5. Insert the battery into the cradle underneath the PCB, fasten with zip ties across horizontal beams
6. Attach brushless motors with M2x8mm screws
7. Press-fit 2CCW and 2CW propellers onto the four brushless motors' 1.5mm shafts
8. Flash the firmware using a USB-C cable and a serial programmer
9. Fly!

## Firmware
### Betaflight
This flight controller is capable of running Betaflight, and the configuration header for a Betaflight target can be found in `firmware/betaflight/`. The compiled binaries can be found in the same directory, and the `.hex` file is recommended. The source code can be found at the [Betaflight firmware repository](https://github.com/betaflight/betaflight), and the `config.h` file can be copied into the config directory to build it yourself.

The flight control software can be flashed using the USB-C port on the aft of the drone. Simply plug it in and use STMCubeProgrammer to flash it!

### Zephyrus
The ESCs run custom firmware, named Zephyrus. The binaries and source code is available at its [repository](https://github.com/Eugene109/zephyrus).

Zephyrus is currently capable of sensorless back-EMF drive, and runs on the STM32F303K8T6 MCU platform. The sensorless drive is powered by ADC zero-crossing detection synchronized with the PWM output to the MOSFET drivers. The choice to use software detection of zero-crossing over hardware comparators was for the increased flexibility as the firmware matures, enabling more interesting control systems than just zero-crossing alone.

The ESCs are a bit harder to flash, though they should require only a single flashing when the firmware is tuned. Connect a serial programmer (FTDI, hijack an Arduino, etc) to the RX, TX, and GND pins and flash the MCU using the ROM bootloader. To switch the MCU to the built-in bootloader to program, simply flip the boot selector switch and press the reset button.

## Roadmap

- [X] PCB designed
- [X] Case designed
- [ ] PCB assembled
- [ ] Prototype assembled
- [ ] Prototype tested
- [ ] Firmware finalized
- [ ] PCB mass-produced

## License

Everything is [GNU GPL-3.0](LICENSE)

zephyrus firmware is licensed under MIT, check its [repo](https://github.com/Eugene109/zephyrus) for more information.

### Attribution
1103 BLDC motor 3D model used in Fusion Assembly is from [GrabCAD](https://grabcad.com/library/geprc-gr1103-1) by [Dang Ngoc Duy](https://grabcad.com/dang.ngoc.duy-1)

2.5" propeller 3D model used in Fusion Assembly is from [GrabCAD](https://www.thingiverse.com/thing:6901068) by [Buddhas_Priest](https://www.thingiverse.com/Buddhas_Priest)

rostock_laage_airport_4k.exr is CC0 from PolyHaven, credit to Greg Zaal

Inspiration was taken from [Electronoobs's ESC design], informing the framework for a standard ESC architecture.
