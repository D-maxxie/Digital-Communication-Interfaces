# README.md

````markdown
# 16-bit 2:1 Multiplexer (MUX) using Verilog

## 📖 Overview

This project implements a **16-bit 2-to-1 Multiplexer (MUX)** using Verilog HDL. The design selects one of two 16-bit input buses based on a single select signal and forwards the selected input to the output.

The module is designed using **hierarchical structural modeling**, where sixteen instances of a 1-bit `mux` module are instantiated simultaneously using Verilog's **module array instantiation** feature.

This design demonstrates modular RTL development and code reusability.

---

## ✨ Features

- 16-bit wide 2:1 multiplexer
- Structural (hierarchical) Verilog design
- Uses sixteen reusable 1-bit MUX modules
- Single select line controls all 16 bits
- Fully synthesizable RTL
- Suitable for FPGA and ASIC implementation

---

## 📋 Specifications

| Parameter | Description |
|-----------|-------------|
| Language | Verilog HDL |
| Module Name | `mux_16` |
| Sub Module | `mux` |
| Design Type | Combinational Logic |
| Input Bus 1 | `i1[15:0]` |
| Input Bus 2 | `i2[15:0]` |
| Select Line | `s` |
| Output Bus | `o[15:0]` |
| Modeling Style | Structural / Hierarchical |

---

## 🏗️ Block Diagram

```
              +--------------------------------+
i1[15:0] ---->|                                |
              |                                |
i2[15:0] ---->|       16-bit 2:1 MUX           |----> o[15:0]
              |                                |
s ----------->|        Select Control          |
              +--------------------------------+
```

### Internal Architecture

```
                mux_16

       +-----------------------+

i1[0] ---> mux ----> o[0]
i2[0] --->

i1[1] ---> mux ----> o[1]
i2[1] --->

i1[2] ---> mux ----> o[2]
i2[2] --->

      ...

i1[15] --> mux ----> o[15]
i2[15] -->

       +-----------------------+

All multiplexers share the same select signal (s).
```

---

## ⚙️ Functional Description

The module consists of **16 individual 2:1 multiplexers**, each responsible for selecting one bit from the corresponding input buses.

The Verilog statement

```verilog
mux mux_1[15:0](o, i1, i2, s);
```

creates sixteen instances of the `mux` module, one for each bit position.

For each bit:

```
o[i] = (i1[i] & ~s) | (i2[i] & s)
```

where:

```
i = 0 to 15
```

---

## 📊 Truth Table

| Select (S) | Output |
|------------|--------|
| 0 | `o = i1` |
| 1 | `o = i2` |

---

## 🔢 Example

### Example 1

```
Input 1 : 16'b0000000011111111

Input 2 : 16'b1111111100000000

Select  : 0

Output  : 16'b0000000011111111
```

---

### Example 2

```
Input 1 : 16'b0000000011111111

Input 2 : 16'b1111111100000000

Select  : 1

Output  : 16'b1111111100000000
```

---

## ⏱️ Timing Behavior

- Pure combinational logic
- No clock required
- Output updates immediately when any input or the select signal changes
- Delay depends only on combinational logic propagation

---

## 💡 Applications

- ALU input selection
- Register file read/write paths
- Processor datapaths
- Memory address and data routing
- FPGA interconnect logic
- Communication systems
- Bus multiplexing

---

## ✅ Advantages

- Modular and reusable architecture
- Easy to expand to wider buses
- Efficient structural implementation
- Simple and readable RTL
- Fully synthesizable
- Demonstrates Verilog module array instantiation

---

## 🧪 Simulation

### Recommended Simulators

- Xilinx Vivado Simulator
- ModelSim
- QuestaSim
- Icarus Verilog
- GTKWave

### Sample Test Cases

| Input 1 (Hex) | Input 2 (Hex) | S | Expected Output |
|---------------|---------------|---|-----------------|
| 16'hAAAA | 16'h5555 | 0 | 16'hAAAA |
| 16'hAAAA | 16'h5555 | 1 | 16'h5555 |
| 16'h1234 | 16'hABCD | 0 | 16'h1234 |
| 16'h1234 | 16'hABCD | 1 | 16'hABCD |

---

## 🔧 Synthesis

- FPGA Compatible ✅
- ASIC Compatible ✅
- Synthesizable RTL ✅
- Uses hierarchical structural design
- No inferred latches or flip-flops

---

## 📁 Project Structure

```
mux_16/

├── rtl/
│   ├── mux.v
│   └── mux_16.v
│
├── tb/
│   └── mux_16_tb.v
│
├── docs/
│   ├── block_diagram.png
│   └── waveform.png
│
└── README.md
```

---

## 🚀 Future Improvements

- Parameterize the data width using Verilog parameters
- Implement 4:1, 8:1, and 16:1 multiplexers
- Create a behavioral implementation for comparison
- Add a self-checking SystemVerilog testbench
- Perform FPGA resource and timing analysis

---

## 👨‍💻 Author

**Maddineni Dileep Kumar**
