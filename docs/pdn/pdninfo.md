---
title: "Power Delivery Networks (PDN) in ASIC Design"
parent: ASIC
---

# Power Delivery Networks (PDN) in ASIC Design: A Detailed Analysis

## Table of Contents

1.  [Introduction to Power Delivery Networks (PDN)](#introduction-to-power-delivery-networks-pdn)
2.  [Importance of PDN](#importance-of-pdn)
    *   [Impact on Performance](#impact-on-performance)
    *   [Impact on Reliability](#impact-on-reliability)
    *   [Impact on Power Consumption](#impact-on-power-consumption)
3.  [PDN Components and Architecture](#pdn-components-and-architecture)
    *   [Power and Ground Pads](#power-and-ground-pads)
    *   [Power and Ground Rings](#power-and-ground-rings)
    *   [Power Straps and Meshes](#power-straps-and-meshes)
    *   [Via Structures](#via-structures)
    *   [Decoupling Capacitors (Decaps)](#decoupling-capacitors-decaps)
4.  [PDN Design Considerations](#pdn-design-considerations)
    *   [IR Drop Analysis](#ir-drop-analysis)
        *   [Static IR Drop](#static-ir-drop)
        *   [Dynamic IR Drop](#dynamic-ir-drop)
    *   [Electromigration (EM)](#electromigration-em)
    *   [Inductance and Impedance](#inductance-and-impedance)
        *   [Target Impedance](#target-impedance)
    *   [Resonance](#resonance)
    *   [Simultaneous Switching Noise (SSN)](#simultaneous-switching-noise-ssn)
5.  [PDN Analysis and Verification](#pdn-analysis-and-verification)
    *   [Simulation Tools](#simulation-tools)
    *   [PDN Modeling Techniques](#pdn-modeling-techniques)
    *   [Verification Methodologies](#verification-methodologies)
    *   [PDN Analysis Flow](#pdn-analysis-flow)
6.  [Advanced PDN Techniques](#advanced-pdn-techniques)
    *   [Adaptive Voltage Scaling (AVS)](#adaptive-voltage-scaling-avs)
    *   [Power Gating](#power-gating)
    *   [On-Chip Voltage Regulators](#on-chip-voltage-regulators)
7.  [Industry Practices and Tools](#industry-practices-and-tools)
    *   [Power Analysis Tools](#power-analysis-tools)
    *   [PDN Verification Tools](#pdn-verification-tools)
    *   [Design Methodologies](#design-methodologies)
8.  [Conclusion](#conclusion)

## Introduction to Power Delivery Networks (PDN)

A Power Delivery Network (PDN) is the intricate system of metal interconnects, vias, and capacitors designed to distribute power from the external power source to all the active and passive components on an integrated circuit (IC). The primary function of a PDN is to provide a stable and reliable power supply to the logic circuitry while minimizing voltage fluctuations, noise, and power losses. A well-designed PDN is critical for ensuring the correct functionality, performance, and reliability of an ASIC. The PDN is a distributed network consisting of multiple metallic layers and multiple components connected in a network. The goal of this network is to deliver a constant supply of power with minimal variations.

## Importance of PDN

The PDN plays a vital role in the functionality and reliability of an ASIC. An inadequate PDN can lead to a variety of issues.

### Impact on Performance

*   **Voltage Drops:** Insufficiently designed PDNs can result in voltage drops (IR drop) across the chip. This can cause transistors to operate at lower voltages, leading to reduced performance (reduced switching speed) and logic errors. This variation of voltage also leads to timing violations.
*   **Signal Integrity:** Variations in the power supply can cause noise in the digital and analog circuits which affects their performance. A noisy PDN can make the circuit to be more susceptible to errors in the signals.
*   **Timing Violations:** Voltage variations due to poorly designed PDN can change the timing of the circuit resulting in setup and hold time violations

### Impact on Reliability

*   **Electromigration (EM):** High current densities in the PDN can cause electromigration, which can lead to metal interconnect failure over time. This can reduce the long-term reliability of the chip and can eventually lead to functional failure.
*   **Thermal Issues:** Uneven power distribution can result in hot spots on the chip which can lead to reduced reliability and also can lead to functional failure. In worst case scenarios it can even cause physical damage.
*   **Device Degradation:** Variations in voltage and current levels can cause degradation of devices over time.

### Impact on Power Consumption

*   **Leakage Current:** Variations in the supply voltage can increase the leakage currents in the transistors. These currents lead to static power consumption in the chip.
*   **Dynamic Power:** Poorly designed PDNs can increase the dynamic power consumption due to higher switching current requirements. If the current drawn by the PDN is not delivered efficiently then it leads to excess power consumption.
*   **Inefficient Power Delivery:** Power wasted due to resistive losses within the PDN contribute to higher overall power consumption.

## PDN Components and Architecture

The PDN is composed of various interconnected components which form a network from the power input pad to the various functional blocks in the chip.

### Power and Ground Pads

*   **External Connection:** These are the points at which the power supply is connected to the chip. These pads have to be designed to withstand high voltage and current levels.
*   **Pad Placement:** The location and number of pads influence the overall PDN design. Proper placement of pads at the periphery are needed to ensure proper connectivity to external power source.
*   **Pad Types:** Various types of pads such as power, ground, and signal pads are required to facilitate the complete functioning of the chip.

### Power and Ground Rings

*   **Perimeter Distribution:** These rings are placed along the periphery of the chip to provide an initial distribution of power across the die.
*   **Low Resistance Paths:** Rings provide low-resistance paths for current flow and provide low impedance to the core circuits.
*   **Layer Assignment:** The power and ground rings can be placed on the top most metal layers for optimal performance

### Power Straps and Meshes

*   **Grid Structure:** Power straps are usually routed vertically or horizontally in the form of grid to distribute power and ground across the entire chip area.
*   **Mesh Structure:** Power mesh structures provide an interconnected network for power distribution and are useful in providing power to larger area.
*   **Metal Layers:** Power straps and meshes use multiple metal layers to reduce resistance.
*   **Layer Selection:** Straps and mesh can be selected in multiple metal layers in the design to reduce the overall impedance.

### Via Structures

*   **Layer Connections:** Vias are used to connect different metal layers in the PDN. These vias carry the power from top level routing to lower level routing and finally to the standard cells.
*   **Via Redundancy:** Redundant vias are added to increase the current carrying capacity and also to improve reliability.
*   **Via Resistance:** Vias have resistance and can cause a voltage drop, thus optimizing the number and type of vias used is necessary to achieve desired goals.

### Decoupling Capacitors (Decaps)

*   **Noise Reduction:** Decaps are placed near the logic blocks to reduce local noise and voltage fluctuations.
*   **Charge Storage:** Decaps act as a local charge reservoir to supply current when the circuit is switching.
*   **Capacitor Types:** Various types of capacitors are used with varying values and properties.
*   **Placement:** Decaps are placed strategically across the chip. These decaps can also be integrated inside the standard cells.

## PDN Design Considerations

Designing an efficient PDN requires careful consideration of various parameters.

### IR Drop Analysis

IR drop is the voltage drop across the power grid due to resistance (R) and current (I).

#### Static IR Drop

*   **DC Analysis:** Static IR drop is analyzed under steady-state DC conditions.
*   **Current Calculation:** The total DC current drawn by the circuit is calculated and then the voltage drop across the grid is analyzed.
*   **Voltage Drop:** The voltage drop is calculated based on the resistance of the metal and the via structures.

#### Dynamic IR Drop

*   **Transient Analysis:** Dynamic IR drop is analyzed under transient conditions during switching activity of the circuit.
*   **Switching Currents:** The dynamic currents drawn during switching are analyzed to determine the voltage variation in the power grid.
*   **Worst-Case Analysis:** Analyzing for the worst case switching scenarios to ensure robust design.

### Electromigration (EM)

*   **Current Density:** EM is a concern when high current densities in metal conductors cause movement of metal atoms, leading to open circuits.
*   **Metal Thickness:** Proper metal width and thickness are selected for the metal routes so that they can withstand the high currents.
*   **Via Design:** Via structures must be designed to support the current requirements to reduce the risk of EM related failures.
*   **Lifetime Calculation:** The lifetime of metal interconnects are calculated based on the current densities.

### Inductance and Impedance

*   **Inductive Effects:** Inductance in the power grid can cause voltage fluctuations due to changes in current.
*   **Impedance:** The impedance of the PDN impacts how the power is delivered to different parts of the circuit. The impedance should be low.
*   **Parasitic Extraction:** Extraction tools are used to estimate the parasitic inductance.

#### Target Impedance

*   **Low Impedance:** The target impedance of the PDN should be low enough to minimize voltage drops and keep the power distribution network stable.
*   **Frequency Response:** The PDN impedance needs to be low across a wide frequency range.
*   **Matching Impedance:** The PDN target impedance is carefully selected so that it does not cause resonance at any frequency.

### Resonance

*   **LC Circuits:** The PDN has inherent inductive and capacitive elements and can resonate at certain frequencies.
*   **Peak Impedance:** Resonance leads to a peak in impedance which results in voltage variations.
*   **Mitigation Techniques:** Proper decoupling with different capacitor values is needed to avoid resonance. The power and ground grid should be designed to minimize the impedance.

### Simultaneous Switching Noise (SSN)

*   **Switching Currents:** SSN occurs when multiple outputs switch simultaneously, causing large current spikes in the PDN.
*   **Voltage Fluctuations:** These current spikes lead to noise and voltage fluctuations in the power supply.
*   **Decoupling:** Proper decoupling is required to reduce SSN.

## PDN Analysis and Verification

Analyzing and verifying the PDN design is critical for achieving robust power delivery.

### Simulation Tools

*   **Power Analysis Tools:** Tools like Voltus (Cadence) and PowerArtist (Synopsys) are used to analyze power consumption.
*   **Spice Simulators:** SPICE simulators such as HSPICE and Spectre are used for accurate simulation of the power grid under different conditions.
*   **IR Drop Analysis Tools:** Tools such as Redhawk (Ansys) are used for IR drop analysis in power delivery networks.

### PDN Modeling Techniques

*   **Simplified Models:** Simplified RLC models can be used for initial analysis.
*   **Full Chip Models:** Detailed models capture the full complexity of the PDN for accurate simulation.
*   **Hierarchical Modeling:** Complex PDNs can be modeled hierarchically to reduce the complexity of the analysis.

### Verification Methodologies

*   **IR Drop Verification:** Ensuring voltage drops are within acceptable limits.
*   **Electromigration Verification:** Checks to make sure the metal widths, via numbers and current densities are within the permissible limits and lifetime is achieved.
*   **Impedance Verification:** Verify the PDN impedance is within target impedance values across the design.
*   **SSN Verification:** Verifying that SSN is within acceptable limits.

### PDN Analysis Flow

Here's a flow diagram outlining the PDN analysis process, including inputs and potential ECOs:

```mermaid
graph TD
    A[Netlist & Power Intent] --> B(PDN Synthesis);
    B --> C{PDN Simulation Setup};
    C --> D[PDN Analysis];
    D --> E{Pass?};
    E -- Yes --> F[PDN Verified];
    E -- No --> G(PDN ECO);
    G --> C;
    style A fill:#f9f,stroke:#333,stroke-width:2px
    style F fill:#ccf,stroke:#333,stroke-width:2px
    style D fill:#9cf,stroke:#333,stroke-width:2px
     style G fill:#fcc,stroke:#333,stroke-width:2px
    subgraph "Inputs for Analysis"
    style A fill:#f9f,stroke:#333,stroke-width:2px
        A1[Netlist]
        A2[Power Intent (UPF)]
        A3[Technology Files]
         A -- A1
         A -- A2
          A -- A3
    end
  subgraph "Inputs for ECO"
      style G fill:#fcc,stroke:#333,stroke-width:2px
        G1[ECO Script]
        G2[Placement/Routing Data]
        G -- G1
        G -- G2
      end
    subgraph "PDN Simulation Setup"
     style C fill:#cdf,stroke:#333,stroke-width:2px
         C1[Simulation Parameters]
         C2[PDN Model]
          C -- C1
          C -- C2
   end
    subgraph "Analysis Results"
        style D fill:#9cf,stroke:#333,stroke-width:2px
       D1[IR Drop Map]
         D2[EM Violations]
         D3[Impedance Plots]
         D -- D1
          D -- D2
           D -- D3
   end
```

*   **Inputs:** Netlist, power intent (UPF), technology files, simulation parameters, and the PDN model.
*   **Analysis:**  IR drop, electromigration, impedance, and SSN analysis.
*   **ECOs:** If the analysis fails, the design is modified (ECO) and the analysis is run again.

## Advanced PDN Techniques

### Adaptive Voltage Scaling (AVS)

*   **Dynamic Voltage Adjustment:** AVS adjusts the supply voltage dynamically based on workload and performance requirements.
*   **Power Savings:** AVS reduces the power consumption without compromising the performance by using an optimal voltage level.
*   **Performance Optimization:** At low loads the voltage can be scaled down, while at higher workloads the voltage can be scaled up.

### Power Gating

*   **Power Shutoff:** Power gating shuts off power to inactive blocks of the circuit, reducing the static power consumption.
*   **Low Power Modes:** Used to implement multiple power modes, providing efficient power management.
*   **Sleep and Active Modes:** Unused blocks are put to sleep mode to reduce power consumption.

### On-Chip Voltage Regulators

*   **Local Voltage Regulation:** On-chip voltage regulators provide precise voltage regulation locally, minimizing noise and variations.
*   **Integration:** These regulators are integrated onto the chip, eliminating external components.
*   **Multiple Voltage Levels:** Different blocks can use different voltages that are generated on chip using the voltage regulators.

## Industry Practices and Tools

### Power Analysis Tools

*   **Voltus (Cadence):** A comprehensive tool for power analysis, including static and dynamic power analysis.
*   **PowerArtist (Synopsys):** Another industry-leading power analysis tool for estimating power.
*   **RedHawk (Ansys):** Used for detailed IR drop and EM analysis, with good modeling capabilities.

### PDN Verification Tools

*   **Voltus (Cadence):** Includes PDN analysis capabilities for verification.
*   **RedHawk (Ansys):** Used for comprehensive PDN analysis, including IR drop, EM, and SSN.
*   **Custom Tools:** Many companies also use their own custom built tools for PDN verification.

### Design Methodologies

*   **Early Planning:** Planning the PDN early in the design process using a top down approach.
*   **Hierarchical Design:** Designing the PDN hierarchically with increasing details at each level.
*   **Iteration:** Iterating over the design based on analysis and verification results.
*   **Collaboration:** Close collaboration between physical design and power integrity engineers is essential.

## Conclusion

A well designed Power Delivery Network (PDN) is essential for the reliable, high-performance, and efficient operation of modern ASICs. This detailed exploration of PDN components, design considerations, analysis, and advanced techniques provides a comprehensive understanding of this critical aspect of ASIC design. Understanding the design constraints and requirements of the PDN and also the analysis and verification methods is required for all ASIC designs. The PDN design has to be well thought out and implemented to ensure the correct operation of the ASIC under all operating conditions.
