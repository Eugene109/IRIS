# IRIS
Independent Robotic Import System
![Render of the front side of the PCB](./images/PCB_Front_RENDER.png)
IRIS is centered around a mass-manufacturable low-cost PCB, containing a flight controller and 4 ESCs. Designed with the idea of delivering Skittles autonomously, IRIS is built from the ground-up for autonomous operation in swarms. With a rich sensor set and high degree of customizability, IRIS represents an accessible, low-cost entry into the world of autonomous and swarm drone design.

## Highlights
 - STM32H743 MCU clocked at 480 MHz
 - 2MB Flash + 1MB RAM
 - IMU, Barometer, Compass, GPS
 - OV2640 Camera connector
 - 2.4GHz Radio Transciever
 - nanoELRS header
 - MOSFET ESCs capable of sensorless drive
 - XT60 battery connector
 - 2S to 3S battery voltages

## PCB Design
The PCB design features a 4-layer PCB stackup for lowered manufacturing costs, with a (nearly) continuous ground plane for reduced RF interference. The area under the MCU and core sensors also feature a continuous 3.3V power plane. The high voltage and current for the ESCs are routed on large copper pours on the bottom layer, with cross-layer connections connected by suture vias.
![Render of the back side of the PCB](./images/PCB_Back_RENDER.png)
The total board dimensions are 3.650" by 2.856", a little larger than a credit card.

Power is routed on 3 different voltages: unregulated 7.4V nominal from the battery to the ESCs, regulated 5V for the radio and camera, and 3.3V stepped down from the 5V for IC logic. 5V power can be switched from the battery to USB to turn off the board or to enable flashing.

The main flight controller MCU is programmable over USB. The 4 ESC MCUs require a serial programmer, with ground, rx and tx exposed on header pins. All MCUs have a dedicated switch for a physical boot selector.

## 3D Printed Frame
The IRIS board is highly versatile, compatible with many motor and propeller types, and users are encouraged to design custom housings and experiment with varying batteries and motors. For new drone enthusiasts, IRIS recommends a 2S setup with 1505 motors.
This beginner setup pairs nicely with the minimalist frame to provide a simple low-cost entry into autonomous drones.