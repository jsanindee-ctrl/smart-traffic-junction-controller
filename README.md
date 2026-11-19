# smart-traffic-junction-controller


The Smart Traffic Junction Controller is an embedded systems project that implements an automated four-way traffic light control system using an Arduino Uno based on the ATmega328P microcontroller.

The system is programmed entirely in AVR Assembly Language, demonstrating direct control of microcontroller registers, GPIO ports, hardware timers, external interrupts, and Analog-to-Digital Conversion (ADC).

The controller manages traffic flow across four road approaches: North, South, East, and West. It also provides pedestrian crossing functionality and automatically changes its operation according to ambient light conditions using an LDR sensor.

The project implements three main operating modes:

                                    Normal Traffic Mode
                                    
                                    Pedestrian Crossing Mode
                                    
                                    Automatic Night Mode

### 1.Project Objectives

* Implement multi-port digital I/O control for traffic lights and the countdown display.
* Generate accurate timing using a hardware timer and a table-driven traffic phase sequence.
* Handle pedestrian crossing requests using an external interrupt.
* Implement a deferred, single-crossing pedestrian request mechanism.
* Read ambient light intensity using an LDR sensor and the microcontroller ADC.

### 2.Operations
### Normal Traffic Mode
In Normal Mode, the traffic signals operate according to a predefined six-phase sequence, beginning with North–South Green and East–West Red, followed by North–South Yellow and East–West Red, then an All-Red clearance phase. After that, East–West Green and North–South Red are activated, followed by East–West Yellow and North–South Red, and finally another All-Red clearance phase before the sequence repeats continuously. The yellow phases warn vehicles that the signals are changing, while the all-red intervals provide a clearance period between changes in traffic direction. The traffic signal durations are 7 seconds for each green phase, 2 seconds for each yellow phase, and 2 seconds for each all-red phase, resulting in a total cycle time of 22 seconds. The phase patterns and durations are stored in a table in program memory, allowing timing values to be modified by editing the table without changing the phase-control routine.

### Pedestrian Crossing Mode and Request Handling
Pedestrian Crossing Mode allows pedestrians to request a safe road-crossing opportunity by pressing a push button. The external interrupt detects the button press and sets the F_REQ flag to record the request. The controller completes the current traffic phase and waits until the all-red clearance phase before activating pedestrian mode, during which all traffic lights remain red and a countdown from 5 to 0 is displayed. After the countdown, normal traffic operation resumes. The F_INPED flag indicates an active pedestrian crossing, while F_LOCK prevents repeated requests from triggering consecutive crossings. The F_NIGHT flag indicates Night Mode, and F_TICK indicates that a timer interval has occurred. This flag-based approach ensures that pedestrian requests are handled safely and that the interrupt only records requests while the main control routine decides when to activate the crossing.

### Automatic Night Mode
The system uses an LDR sensor and a 10 kΩ resistor connected as a voltage divider to detect changes in ambient light, with the sensor output connected to the Arduino Uno analog input A0 (ADC0). The firmware reads the light intensity through the ADC and compares the value with a predefined darkness threshold. When darkness is detected, Night Mode is activated, causing all four yellow traffic LEDs to flash together at a frequency of 1 Hz, with 0.5 seconds ON and 0.5 seconds OFF, while the red, green, and pedestrian countdown indicators remain OFF. To prevent frequent switching between operating modes due to temporary light changes, the controller uses a hysteresis mechanism. When sufficient light is detected, the system waits until the condition remains stable for five consecutive seconds before returning to Normal Mode. If darkness is detected again during this period, the recovery counter resets. Once the five-second condition is satisfied, normal traffic operation resumes from an all-red phase. During Night Mode, pedestrian button requests are ignored, and any previously pending pedestrian request is cleared to ensure stable and safe operation.

### 3.Firmware Design
The firmware is developed entirely in AVR Assembly Language, using direct register-level programming of the ATmega328P to control GPIO ports, timers, external interrupts, and ADC operations. Timer1 is configured to generate periodic interrupts every 100 milliseconds, providing accurate timing for traffic signal operations. The external interrupt INT0 detects pedestrian button presses and records crossing requests using control flags. The LDR sensor connected to ADC0 measures ambient light intensity, allowing the controller to automatically activate or deactivate Night Mode based on the detected light level. The main program follows a state-machine approach to manage traffic signal phases, pedestrian crossing requests, countdown operations, and transitions between Normal Mode and Night Mode. Interrupt service routines handle time-based and external events, while the main loop processes these events and controls the overall system operation. This approach provides a structured and efficient method of managing the different functions of the traffic junction controller.

### 4.Design Decisions
Several design approaches were selected to improve the organization, safety, and reliability of the traffic controller:
* Table-driven traffic phases: Traffic signal patterns and their durations are stored in a program memory table, allowing timing values to be modified without changing the main phase-control routine.
* Deferred pedestrian requests: Pedestrian requests are recorded and processed at a safe point in the traffic sequence rather than changing signals immediately.
* Request locking: A lock mechanism prevents repeated button presses from generating consecutive pedestrian crossings.
* Short interrupt routines: Interrupt service routines mainly update control flags, leaving the main program loop to perform traffic-control operations.
* Night Mode hysteresis: A five-second stable light condition is required before returning to Normal Mode, helping prevent frequent switching caused by temporary lighting changes.

### 5.Project Limitations
* The pedestrian request button does not use hardware debouncing, so switch noise may cause unintended requests.
* A single light threshold is used for entering and leaving Night Mode.
* LDR threshold calibration depends on the surrounding lighting conditions.
*  Phase durations are stored as single-byte values, limiting the maximum representable duration to 25.5 seconds when using 100 ms timing units.
