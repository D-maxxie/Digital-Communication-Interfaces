# README.md

````markdown id="37184"
# 64-to-1 16-bit Multiplexer using Verilog

## 📖 Overview

This project implements a **64-to-1 Multiplexer with 16-bit wide inputs** using Verilog HDL.

The design selects **one of sixty-four 16-bit input buses** and routes it to a single **16-bit output bus** based on a **6-bit select signal**. To improve modularity and scalability, the design follows a **hierarchical structural architecture**, where multiple **8-to-1 16-bit multiplexers (`mux_16_8`)** are combined to form the final 64-to-1 multiplexer.

This hierarchical implementation reduces design complexity and demonstrates reusable RTL design practices commonly used in FPGA and ASIC development.

---

## ✨ Features

- Implements a 64-to-1 multiplexer
- Supports 16-bit wide data buses
- Hierarchical structural RTL design
- Built using reusable 8-to-1 multiplexer modules
- Single 6-bit select signal
- Fully synthesizable Verilog HDL
- FPGA and ASIC compatible

---

## 📋 Specifications

| Parameter | Description |
|-----------|-------------|
| Language | Verilog HDL |
| Module Name | `mux_16_64` |
| Design Type | Combinational Logic |
| Data Width | 16 bits |
| Number of Inputs | 64 |
| Select Width | 6 bits |
| Output Width | 16 bits |
| Sub Module | `mux_16_8` |
| Modeling Style | Structural / Hierarchical |

---

## 🏗️ Architecture

The design consists of **nine 16-bit 8:1 multiplexers**.

### Stage 1

Eight multiplexers select one input from each group of eight inputs.

```
Inputs 1  -  8   ---> mux_16_8 ---> x1

Inputs 9  - 16   ---> mux_16_8 ---> x2

Inputs 17 - 24   ---> mux_16_8 ---> x3

Inputs 25 - 32   ---> mux_16_8 ---> x4

Inputs 33 - 40   ---> mux_16_8 ---> x5

Inputs 41 - 48   ---> mux_16_8 ---> x6

Inputs 49 - 56   ---> mux_16_8 ---> x7

Inputs 57 - 64   ---> mux_16_8 ---> x8
```

Each first-stage multiplexer is controlled by:

```
s[2:0]
```

---

### Stage 2

The outputs of the eight first-stage multiplexers are connected to one final 8-to-1 multiplexer.

```
         x1
         x2
         x3
         x4
         x5
         x6
         x7
         x8
          |
          |
      mux_16_8
          |
          |
     Output o[15:0]
```

The second-stage multiplexer is controlled by:

```
s[5:3]
```

---

## 🏗️ Block Diagram

```
                 64 Inputs
             (16 bits each)

        +----------------------+
        | 8 x mux_16_8 Modules |
        +----------------------+
                 |
                 |
        Intermediate Outputs
      x1 x2 x3 x4 x5 x6 x7 x8
                 |
                 |
           +-------------+
           |  mux_16_8   |
           +-------------+
                 |
                 |
           Output o[15:0]
```

---

## ⚙️ Functional Description

The 6-bit select signal determines which of the 64 inputs is routed to the output.

Selection process:

- **Lower three bits (`s[2:0]`)** select one input within each group of eight.
- **Upper three bits (`s[5:3]`)** select which group's output is forwarded to the final output.

Mathematically:

```
Output = Input[s]
```

where:

```
0 ≤ s ≤ 63
```

---

## 📊 Selection Table

| Select Value | Selected Input |
|--------------|----------------|
| 0 | i1 |
| 1 | i2 |
| 2 | i3 |
| ... | ... |
| 31 | i32 |
| 32 | i33 |
| ... | ... |
| 62 | i63 |
| 63 | i64 |

---

## 🔢 Example

### Example 1

```
s = 6'd0

Output = i1
```

---

### Example 2

```
s = 6'd15

Output = i16
```

---

### Example 3

```
s = 6'd27

Output = i28
```

---

### Example 4

```
s = 6'd63

Output = i64
```

---

## ⏱️ Timing Behavior

- Pure combinational logic
- No clock required
- Two-stage multiplexer architecture
- Output changes immediately when inputs or select signal change
- Overall delay equals the propagation delay of two cascaded 8:1 multiplexers

---

## 💡 Applications

- Processor datapaths
- Register file output selection
- Memory and cache systems
- Network packet routing
- FPGA switching fabrics
- Digital communication systems
- Bus arbitration
- Data acquisition systems

---

## ✅ Advantages

- Hierarchical and modular design
- Highly reusable architecture
- Easy to scale for larger multiplexers
- Efficient structural implementation
- Suitable for FPGA and ASIC synthesis
- Simplifies verification and maintenance

---

## 🧪 Simulation

### Recommended Simulators

- Xilinx Vivado Simulator
- ModelSim
- QuestaSim
- Icarus Verilog
- GTKWave

### Sample Test Cases

| Select (`s`) | Expected Output |
|--------------|-----------------|
| 0 | i1 |
| 7 | i8 |
| 8 | i9 |
| 15 | i16 |
| 31 | i32 |
| 40 | i41 |
| 55 | i56 |
| 63 | i64 |

---

## 🔧 Synthesis

- FPGA Compatible ✅
- ASIC Compatible ✅
- Fully Synthesizable RTL ✅
- Hierarchical structural implementation
- No sequential elements
- Suitable for large datapath routing applications

---

## 📁 Project Structure

```
mux_16_64/

├── rtl/
│   ├── mux.v
│   ├── mux_16.v
│   ├── mux_16_8.v
│   └── mux_16_64.v
│
├── tb/
│   └── mux_16_64_tb.v
│
├── docs/
│   ├── architecture.png
│   ├── hierarchy.png
│   └── waveform.png
│
└── README.md
```

---

## 🚀 Future Improvements

- Parameterize the number of inputs and data width
- Reduce port count using arrays (SystemVerilog)
- Implement behavioral and generate-based versions
- Develop a self-checking SystemVerilog testbench
- Compare timing and resource utilization across architectures
- Integrate into larger processor or memory subsystems

---

## 👨‍💻 Author

**Maddineni Dileep Kumar**
