# 2x1 Multiplexer (MUX) using Verilog

## 📖 Overview

This project implements a **2-to-1 Multiplexer (MUX)** using Verilog HDL.

A multiplexer is a combinational digital circuit that selects one of multiple input signals and forwards the selected input to the output based on a selection line.

This design uses basic logic gates (**NOT, AND, OR**) to implement the MUX functionality using structural modeling.

---

## ✨ Features

- Implements 2:1 Multiplexer
- Structural gate-level modeling
- Uses basic logic gates
- Pure combinational logic
- Synthesizable Verilog HDL
- Suitable for FPGA and ASIC implementation
- Simple building block for larger digital systems

---

## 📋 Specifications

| Parameter | Description |
|-----------|-------------|
| Language | Verilog HDL |
| Module Name | `mux` |
| Design Type | Combinational Logic |
| Number of Inputs | 2 |
| Inputs | `i1`, `i2` |
| Select Line | `s` |
| Output | `o` |
| Modeling Style | Structural Modeling |

---

# 🏗️ Block Diagram

```
                 i1
                 |
                 |
                 +---------+
                           |
                           AND
                           |
                 +---------+
                 |
                 | 
              +-----+
              | OR  |------------ o
              +-----+
                 |
                 |
                 +---------+
                           |
                           AND
                           |
                 +---------+

                 i2
                 |
                 |
                Select
                 |
                 s

              s ---- NOT ---- sc
```

---

# ⚙️ Functional Description

A 2x1 multiplexer selects one of two inputs based on the select signal.

The Boolean equation is:

```
o = (i1 & ~s) | (i2 & s)
```

### Operation:

- When `s = 0`

```
o = i1
```

- When `s = 1`

```
o = i2
```

---

# 📊 Truth Table

| Select (S) | Input 1 (I1) | Input 2 (I2) | Output (O) |
|------------|---------------|---------------|------------|
| 0 | 0 | X | 0 |
| 0 | 1 | X | 1 |
| 1 | X | 0 | 0 |
| 1 | X | 1 | 1 |

`X` = Don't care condition

---

# 🔢 Example

### Case 1:

```
i1 = 1
i2 = 0
s  = 0

Output:

o = i1 = 1
```

### Case 2:

```
i1 = 1
i2 = 0
s  = 1

Output:

o = i2 = 0
```

---

# ⏱️ Timing Behavior

- Combinational circuit
- No clock required
- Output changes based on input/select changes
- Propagation delay depends on logic gate delay

---

# 💡 Applications

- Data selection circuits
- Processor datapaths
- Arithmetic Logic Units (ALU)
- Control units
- Communication systems
- FPGA routing logic
- Digital switching circuits

---

# ✅ Advantages

- Simple hardware implementation
- Requires minimum logic gates
- Fast data selection
- Easily scalable to larger multiplexers
- Fully synthesizable RTL

---

# 🧪 Simulation

## Recommended Simulators

- Xilinx Vivado Simulator
- ModelSim
- QuestaSim
- Icarus Verilog
- GTKWave

## Test Cases

| I1 | I2 | S | Expected Output |
|----|----|---|----------------|
|0|0|0|0|
|1|0|0|1|
|0|1|1|1|
|1|1|1|1|
|1|0|1|0|

---

# 🔧 Synthesis

- FPGA Compatible ✅
- ASIC Compatible ✅
- Synthesizable RTL ✅
- Uses standard combinational gates
- No latches or flip-flops inferred

---

# 📁 Project Structure

```
2x1_mux/

│
├── rtl/
│   └── mux.v
│
├── tb/
│   └── mux_tb.v
│
├── docs/
│   ├── block_diagram.png
│   └── waveform.png
│
└── README.md
```

---

# 🚀 Future Improvements

- Implement 4x1 and 8x1 multiplexers
- Design parameterized N:1 multiplexer
- Add SystemVerilog assertions
- Create verification environment using UVM
- Compare gate-level and behavioral implementations

---

# 👨‍💻 Author

**Maddineni Dileep Kumar**
