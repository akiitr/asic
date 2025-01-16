---
title: what is PDN?
parent: pdn
---

# Power Delivery Network (PDN) Design for System-on-Chip (SoC)

**Table of Contents**

1. [Introduction](#introduction)
2. [PDN Fundamentals](#pdn-fundamentals)
    *   [What is a PDN?](#what-is-a-pdn)
    *   [Why is PDN Design Important?](#why-is-pdn-design-important)
    *   [PDN Components](#pdn-components)
3. [PDN Design Goals and Challenges](#pdn-design-goals-and-challenges)
    *   [Low Impedance](#low-impedance)
    *   [IR Drop Minimization](#ir-drop-minimization)
    *   [Signal Integrity](#signal-integrity)
    *   [Electromigration (EM)](#electromigration-em)
    *   [Thermal Management](#thermal-management)
4. [PDN Design and Analysis Flow](#pdn-design-and-analysis-flow)
    *   [Chip Power Requirements](#chip-power-requirements)
    *   [Package Selection](#package-selection)
    *   [On-Die PDN Design](#on-die-pdn-design)
        *   [Power Grid Structure](#power-grid-structure)
        *   [Decoupling Capacitors (Decaps)](#decoupling-capacitors-decaps)
    *   [Static IR Drop Analysis](#static-ir-drop-analysis)
    *   [Dynamic Voltage Drop (DVD) Analysis](#dynamic-voltage-drop-dvd-analysis)
    *   [Electromigration (EM) Analysis](#electromigration-em-analysis)
    *   [Thermal Analysis](#thermal-analysis)
    *   [Chip-Package-Board Co-Design](#chip-package-board-co-design)
5. [Low-Power Design Techniques Affecting PDN](#low-power-design-techniques-affecting-pdn)
    *   [Clock Gating](#clock-gating)
    *   [Power Gating](#power-gating)
    *   [Dynamic Voltage and Frequency Scaling (DVFS)](#dynamic-voltage-and-frequency-scaling-dvfs)
    *   [Multi-Voltage Design](#multi-voltage-design)
6. [Tools and Vendors](#tools-and-vendors)
7. [Conclusion](#conclusion)

## Introduction

The **Power Delivery Network (PDN)** is a critical aspect of System-on-Chip (SoC) design. It's responsible for distributing power from the external voltage regulators to all the components on the chip, including the core logic, I/Os, and memory. A well-designed PDN is essential for ensuring reliable operation, meeting performance targets, and achieving energy efficiency. This document provides a comprehensive overview of PDN design for SoCs, covering its fundamentals, design goals, challenges, analysis flow, and the impact of low-power design techniques.

## PDN Fundamentals

### What is a PDN?

A **Power Delivery Network (PDN)** is the complete system that delivers power from the voltage source to the transistors on an integrated circuit. It encompasses the on-chip power grid, package connections, and the board-level power distribution network.

### Why is PDN Design Important?

*   **Reliability:** A poorly designed PDN can lead to voltage drops (IR drop) and ground bounce, causing logic errors and chip failures.
*   **Performance:** Voltage drops can slow down circuits, impacting the chip's performance and potentially causing timing violations.
*   **Power Consumption:** A well-designed PDN minimizes power losses in the distribution network, improving energy efficiency.
*   **Signal Integrity:** PDN noise can couple to signal lines, degrading signal integrity and causing data corruption.
*   **Electromigration (EM):** High current densities in the PDN can lead to electromigration, a phenomenon that causes metal voids and eventually open circuits, reducing chip lifetime.

### PDN Components

A typical PDN consists of the following components:

*   **Voltage Regulator Module (VRM):** An external component on the board that provides the regulated voltage supply.
*   **Printed Circuit Board (PCB):** Power planes and traces on the PCB distribute power from the VRM to the package.
*   **Package:** The package (e.g., Ball Grid Array (BGA), Flip Chip) connects the chip to the PCB. It contains power and ground balls/bumps and internal routing.
*   **On-Chip Power Grid:** A network of metal lines and vias that distributes power across the chip. It typically consists of multiple metal layers forming a mesh or grid structure.
*   **Decoupling Capacitors (Decaps):** Capacitors placed on-chip, on-package, and on-board to provide a local source of charge and reduce voltage fluctuations.

## PDN Design Goals and Challenges

### Low Impedance

The PDN should have low impedance across a wide frequency range to minimize voltage fluctuations caused by switching currents. This requires careful design of the on-chip power grid, package, and board-level power distribution.

### IR Drop Minimization

**IR drop** is the voltage drop across the PDN due to the resistance of the metal lines and vias. Excessive IR drop can cause logic errors and performance degradation. The goal is to keep the IR drop within acceptable limits (e.g., less than 5% of the supply voltage).

### Signal Integrity

The PDN should be designed to minimize noise coupling to signal lines. This can be achieved through proper shielding, decoupling, and careful routing of power and signal lines.

### Electromigration (EM)

**Electromigration** is the movement of metal atoms due to high current densities. It can cause voids or hillocks in the metal lines, leading to increased resistance or short circuits. PDN design must consider EM limits to ensure long-term reliability.

### Thermal Management

The PDN generates heat due to the current flowing through its resistance. Proper thermal management is needed to dissipate this heat and prevent overheating, which can impact performance and reliability.

## PDN Design and Analysis Flow

### Chip Power Requirements

*   The first step is to estimate the total power consumption of the chip, including dynamic power (due to switching activity) and leakage power.
*   Power estimation is typically done using power analysis tools that take into account the design's activity factors, operating frequency, and technology parameters.

### Package Selection

*   The choice of package has a significant impact on PDN performance. Factors to consider include the number of power and ground balls/bumps, the package's internal routing, and its thermal characteristics.
*   Flip-chip packages generally offer better PDN performance than wire-bond packages due to their lower inductance and higher density of connections.

### On-Die PDN Design

#### Power Grid Structure

*   The on-chip power grid is typically designed as a mesh or grid structure using multiple metal layers.
*   The grid's pitch (spacing between metal lines) and width are optimized to minimize resistance and IR drop while considering routing congestion.
*   Power switches may be incorporated to enable power gating.

#### Decoupling Capacitors (Decaps)

*   **Decaps** are essential for reducing voltage fluctuations and providing a local source of charge during switching events.
*   Different types of decaps are used, each with different characteristics:
    *   **MOS decaps:** High capacitance density but higher leakage.
    *   **Metal-Insulator-Metal (MIM) decaps:** Lower leakage but lower capacitance density.
    *   **Metal-Oxide-Metal (MOM) decaps:** Offer a trade-off between capacitance density and leakage.
*   Decaps placement is critical. They should be placed close to the switching circuits to minimize the loop inductance.

### Static IR Drop Analysis

*   **Static IR drop analysis** calculates the voltage drop across the PDN under steady-state conditions (average current).
*   It involves solving for the voltage distribution in the PDN using the resistance of the power grid and the average current drawn by each cell.
*   **Tools:** Ansys RedHawk-SC, Cadence Voltus, Synopsys PrimeRail

### Dynamic Voltage Drop (DVD) Analysis

*   **Dynamic Voltage Drop (DVD) analysis** simulates the transient voltage fluctuations caused by switching activity.
*   It takes into account the dynamic current waveforms, the inductance and capacitance of the PDN, and the effect of decoupling capacitors.
*   DVD analysis is more accurate than static IR drop analysis but also more computationally expensive.
*   **Tools:** Ansys RedHawk-SC, Cadence Voltus, Synopsys PrimeRail

### Electromigration (EM) Analysis

*   **EM analysis** checks the current density in the power grid against the technology's EM limits.
*   It identifies potential EM hotspots that need to be addressed through design modifications.
*   **Tools:** Ansys RedHawk-SC, Cadence Voltus, Synopsys PrimeRail

### Thermal Analysis

*   **Thermal analysis** simulates the temperature distribution on the chip, considering the heat generated by the PDN and the logic circuits.
*   It helps identify potential thermal hotspots and evaluate the effectiveness of the cooling solution.
*   **Tools:** Ansys Icepak, Cadence Celsius Thermal Solver

### Chip-Package-Board Co-Design

*   PDN design should not be done in isolation. The chip, package, and board must be co-designed and co-analyzed to ensure optimal PDN performance.
*   This involves:
    *   **Package and board modeling:** Creating accurate models of the package and board parasitics.
    *   **Co-simulation:** Simulating the chip, package, and board together to capture their interactions.
    *   **Optimization:** Jointly optimizing the chip, package, and board PDN to meet the design goals.

## Low-Power Design Techniques Affecting PDN

Several low-power design techniques can significantly impact the PDN:

### Clock Gating

*   **Clock gating** disables the clock signal to idle blocks, reducing dynamic power consumption.
*   **Impact on PDN:** Clock gating reduces the average current drawn from the PDN but can also introduce current spikes when blocks are turned on or off.

### Power Gating

*   **Power gating** shuts off the power supply to inactive blocks, reducing both dynamic and leakage power.
*   **Impact on PDN:** Power gating introduces significant inrush currents when blocks are powered up, requiring careful design of the power switches and decoupling capacitors.
*   **Power switches:** Specialized transistors used to turn on/off power to a block. Design needs to consider the trade-off between switch size (impact on area and performance) and the ability to handle inrush currents.

### Dynamic Voltage and Frequency Scaling (DVFS)

*   **DVFS** dynamically adjusts the supply voltage and operating frequency based on the workload to optimize power consumption.
*   **Impact on PDN:** DVFS requires the PDN to be stable across a wide range of voltage and frequency operating points.

### Multi-Voltage Design

*   **Multi-voltage design** uses different supply voltages for different blocks on the chip to optimize power and performance.
*   **Impact on PDN:** Multi-voltage design requires multiple power grids, which can complicate the PDN design and increase the risk of noise coupling between domains.

## Tools and Vendors

Several EDA (Electronic Design Automation) vendors provide tools for PDN design and analysis:

*   **Ansys:**
    *   **RedHawk-SC:** Industry-leading tool for full-chip power integrity and reliability analysis, including static and dynamic IR drop, electromigration, and thermal analysis.
    *   **Totem:** Power integrity and noise analysis platform for analog, mixed-signal, and custom digital designs.
    *   **PathFinder:** ESD (Electrostatic Discharge) analysis solution.
    *   **Icepak:** Thermal analysis tool.

*   **Cadence:**
    *   **Voltus Fi Custom Power Integrity Solution:**  Power integrity signoff solution for transistor-level designs.
    *   **Voltus Power Integrity Solution:** Full-chip power integrity analysis, including IR drop, EM, and thermal.
    *   **Celsius Thermal Solver:** Thermal analysis and thermal-electrical co-simulation.
    *   **Sigrity:**  Signal and power integrity analysis for packages and PCBs.

*   **Siemens EDA:**
    *   **Calibre xACT:** Parasitic extraction tool.
    *   **HyperLynx:** Signal integrity, power integrity, and thermal analysis for PCBs.

*   **Synopsys:**
    *   **PrimeRail:**  Rail analysis solution for IR drop and electromigration, integrated with the PrimeTime platform.

## Conclusion

PDN design is a crucial aspect of modern SoC design, impacting reliability, performance, and power efficiency. This document has provided a comprehensive overview of PDN design, covering its fundamentals, design goals, analysis flow, and the impact of low-power techniques. As SoC designs become more complex and technology nodes continue to shrink, PDN design will become even more challenging. A thorough understanding of PDN principles and the use of advanced analysis tools will be essential for creating robust and high-performance SoCs.
