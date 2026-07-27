# Sine Wave Generator in Verilog HDL

## Overview

This project implements a **Digital Sine Wave Generator** using **Look-Up Table (LUT)** architecture in Verilog HDL. Instead of calculating sine values mathematically during runtime, the module stores **64 precomputed sine samples** and outputs the corresponding sample based on the input address.

This technique is widely used in FPGA and ASIC designs because it is fast, deterministic, and consumes minimal computational resources.

---

# Features

- 64-point sine wave Look-Up Table (LUT)
- 16-bit output samples
- Precomputed sine values
- Simple ROM-based waveform generation
- FPGA-friendly architecture
- Suitable for DDS (Direct Digital Synthesis)

---

# Module Description

## Module Name

```
sine_wave_generator
```

The module outputs one sine sample corresponding to the supplied address.

---

# Inputs and Outputs

## Inputs

| Signal | Width | Description |
|--------|------:|-------------|
| `t` | 10 bits | Address (sample index) |

---

## Outputs

| Signal | Width | Description |
|--------|------:|-------------|
| `o` | 16 bits | Sine wave sample output |

---

# Internal Architecture

The design contains **64 registers**, each storing one sample of the sine waveform.

```
i1
i2
i3
...
i64
```

Each register contains a precomputed amplitude value.

---

# Lookup Table

The stored values are generated using:

```
Amplitude = 1000 × sin(θ) + 1000
```

where

```
θ = Sample × (360° / 64)
```

Thus:

- Minimum value ≈ 0
- Maximum value ≈ 2000
- DC Offset = 1000

The offset ensures that all values are positive, making the waveform suitable for unsigned digital systems.

---

# Working Principle

### Step 1

The input `t` represents the sample number.

Example:

```
t = 0
```

---

### Step 2

The multiplexer selects one of the stored values.

Example

```
t = 0

Output = i1 = 1000
```

---

### Step 3

Changing `t` sequentially produces a complete sine wave.

```
t = 0 → 63
```

After the last sample, the sequence repeats.

---

# Sample Values

| Sample | Output |
|---------|-------:|
| 0 | 1000 |
| 1 | 1098 |
| 2 | 1195 |
| 3 | 1290 |
| 4 | 1383 |
| 5 | 1471 |
| 6 | 1556 |
| 7 | 1634 |
| ... | ... |
| 16 | 2000 |
| ... | ... |
| 32 | 1000 |
| ... | ... |
| 48 | 0 |
| ... | ... |
| 63 | 902 |

These 64 values approximate one complete cycle of a sine wave.

---

# Output Waveform

```
Amplitude

2000 |                 ****
     |             ****    ****
1500 |          ***            ***
     |        **                 **
1000 |-------*---------------------*-------
     |      *                       *
 500 |     *                         *
     |    *                           *
   0 |___*_____________________________*____

        One Complete Sine Wave Cycle
```

(Conceptual representation)

---

# Block Diagram

```
                +--------------------------+
 Address (t) -->|                          |
                |   64-Point LUT / ROM     |
                |      (Sine Samples)      |
                +--------------------------+
                           |
                           |
                     16-bit Output
```

---

# Flow Diagram

```
              Start
                |
                |
      Receive Sample Address
                |
                |
      Select Corresponding LUT Entry
                |
                |
      Output Stored Sample
                |
                |
      Wait for Next Address
                |
                |
             Repeat
```

---

# Mathematical Representation

Stored sample:

```
Output = 1000 × sin(θ) + 1000
```

where

```
θ = n × (360° / 64)

n = 0 ... 63
```

---

# Applications

## 1. Direct Digital Synthesis (DDS)

- Signal generators
- Arbitrary waveform generators

---

## 2. Digital Communication

- QPSK modulators
- OFDM systems
- Carrier generation

---

## 3. Audio Systems

- Tone generators
- Digital synthesizers
- Musical instruments

---

## 4. FPGA Projects

- DAC interfacing
- Function generators
- PWM-based sine generation

---

## 5. Power Electronics

- SPWM inverter control
- Motor drive applications
- UPS waveform generation

---

# Advantages

- Very fast waveform generation
- No real-time trigonometric calculations
- Simple FPGA implementation
- Predictable timing
- Easy to interface with DACs

---

# Limitations

- The module relies on an external `mux_16_64` module, which is not included here.
- The input `t` is 10 bits wide, but only 64 entries are available. If `t` exceeds the valid range (`0–63`), the behavior depends on the multiplexer implementation.
- Using 64 individual registers (`i1` through `i64`) is difficult to maintain compared to using a memory array.

---

# Coding Improvements

### 1. Replace Individual Registers with an Array

Instead of:

```verilog
reg [15:0] i1, i2, ..., i64;
```

use:

```verilog
reg [15:0] sine_lut [0:63];
```

This is cleaner, easier to initialize, and scales well.

---

### 2. Match the Address Width

Since only 64 samples are stored, a 6-bit address is sufficient:

```verilog
input [5:0] t;
```

This prevents invalid addresses from being presented to the lookup table.

---

### 3. Use a ROM-Style Lookup

Rather than a large multiplexer, access the lookup table directly:

```verilog
assign o = sine_lut[t];
```

This allows synthesis tools to infer ROM or block RAM resources on many FPGA families.

---

### 4. Store Initialization in a Memory File (Optional)

For larger waveforms, initialize the LUT using:

```verilog
$readmemh("sine.mem", sine_lut);
```

or

```verilog
$readmemb("sine.mem", sine_lut);
```

This keeps the source code concise and simplifies waveform updates.

---

### 5. Parameterize the Number of Samples

Define parameters such as:

```verilog
parameter LUT_SIZE = 64;
parameter DATA_WIDTH = 16;
```

to make the design reusable for different waveform resolutions.

---

# Project Files

```
sine_wave_generator.v
mux_16_64.v
```

---

## Author

**Dileep Kumar Maddineni**