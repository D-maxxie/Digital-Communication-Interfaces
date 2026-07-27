# SPI Master State Machine – Verilog HDL

## Overview

This project implements a simple **SPI (Serial Peripheral Interface) Master Transmitter** using a finite state machine (FSM) in Verilog HDL. The design serially transmits a 16-bit data word over the SPI interface by controlling the chip select, serial clock, and data output signals.

The module is intended as a learning-oriented implementation of the SPI protocol and demonstrates how an FSM can be used to manage serial communication between a master device and an SPI slave.

---

## Features

* 16-bit serial data transmission
* FSM-based SPI controller
* Active-low chip select (`CS`)
* SPI clock generation
* Serial data output (MOSI)
* Transmission progress tracking using a counter
* Simple and synthesizable RTL design

---

## Module Description

### Inputs

| Signal   |   Width | Description            |
| -------- | ------: | ---------------------- |
| `clk`    |   1 bit | System clock           |
| `reset`  |   1 bit | Asynchronous reset     |
| `datain` | 16 bits | Data to be transmitted |

### Outputs

| Signal     |  Width | Description                |
| ---------- | -----: | -------------------------- |
| `spi_cs_l` |  1 bit | Active-low chip select     |
| `spi_sclk` |  1 bit | SPI serial clock           |
| `spi_data` |  1 bit | Serial data output (MOSI)  |
| `counter`  | 5 bits | Remaining bits to transmit |

---

## Working Principle

The SPI controller operates as a simple finite state machine with three primary states.

### State 0 – Idle

* Chip select remains inactive (`CS = 1`)
* SPI clock is low
* Waits before starting a new transmission

### State 1 – Load Data

* Chip select is asserted (`CS = 0`)
* The next data bit is placed on the MOSI line
* Transmission counter is decremented

### State 2 – Clock Pulse

* SPI clock is driven high
* The connected slave samples the transmitted bit
* If more bits remain, the FSM returns to the load state
* Otherwise, the transmission completes and the FSM returns to the idle state

---

## State Transition

```text id="9uw5v5"
          +--------+
          | Idle   |
          +--------+
               |
               ▼
        +-------------+
        | Load Bit    |
        +-------------+
               |
               ▼
        +-------------+
        | Clock High  |
        +-------------+
          |         |
          | More    | Done
          ▼         ▼
     Load Bit     Idle
```

---

## Transmission Sequence

1. Reset initializes the controller.
2. Chip select is asserted low.
3. The most significant data bit is placed on the MOSI line.
4. The SPI clock toggles high, allowing the slave to sample the bit.
5. The process repeats until all 16 bits have been transmitted.
6. Chip select is deasserted and the controller returns to the idle state.

---

## Applications

* SPI master controller
* FPGA communication with SPI peripherals
* Interfacing sensors and ADCs
* EEPROM and Flash memory communication
* Display driver interfaces
* Educational SPI protocol implementation

---

## Learning Objective

This project demonstrates how a finite state machine can be used to implement the basic operation of an SPI master transmitter. It introduces the concepts of serial data shifting, chip select control, clock generation, and sequential state transitions, providing a practical foundation for understanding SPI communication in FPGA and digital system design.

> **Note:** This implementation focuses on transmitting 16-bit data and is intended for educational purposes. A complete SPI master would typically include configurable clock polarity (CPOL), clock phase (CPHA), variable word lengths, programmable clock division, and support for receiving data on the MISO line.
