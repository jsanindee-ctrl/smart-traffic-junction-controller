# smart-traffic-junction-controller


The Smart Traffic Junction Controller is an embedded systems project that implements an automated four-way traffic light control system using an Arduino Uno based on the ATmega328P microcontroller.

The system is programmed entirely in AVR Assembly Language, demonstrating direct control of microcontroller registers, GPIO ports, hardware timers, external interrupts, and Analog-to-Digital Conversion (ADC).

The controller manages traffic flow across four road approaches: North, South, East, and West. It also provides pedestrian crossing functionality and automatically changes its operation according to ambient light conditions using an LDR sensor.

The project implements three main operating modes:

Normal Traffic Mode
Pedestrian Crossing Mode
Automatic Night Mode

### 1.Project Objectives

The main objectives of this project are:

Implement multi-port digital I/O control for traffic lights and the countdown display.

Generate accurate timing using a hardware timer and a table-driven traffic phase sequence.

Handle pedestrian crossing requests using an external interrupt.

Implement a deferred, single-crossing pedestrian request mechanism.

Read ambient light intensity using an LDR sensor and the microcontroller ADC.

Automatically switch between Normal Mode and Night Mode.

Display a visible countdown from 5 to 0 during pedestrian crossing.

Design, build, test, and document an embedded control system with register-level programming.
