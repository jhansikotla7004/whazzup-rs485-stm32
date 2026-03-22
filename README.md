📡 WhazzUp – RS485 Communication using STM32

## 🟢 Overview

This project demonstrates communication between two STM32 boards using UART extended with RS485 for long-distance and reliable data transmission.

## 🎯 Aim

To design and implement communication between two embedded systems using UART and RS485.

## 🧠 Key Concepts

* UART Communication
* RS485 Differential Signaling
* Half-Duplex Communication
* Communication Channels (A & B)
* Register-Level Programming

---

## 🔧 Hardware Components

* STM32L432KC
* SN75176 RS485 Transceiver
* Breadboard
* LED + Resistor
* Jumper wires

## 🔌 Circuit Connections

| STM32 Pin | RS485 Pin | Function     |
| --------- | --------- | ------------ |
| PA9       | DI        | Transmit     |
| PA10      | RO        | Receive      |
| PB5       | DE + RE   | Mode Control |


## 💡 LED Indicator

PB5 → Resistor → LED → GND
Used to indicate transmission mode.


## 🔁 Working Principle

1. STM32 sends data using UART
2. RS485 converts it into differential signals
3. Data travels through A & B wires
4. Receiver converts it back to UART

## 🧪 Testing

* Verified output using serial monitor
* LED used to confirm transmit mode


## 📊 Results

* Successful communication achieved
* Stable transmission observed

## ⚙️ Software

Implemented using register-level programming.

## 📽️ Demo Video

## 👨‍💻 Author

KOTLA JHANSI LAKSHMI
