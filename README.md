# PowerConverter_LabActivities_BuckConverter
# Synchronous Buck Converter & PID Control Lab

This repository contains documentation, hardware schematics, and experimental measurements for a DC-DC Buck Converter driven by an **IR2106** MOSFET driver and regulated via a closed-loop **PID controller**.

---

## 📋 System & Converter Parameters

The nominal hardware specifications and control design targets for the Buck converter are listed below:

| Parameter | Symbol | Value / Range |
| :--- | :---: | :--- |
| **Input Voltage** | \(V_{in}\) | \(15\,\text{V}\) |
| **Filter Inductance** | \(L\) | \(390\,\mu\text{H} + 560\,\mu\text{H}\) (\(950\,\mu\text{H}\) total) |
| **Filter Capacitance** | \(C\) | \(330\,\mu\text{F}\) |
| **Load Resistance** | \(R_{load}\) | \(5\,\Omega - 15\,\Omega\) |
| **Switching Frequency** | \(f_s\) | \(5\,\text{kHz} - 50\,\text{kHz}\) |
| **Voltage Sensor Gain** | \(H\) | \(1\) |
| **PID Crossover Frequency** | \(f_c\) | \(1\,\text{kHz}\) |
| **PID Phase Margin** | \(\text{PM}\) | \(60^\circ\) |

---

## 🔧 Hardware Architecture

### Gate Driver: IR2106 MOSFET Driver
The **IR2106** is a high-voltage, high-speed power MOSFET and IGBT driver featuring independent high-side and low-side referenced output channels.
* **Floating Channel:** Designed for bootstrap operation up to \(+600\,\text{V}\).
* **Logic Compatibility:** Standard CMOS or LSTTL logic inputs (down to \(3.3\,\text{V}\)).
* **Outputs in Phase:** Gate driver outputs match the phase of their respective logic inputs.
* **Gate Drive Supply (\(V_{CC}\)):** Operational range from \(10\,\text{V}\) to \(20\,\text{V}\).

#### IR2106 Pin Configuration

| Pin # | Name | Description |
| :---: | :---: | :--- |
| **1** | \(V_{CC}\) | Low-side and logic fixed supply |
| **2** | \(HIN\) | Logic input for high-side gate driver output (in phase) |
| **3** | \(LIN\) | Logic input for low-side gate driver output (in phase) |
| **4** | \(COM\) | Low-side return (Ground) |
| **5** | \(LO\) | Low-side gate drive output |
| **6** | \(V_S\) | High-side floating supply return |
| **7** | \(HO\) | High-side gate drive output |
| **8** | \(V_B\) | High-side floating supply (Bootstrap) |

### Power Stage & Output Filter
* **Driver & Buck Configuration:** (`image/20261110_Lab1.jpg`, `image/IR2106_driver_MOS.pdf`)
* **L-C Output Filter & Load Connections:** (`image/20261110_Lab2.jpg`, `image/20261110_Lab3.jpg`)

---

## 📐 Theoretical Analysis & Design Guidelines

### Inductor Current Ripple (\(\Delta I_L\))
In Continuous Conduction Mode (CCM), the peak-to-peak inductor current ripple is calculated as:

\[\Delta I_L = \frac{V_{\text{in}} - V_{\text{out}}}{L \cdot f_s} \cdot D\]

Where:
* \(\Delta I_L\): Peak-to-peak inductor current ripple (\(\text{A}\))
* \(V_{\text{in}}, V_{\text{out}}\): Input and output DC voltages (\(\text{V}\))
* \(L\): Inductance value (\(\text{H}\))
* \(D\): Duty cycle, ideally \(D = \frac{V_{\text{out}}}{V_{\text{in}}}\)
* \(f_s\): Switching frequency (\(\text{Hz}\))

> **Design Rule of Thumb:** Target \(\Delta I_L\) to be **20% to 40%** of the maximum load current (\(I_{\text{out,max}}\)) to balance inductor size and core losses. Ripple can be reduced by increasing either the switching frequency (\(f_s\)) or the inductance (\(L\)).

---

## 🧪 Experimental Validation

### Test #1: Open-Loop Inductor Current Ripple
Compare theoretical numerical results against physical oscilloscope measurements under the following operating conditions:
* **Input Voltage (\(V_{in}\)):** \(10\,\text{V}\)
* **Duty Cycle (\(D\)):** \(50\%\)
* **Switching Frequency (\(f_s\)):** \(20\,\text{kHz}\)
* **Inductance (\(L\)):** \(390\,\mu\text{H}\)

*Note: Due to non-idealities (parasitic resistances and voltage drops), the real output voltage differs from the ideal \(V_{out} = D \cdot V_{in}\). Always use the measured \(V_{out}\) when calculating the expected ripple.*

---

## 📁 Repository Structure
```text
├── image/
│   ├── 20261110_Lab1.jpg       # Buck driver hardware setup
│   ├── 20261110_Lab2.jpg       # Load connection setup
│   ├── 20261110_Lab3.jpg       # L-C output filter hardware
│   └── IR2106_driver_MOS.pdf   # IR2106 schematic diagram
└── README.md
```
