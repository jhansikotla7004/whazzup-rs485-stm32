# WhazzUp – RS485 Communication using STM32

## Overview
This project demonstrates communication between two STM32 boards using UART extended with RS485. The aim of the project is to achieve reliable communication between embedded systems using differential signaling.

## Project Aim
To design and implement communication between two STM32 boards using UART and RS485.

## Objectives
- To implement UART communication on STM32
- To interface the SN75176 RS485 transceiver
- To transmit data reliably between two boards
- To control transmit and receive mode using GPIO
- To verify the system using serial monitor and LED indication

## Hardware Used
- STM32L432KC
- SN75176AP RS485 Transceiver
- Breadboard
- Jumper wires
- LED
- Resistor
- USB cable

## Circuit Connections

| STM32 Pin | SN75176 Pin | Function |
|----------|-------------|----------|
| PA9      | DI          | UART transmit to RS485 |
| PA10     | RO          | UART receive from RS485 |
| PB5      | DE and RE   | Direction control |
| +5V      | VCC         | Power supply |
| GND      | GND         | Ground |

## LED Connection
PB5 is also connected to an LED through a resistor.  
This LED gives a visual indication of transmission activity.

## RS485 Bus
- A -> RS485_A
- B -> RS485_B

These two wires form the RS485 communication channel.

## Working Principle
The STM32 uses UART to generate serial data.  
The SN75176 transceiver converts UART signals into RS485 differential signals.  
PB5 controls whether the transceiver is in transmit mode or receive mode.  
When PB5 is high, the system transmits data.  
When PB5 is low, the system returns to receive mode.

## Software Implementation
The final working software was implemented using low-level register programming.  
GPIO and USART peripherals were configured by directly accessing STM32 registers.

A HAL-based approach was explored during development, but the final verified implementation used register-level programming because it produced stable and correct operation.

## Testing and Debugging
The system was tested using:
- Serial monitor output
- LED indication
- Manual reset observation
- Verification of GPIO and USART behavior

The LED was used as an ad-hoc debugging method to confirm when the transceiver entered transmit mode.

## Results
The system successfully transmitted the message:

`Hello from Person 1`

through the RS485 channel.  
Serial monitor output confirmed that the communication logic worked correctly.

## Circuit Diagram
![Schematic](schematic.png)
## 💻 Source Code

The main program is located in:

`src/main.c`

### Key Features:
- UART communication using USART1 and USART2  
- RS485 direction control using PB5  
- Register-level programming  

## Demo Video


## Author
KOTLA JHANSI LAKSHMI
