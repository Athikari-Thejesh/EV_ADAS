\# Real-Time EV Monitoring \& ADAS Warning System



A modular embedded firmware project built on the \*\*STM32F103C8T6 (ARM Cortex-M3)\*\* for real-time electric-vehicle monitoring and Advanced Driver Assistance System (ADAS) warning functions.


The system combines vehicle monitoring, multi-sensor ultrasonic distance measurement, ADAS decision logic, fault handling, UART-based diagnostics, and a Python monitoring dashboard.



\## Overview



This project demonstrates the development of a structured embedded firmware application for an EV monitoring and ADAS prototype.



The firmware is divided into independent modules for sensor acquisition, vehicle monitoring, ADAS processing, fault handling, and diagnostics.



\### Main Functions



\- Real-time EV parameter monitoring

\- Three-channel ultrasonic distance measurement

\- Forward Collision Warning (FCW)

\- Blind Spot Detection (BSD)

\- Time-to-Collision (TTC) warning logic

\- Fault monitoring and handling

\- UART diagnostic shell

\- Sensor data processing

\- Modular embedded firmware architecture

\- Python-based monitoring dashboard



\---



\## System Architecture



```text

&#x20;                        STM32F103C8T6

&#x20;                         ARM Cortex-M3

&#x20;                              |

&#x20;         +--------------------+--------------------+

&#x20;         |                    |                    |

&#x20;         v                    v                    v

&#x20;  +-------------+      +-------------+      +-------------+

&#x20;  | EV Control  |      | Ultrasonic  |      |    Fault    |

&#x20;  |   Module    |      |   Module    |      |   Manager   |

&#x20;  +------+------+      +------+------+      +-------------+

&#x20;         |                    |

&#x20;         |             +------+------+------+

&#x20;         |             |      |      |      |

&#x20;         |             v      v      v      |

&#x20;         |           Front   Left   Right   |

&#x20;         |           Sensor Sensor Sensor   |

&#x20;         |             |      |      |      |

&#x20;         |             +------+------+------+

&#x20;         |                    |

&#x20;         |                    v

&#x20;         |              +-----------+

&#x20;         +------------->|    ADAS   |

&#x20;                        |   Module  |

&#x20;                        +-----+-----+

&#x20;                              |

&#x20;                   +----------+----------+

&#x20;                   |          |          |

&#x20;                   v          v          v

&#x20;                  FCW        TTC        BSD

&#x20;                   |          |          |

&#x20;                   +----------+----------+

&#x20;                              |

&#x20;                              v

&#x20;                     +----------------+

&#x20;                     |  UART Shell /  |

&#x20;                     |  Diagnostics   |

&#x20;                     +-------+--------+

&#x20;                             |

&#x20;                             v

&#x20;                    +------------------+

&#x20;                    | Python Dashboard |

&#x20;                    +------------------+


Hardware

Component	Purpose

STM32F103C8T6 Blue Pill	Main embedded controller

HC-SR04 × 3	Distance measurement

Front Ultrasonic Sensor	Forward obstacle detection

Left Ultrasonic Sensor	Left-side monitoring

Right Ultrasonic Sensor	Right-side monitoring

UART Interface	Diagnostics and communication

Software

Development Environment

STM32CubeIDE

STM32 HAL

CMSIS

Embedded C

Python

Git / GitHub

Target MCU

MCU             : STM32F103C8T6

CPU Core        : ARM Cortex-M3

Architecture    : 32-bit ARM

Programming     : Embedded C

IDE             : STM32CubeIDE

Firmware Architecture



The firmware is organized into independent modules rather than placing the complete application inside main.c.



Application Layer

&#x20;      |

&#x20;      +-------------------+

&#x20;      |                   |

&#x20;      v                   v

&#x20;EV Control            ADAS Logic

&#x20;      |                   |

&#x20;      |             +-----+-----+

&#x20;      |             |     |     |

&#x20;      |            FCW   TTC   BSD

&#x20;      |                   |

&#x20;      +---------+---------+

&#x20;                |

&#x20;                v

&#x20;         Sensor Processing

&#x20;                |

&#x20;                v

&#x20;       Ultrasonic Interface

&#x20;                |

&#x20;                v

&#x20;          STM32 Hardware



This modular architecture makes the firmware easier to understand, debug, test, and extend.



Firmware Modules

EV Control



Files:



Core/Inc/ev\_control.h

Core/Src/ev\_control.c



The EV control module handles the vehicle monitoring functionality and maintains the software representation of vehicle operating parameters.



Separating EV control from ADAS processing keeps vehicle monitoring and warning decisions independently maintainable.



ADAS Module



Files:



Core/Inc/adas.h

Core/Src/adas.c



The ADAS module processes sensor information and evaluates warning conditions.



The project contains logic for:



Forward Collision Warning (FCW)



The forward ultrasonic sensor is used to monitor the distance between the vehicle and an obstacle.



Front Sensor

&#x20;    |

&#x20;    v

Distance Measurement

&#x20;    |

&#x20;    v

ADAS Evaluation

&#x20;    |

&#x20;    v

Collision Warning



The system evaluates the measured distance and generates a warning condition when the configured criteria are satisfied.



Blind Spot Detection (BSD)



The left and right ultrasonic sensors are used for side-area monitoring.



Left Sensor  ---> Left-side monitoring

Right Sensor ---> Right-side monitoring



The ADAS module evaluates these measurements to identify objects in the monitored side regions.



Time-to-Collision (TTC)



The system also contains TTC-related warning logic for evaluating collision risk using available vehicle and distance information.



Ultrasonic Sensor Module



Files:



Core/Inc/ultrasonic.h

Core/Src/ultrasonic.c



Three ultrasonic sensors are used for monitoring the front, left, and right areas around the vehicle.



&#x20;                   FRONT

&#x20;                     |

&#x20;                     v

&#x20;              +-------------+

&#x20;              | Ultrasonic  |

&#x20;              +-------------+

&#x20;                     |

&#x20;         +-----------+-----------+

&#x20;         |                       |

&#x20;         v                       v

&#x20;  +-------------+         +-------------+

&#x20;  | Left Sensor |         |Right Sensor |

&#x20;  +-------------+         +-------------+



The firmware generates the ultrasonic trigger signal and measures the returning echo duration to determine the distance.



Distance Calculation



The basic ultrasonic distance relationship is:



Distance = Echo Time × Speed of Sound / 2



The division by two accounts for the ultrasonic pulse travelling to the obstacle and returning to the sensor.



The calculated distance is then provided to the application and ADAS layers.



Sensor Processing Flow

&#x20;       Trigger Sensor

&#x20;             |

&#x20;             v

&#x20;    Generate Ultrasonic Pulse

&#x20;             |

&#x20;             v

&#x20;      Measure Echo Time

&#x20;             |

&#x20;             v

&#x20;      Calculate Distance

&#x20;             |

&#x20;             v

&#x20;      Update Sensor Data

&#x20;             |

&#x20;             v

&#x20;        ADAS Processing

&#x20;             |

&#x20;             v

&#x20;       Warning / Status



The sensor acquisition functionality is separated from the ADAS decision logic so that the sensing and application layers remain independent.



Fault Management



Files:



Core/Inc/fault.h

Core/Src/fault.c



The project contains a dedicated fault-management module for handling system fault conditions.



Instead of distributing fault handling throughout the application, the firmware provides a separate location for fault-related processing.



Hardware / Software Event

&#x20;         |

&#x20;         v

&#x20;   Fault Detection

&#x20;         |

&#x20;         v

&#x20;    Fault Manager

&#x20;         |

&#x20;         v

&#x20;    Fault State

&#x20;         |

&#x20;         v

&#x20;Diagnostic Output



This structure makes it easier to extend the system with additional diagnostics and error conditions.



UART Diagnostic Shell



Files:



Core/Inc/uart\_shell.h

Core/Src/uart\_shell.c



A UART-based diagnostic shell is included for interacting with and monitoring the firmware during development and testing.



&#x20;      PC / Serial Terminal

&#x20;               |

&#x20;               | UART

&#x20;               v

&#x20;       +---------------+

&#x20;       |  UART Shell   |

&#x20;       +-------+-------+

&#x20;               |

&#x20;               v

&#x20;      Firmware Modules



The diagnostic interface provides a practical method for observing system behavior without relying entirely on the debugger.



Python Dashboard



File:



ev\_dashboard.py



A Python-based dashboard is included for higher-level monitoring of the embedded system.



The dashboard provides a more convenient way to observe system information compared with raw serial-terminal output.



&#x20;            STM32

&#x20;              |

&#x20;              | UART

&#x20;              v

&#x20;      +----------------+

&#x20;      | Python         |

&#x20;      | Dashboard      |

&#x20;      +----------------+

&#x20;              |

&#x20;              v

&#x20;      System Monitoring

Interrupt and Peripheral Integration



The project uses the STM32 peripheral and interrupt infrastructure.



Relevant files include:



Core/Src/stm32f1xx\_hal\_msp.c

Core/Src/stm32f1xx\_it.c



The firmware integrates multiple STM32 peripherals and embedded concepts including:



GPIO

Timers

UART

ADC-related functionality

Interrupt handling

Sensor interfaces



Peripheral configuration is maintained through the STM32CubeIDE / CubeMX project configuration.



Project Data Flow



The overall system data flow is:



&#x20;                Ultrasonic Sensors

&#x20;                 /      |      \\

&#x20;                /       |       \\

&#x20;               v        v        v

&#x20;            Front      Left     Right

&#x20;               \\         |        /

&#x20;                \\        |       /

&#x20;                 +-------+------+

&#x20;                         |

&#x20;                         v

&#x20;                 Distance Data

&#x20;                         |

&#x20;                         v

&#x20;                   ADAS Module

&#x20;                         |

&#x20;            +------------+------------+

&#x20;            |            |            |

&#x20;            v            v            v

&#x20;           FCW          TTC          BSD

&#x20;            |            |            |

&#x20;            +------------+------------+

&#x20;                         |

&#x20;                         v

&#x20;                  Warning / Status

&#x20;                         |

&#x20;                         v

&#x20;                   UART Shell

&#x20;                         |

&#x20;                         v

&#x20;                 Python Dashboard



Vehicle monitoring information is handled through the EV control module and diagnostic information is made available through the UART interface.



Project Structure

EV\_ADAS/

│

├── ev\_dash/

│   │

│   ├── Core/

│   │   ├── Inc/

│   │   │   ├── adas.h

│   │   │   ├── common.h

│   │   │   ├── ev\_control.h

│   │   │   ├── fault.h

│   │   │   ├── main.h

│   │   │   ├── stm32f1xx\_hal\_conf.h

│   │   │   ├── stm32f1xx\_it.h

│   │   │   ├── uart\_shell.h

│   │   │   └── ultrasonic.h

│   │   │

│   │   ├── Src/

│   │   │   ├── adas.c

│   │   │   ├── ev\_control.c

│   │   │   ├── fault.c

│   │   │   ├── main.c

│   │   │   ├── stm32f1xx\_hal\_msp.c

│   │   │   ├── stm32f1xx\_it.c

│   │   │   ├── syscalls.c

│   │   │   ├── sysmem.c

│   │   │   ├── system\_stm32f1xx.c

│   │   │   ├── uart\_shell.c

│   │   │   └── ultrasonic.c

│   │   │

│   │   └── Startup/

│   │

│   ├── Drivers/

│   │   ├── CMSIS/

│   │   └── STM32F1xx\_HAL\_Driver/

│   │

│   ├── STM32F103C8TX\_FLASH.ld

│   └── ev\_dash.ioc

│

├── ev\_dashboard.py

├── .gitignore

└── README.md



Build output such as the STM32CubeIDE Debug/ directory is intentionally excluded from version control.



Embedded Concepts Demonstrated



This project demonstrates practical embedded-firmware concepts including:



Embedded C programming

ARM Cortex-M firmware development

STM32 development

STM32 HAL

GPIO control

Timer-based measurement

Interrupt handling

UART communication

Ultrasonic sensor interfacing

Real-time sensor processing

Modular firmware architecture

Fault handling

Diagnostic interfaces

Sensor-to-application data flow

Hardware-software integration

Python-based embedded-system monitoring

Git-based source-code management

Testing and Validation



The system can be tested in multiple stages.



Sensor Testing



Verify:



Ultrasonic trigger generation

Echo measurement

Distance calculation

Front sensor response

Left sensor response

Right sensor response

ADAS Testing



Test different obstacle positions and distances.



No Obstacle

&#x20;    |

&#x20;    v

Normal Operation

Obstacle Detected

&#x20;    |

&#x20;    v

Distance Evaluation

&#x20;    |

&#x20;    v

ADAS Decision

&#x20;    |

&#x20;    v

Warning Condition

Fault Testing



Test expected fault and error conditions and verify that the fault-management module correctly reports the corresponding system state.



UART Testing



Verify:



UART communication

Diagnostic output

Shell interaction

Runtime system information

Dashboard Testing



Verify that the Python dashboard receives and displays the expected information from the embedded system.



Build and Run

1\. Clone the Repository

git clone https://github.com/Athikari-Thejesh/EV\_ADAS.git

2\. Open the Project



Open the following directory using STM32CubeIDE:



ev\_dash/

3\. Import the Project



The project contains its STM32CubeIDE configuration and CubeMX project file:



ev\_dash.ioc



Import/open the project in STM32CubeIDE.



4\. Build the Firmware



In STM32CubeIDE:



Project → Build Project

5\. Program the STM32



Connect the STM32F103C8T6 board using an appropriate ST-Link/debug programming interface and flash the generated firmware.



6\. Monitor the System



Connect the UART interface to a serial terminal and observe the diagnostic information generated by the firmware.



The Python dashboard can be used for higher-level system monitoring.



Engineering Approach



The project follows a layered approach:



+-----------------------------------+

|        Application Logic          |

|     EV Control + ADAS Logic      |

+-----------------------------------+

|       Sensor Processing           |

|       Ultrasonic Module           |

+-----------------------------------+

|      Diagnostics / Faults         |

|       UART Shell + Fault          |

+-----------------------------------+

|       STM32 HAL / CMSIS           |

+-----------------------------------+

|          STM32F103C8T6            |

|          ARM Cortex-M3            |

+-----------------------------------+



This separation helps keep hardware interaction, sensor processing, application logic, and diagnostics independently manageable.



Why This Project?



This project was developed to gain practical experience in embedded firmware development and hardware-software integration.



The main learning areas include:



Designing modular Embedded C firmware

Interfacing sensors with a microcontroller

Working with STM32 peripherals

Handling real-time sensor information

Implementing application-level decision logic

Building diagnostic interfaces

Debugging hardware and firmware together

Connecting embedded firmware with a host-side Python application

Limitations



This project is a prototype embedded-system implementation and is not intended for deployment in a production vehicle.



The ultrasonic sensing and ADAS functionality are intended for embedded-system development and demonstration purposes.



A production automotive ADAS system would require substantially more advanced sensing, redundancy, diagnostics, validation, cybersecurity, functional-safety processes, and automotive-grade hardware.



Future Improvements



Possible future improvements include:



CAN-based vehicle communication

More advanced sensor fusion

Additional vehicle parameters

Improved collision prediction

More robust fault diagnostics

Data logging

Hardware-in-the-loop testing

Automated firmware testing

Additional diagnostic commands

Production-oriented bootloader

Firmware update mechanism

More advanced vehicle communication interfaces

Technologies Used

Microcontroller

&#x20;   STM32F103C8T6



CPU

&#x20;   ARM Cortex-M3



Programming

&#x20;   Embedded C

&#x20;   Python



Firmware

&#x20;   STM32 HAL

&#x20;   CMSIS



Communication

&#x20;   UART



Sensors

&#x20;   HC-SR04 Ultrasonic Sensors



Development

&#x20;   STM32CubeIDE

&#x20;   STM32CubeMX



Version Control

&#x20;   Git

&#x20;   GitHub

Repository



GitHub Repository:



https://github.com/Athikari-Thejesh/EV\_ADAS

