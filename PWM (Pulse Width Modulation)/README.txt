# Pulse Width Modulation (PWM) Generator in Verilog HDL

## Overview

This project implements a **Pulse Width Modulation (PWM)** generator using Verilog HDL. PWM is a digital technique used to control the average power delivered to a load by varying the **duty cycle** of a periodic signal while keeping its frequency constant.

In this implementation, the duty cycle automatically increases from **0% to 100%** by incrementing the ON time (`ton`) after each PWM period.

---

# Features

- Synchronous PWM generation
- Fixed PWM period
- Variable duty cycle
- Automatic duty cycle sweep
- Reset support
- Simple counter-based implementation

---

# Module Description

## Module Name

```
PWM
```

The module generates a PWM waveform whose duty cycle gradually increases every PWM cycle.

---

# Parameters

| Parameter | Default | Description |
|-----------|:-------:|-------------|
| `period` | 10 | PWM period in clock cycles |

---

# Inputs and Outputs

## Inputs

| Signal | Width | Description |
|--------|------:|-------------|
| `clk` | 1 bit | System clock |
| `rst` | 1 bit | Active-high reset |

---

## Outputs

| Signal | Width | Description |
|--------|------:|-------------|
| `dout` | 1 bit | PWM output signal |

---

# Internal Variables

### Counter

```verilog
integer count;
```

Counts clock cycles within one PWM period.

---

### ON Time

```verilog
integer ton;
```

Represents the number of clock cycles for which the PWM output remains HIGH.

---

### New Cycle Indicator

```verilog
reg ncyc;
```

Indicates completion of one PWM period and triggers the duty cycle update.

---

# Working Principle

The PWM generator operates using two clocked processes.

---

## 1. PWM Generation

On every positive clock edge:

### Reset

When

```verilog
rst = 1
```

the module initializes:

- Counter = 0
- ON time = 0
- New-cycle flag = 0

---

### HIGH Interval

If

```verilog
count <= ton
```

then

```
dout = 1
```

and the counter increments.

---

### LOW Interval

If

```verilog
count > ton
```

and

```verilog
count < period
```

then

```
dout = 0
```

and the counter continues incrementing.

---

### End of PWM Period

When the counter reaches the configured period:

- Counter resets to zero.
- `ncyc` is asserted.
- A new PWM cycle begins.

---

## 2. Duty Cycle Update

Whenever a new PWM cycle starts:

```
ton = ton + 1
```

until

```
ton = period
```

After reaching the maximum value, `ton` wraps back to zero and the process repeats.

---

# Duty Cycle Progression

```
Cycle 1

ton = 0

Duty = 0%
```

↓

```
Cycle 2

ton = 1

Duty ≈ 10%
```

↓

```
Cycle 3

ton = 2

Duty ≈ 20%
```

↓

```
...
```

↓

```
Cycle 11

ton = 10

Duty = 100%
```

↓

```
ton = 0

Repeat
```

---

# Example Waveform

```
Clock

|_|-|_|-|_|-|_|-|_|-|_|-|_|-|_|-

PWM (20%)

██____██____██____██____

PWM (50%)

█████_____█████_____

PWM (80%)

████████__████████__
```

(Conceptual representation)

---

# Flow Diagram

```
               Start
                 |
                 |
             Reset?
            /      \
          Yes      No
          |         |
 Initialize Values  |
          |         |
          +---------+
                 |
         count <= ton ?
           /        \
         Yes        No
         |           |
    Output HIGH   Output LOW
         |           |
     Increment    Increment
      Counter      Counter
           \        /
            \      /
        count == period ?
           /        \
         No         Yes
         |           |
      Continue   Reset Counter
                     |
              Assert ncyc
                     |
             Increase ton
                     |
              Start New Cycle
```

---

# Example Operation

For

```
period = 10
```

The duty cycle evolves as follows:

| PWM Cycle | ton | Approximate Duty Cycle |
|-----------|----:|-----------------------:|
| 1 | 0 | 0% |
| 2 | 1 | 10% |
| 3 | 2 | 20% |
| 4 | 3 | 30% |
| 5 | 4 | 40% |
| 6 | 5 | 50% |
| 7 | 6 | 60% |
| 8 | 7 | 70% |
| 9 | 8 | 80% |
| 10 | 9 | 90% |
| 11 | 10 | 100% |
| 12 | 0 | Repeat |

---

# Block Diagram

```
                  +----------------------+
      clk ------->|                      |
      rst ------->|    PWM Generator     |
                  |                      |
                  +----------------------+
                           |
                           |
                        PWM Output
```

---

# Applications

## 1. Motor Control

- DC motor speed control
- Servo motor control
- Brushless DC motor drivers

---

## 2. Power Electronics

- Buck converters
- Boost converters
- Inverters
- Battery charging circuits

---

## 3. Embedded Systems

- LED brightness control
- Fan speed control
- Audio tone generation
- Heating element regulation

---

## 4. FPGA Designs

- PWM signal generation
- Digital power control
- DAC implementation using filtering

---

# Advantages

- Simple counter-based design
- Automatic duty cycle sweep
- Easy to understand and simulate
- Parameterized PWM period
- Suitable for FPGA implementation

---

# Limitations

- The comparison
  ```verilog
  if (count <= ton)
  ```
  causes the HIGH pulse to last `ton + 1` clock cycles. If the intended duty cycle is exactly `ton/period`, using `<` instead of `<=` may be more appropriate.
- `dout` is **not initialized during reset**, so it retains its previous value until the next clock cycle after reset is released. Resetting `dout` improves deterministic behavior.
- The PWM frequency is fixed by the `period` parameter; only the duty cycle changes.
- Integer variables synthesize correctly in many tools but consume more bits than necessary. Explicitly sized registers are generally preferred for hardware design.

---

# Coding Improvements

### 1. Reset the Output

During reset, initialize the PWM output:

```verilog
dout <= 1'b0;
```

---

### 2. Use Sized Registers

Instead of integers:

```verilog
integer count;
integer ton;
```

use appropriately sized registers based on the period, for example:

```verilog
localparam CNT_WIDTH = $clog2(period + 1);

reg [CNT_WIDTH-1:0] count;
reg [CNT_WIDTH-1:0] ton;
```

---

### 3. Clarify the Duty Cycle Calculation

If an exact duty cycle of `ton / period` is desired, compare using:

```verilog
if (count < ton)
```

rather than:

```verilog
if (count <= ton)
```

---

### 4. Combine Sequential Logic (Optional)

The two `always @(posedge clk)` blocks can be merged into a single sequential block to simplify synchronization between `count`, `ncyc`, and `ton`.

---

### 5. Parameterize Initial Duty Cycle

Allow the initial ON time to be configurable:

```verilog
parameter INITIAL_TON = 0;
```

This makes the module more reusable across different applications.

---

# Project Files

```
PWM.v
```

---

## Author

**Dileep Kumar Maddineni**