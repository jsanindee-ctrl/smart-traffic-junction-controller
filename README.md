# smart-traffic-junction-controller

1. Project Overview

The Smart Traffic Junction Controller is an embedded systems project that implements an automated four-way traffic light control system using an Arduino Uno based on the ATmega328P microcontroller.

The system is programmed entirely in AVR Assembly Language, demonstrating direct control of microcontroller registers, GPIO ports, hardware timers, external interrupts, and Analog-to-Digital Conversion (ADC).

The controller manages traffic flow across four road approaches: North, South, East, and West. It also provides pedestrian crossing functionality and automatically changes its operation according to ambient light conditions using an LDR sensor.

The project implements three main operating modes:

Normal Traffic Mode

Pedestrian Crossing Mode

Automatic Night Mode
