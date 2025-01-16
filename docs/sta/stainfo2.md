---
title: "Static Timing Analysis (STA) in ASIC Design"
parent: STA
---

# Static Timing Analysis (STA) in ASIC Design: A Comprehensive Overview

## Table of Contents

1.  [Introduction to Static Timing Analysis (STA)](#introduction-to-static-timing-analysis-sta)
2.  [Why is STA Important?](#why-is-sta-important)
    *   [Timing Verification](#timing-verification)
    *   [Performance Optimization](#performance-optimization)
    *   [Sign-Off Criterion](#sign-off-criterion)
3.  [Key Components of STA](#key-components-of-sta)
    *   [Timing Paths](#timing-paths)
        *   [Data Paths](#data-paths)
        *   [Clock Paths](#clock-paths)
    *   [Timing Arcs](#timing-arcs)
        *   [Cell Delay](#cell-delay)
        *   [Net Delay](#net-delay)
    *   [Clock Skew and Jitter](#clock-skew-and-jitter)
        *   [Clock Skew](#clock-skew)
        *   [Clock Jitter](#clock-jitter)
    *   [Setup and Hold Time](#setup-and-hold-time)
        *   [Setup Time](#setup-time)
        *   [Hold Time](#hold-time)
    *   [Timing Constraints](#timing-constraints)
        *   [Clock Constraints](#clock-constraints)
        *   [Input/Output Delay Constraints](#inputoutput-delay-constraints)
        *   [False Paths and Multicycle Paths](#false-paths-and-multicycle-paths)
    *   [Timing Libraries](#timing-libraries)
        *   [Liberty Files](#liberty-files)
        *   [Delay Models](#delay-models)
4.  [STA Flow and Methodology](#sta-flow-and-methodology)
    *   [Netlist and Constraints](#netlist-and-constraints)
    *    [STA Flow Diagram](#sta-flow-diagram)
    *   [Timing Analysis Engine](#timing-analysis-engine)
    *   [Timing Report Generation](#timing-report-generation)
    *   [Timing Closure](#timing-closure)
    *   [Sign-Off STA](#sign-off-sta)
5.  [Advanced STA Concepts](#advanced-sta-concepts)
    *   [On-Chip Variation (OCV)](#on-chip-variation-ocv)
    *   [Advanced On-Chip Variation (AOCV)](#advanced-on-chip-variation-aocv)
    *   [Parametric On-Chip Variation (POCV)](#parametric-on-chip-variation-pocv)
    *   [Statistical Static Timing Analysis (SSTA)](#statistical-static-timing-analysis-ssta)
    *   [Crosstalk Delay Analysis](#crosstalk-delay-analysis)
    *   [Signal Integrity Analysis](#signal-integrity-analysis)
6.  [Tools for Static Timing Analysis](#tools-for-static-timing-analysis)
    *   [PrimeTime (Synopsys)](#primetime-synopsys)
    *   [Tempus (Cadence)](#tempus-cadence)
    *   [Questa STA (Siemens EDA)](#questa-sta-siemens-eda)
7.  [Conclusion](#conclusion)

## Introduction to Static Timing Analysis (STA)

Static Timing Analysis (STA) is a method used in the design of digital circuits to verify the timing performance of a design. Unlike simulation-based timing analysis, STA is a static analysis technique that checks all possible timing paths in a design to ensure that they meet the specified timing constraints. STA does not require any input vectors, and it is a static analysis process which is used to verify the timing behavior of the circuit. STA is the most important step in the design process of all synchronous digital circuits.

## Why is STA Important?

STA plays a crucial role in the design and verification of ASICs.

### Timing Verification

*   **Path Analysis:** STA verifies that all timing paths in the design meet the setup and hold time requirements.
*   **Error Detection:** Detects potential timing violations that could lead to incorrect operation of the circuit.
*   **Robust Design:** Ensures that the design is robust and reliable under all conditions.

### Performance Optimization

*   **Critical Path Identification:** STA identifies the critical timing paths that limit the performance of the design.
*   **Design Optimization:** Helps in optimizing the design to improve the overall speed and performance.
*   **Optimization Techniques:** Optimizing the design for performance such as using faster cells in critical paths and also using less congested routing in the critical paths.

### Sign-Off Criterion

*   **Tapeout Requirement:** STA is a mandatory step before tapeout (sending the design for manufacturing).
*   **Timing Sign-Off:** The design has to pass all STA checks before sending it for tapeout.
*   **Reliability:** A design that passes STA is considered reliable and will meet the functionality and the performance targets.

## Key Components of STA

STA involves several key components that are essential for accurate timing analysis.

### Timing Paths

A timing path is a sequence of logic gates and interconnects that a signal travels from a launch flip-flop to a capture flip-flop.

#### Data Paths

*   **Logic Gates and Interconnects:** Data paths consist of logic gates, wires, and vias that carry the data signal from the launch flip-flop to the capture flip-flop.
*   **Delay:** Data paths have delays associated with them which are dependent on the components present in the path.
*   **Data Arrival Time:** The delay of a data path is used to calculate the data arrival time.

#### Clock Paths

*   **Clock Distribution Network:** Clock paths start from the clock source to the clock pins of flip-flops.
*   **Clock Tree:** Clock paths include all the components present in the clock tree.
*   **Clock Arrival Time:** The delay in the clock path is used to calculate the clock arrival time.

### Timing Arcs

Timing arcs define the delay characteristics between the input and output pins of a logic cell and also the delay across a net.

#### Cell Delay

*   **Internal Delay:** Cell delay refers to the time it takes for a signal to propagate through a logic gate.
*   **Input-to-Output Delay:** The cell delay is from an input pin of a cell to an output pin of the cell.
*   **Non-Linear Delay Model:** Cell delay can be dependent on various parameters and is captured using a non-linear delay model.

#### Net Delay

*   **Interconnect Delay:** Net delay refers to the delay caused by the metal wires and vias connecting logic cells.
*   **RC Delay:** The resistance and the capacitance of the interconnect contribute to the net delay.
*   **Extraction Tools:** Extraction tools are used to extract the values of resistance and capacitance.

### Clock Skew and Jitter

Clock skew and jitter are two critical factors that impact timing performance.

#### Clock Skew

*   **Clock Arrival Variation:** Clock skew refers to the difference in arrival time of the clock signal at different flip-flops.
*   **Clock Path Imbalance:** Clock skew is due to imbalances in the clock distribution network.
*   **Impact on Timing:** Clock skew can cause setup and hold time violations.

#### Clock Jitter

*   **Cycle-to-Cycle Variation:** Clock jitter refers to the variation in the clock signal period.
*   **Noise and Instability:** Clock jitter can be caused by noise and instability in the clock source or clock distribution network.
*   **Timing Uncertainty:** Jitter adds to timing uncertainty in the design.

### Setup and Hold Time

Setup and hold time are crucial timing parameters for flip-flops.

#### Setup Time

*   **Data Stability:** Setup time is the minimum time the data signal must be stable before the clock edge to guarantee proper data capture.
*   **Violation:** If the setup time requirement is not met, it can lead to metastability and data corruption.
*   **Data Required Time:** The setup time is used to compute the data required time.

#### Hold Time

*   **Data Stability:** Hold time is the minimum time the data signal must be stable after the clock edge.
*   **Violation:** If the hold time requirement is not met, it can cause data corruption.
*   **Data Required Time:** The hold time is used to compute the data required time.

### Timing Constraints

Timing constraints guide the STA process and specify the timing requirements of the design.

#### Clock Constraints

*   **Clock Period:** Clock constraints specify the clock period, frequency, and duty cycle.
*   **Clock Uncertainty:** Clock uncertainty can be added to the clock period due to jitter or other variations.
*   **Clock Definition:** All the clock signals are defined using the clock constraints.

#### Input/Output Delay Constraints

*   **Input Delays:** Input delays specify the arrival times of input signals relative to the clock.
*   **Output Delays:** Output delays specify the required time for the output signals.
*   **Interface Timing:** Used to define the timing at the interface of the design.

#### False Paths and Multicycle Paths

*   **False Paths:** False paths are paths that are not critical for timing and can be excluded from the STA.
*   **Multicycle Paths:** Multicycle paths are paths where the data is allowed to propagate through multiple clock cycles.
*   **Timing Exclusions:** False and multicycle paths are timing exceptions that should be considered in STA.

### Timing Libraries

Timing libraries contain the timing information about standard cells and other components.

#### Liberty Files

*   **Timing Data:** Liberty files are the industry standard for representing the timing, power, and area information about standard cells.
*   **Delay and Timing Arcs:** Contains detailed information about the delay of cells, setup time, hold time, and timing arcs.
*   **Lookup Tables:** Provides lookup tables (LUT) that are used to calculate the delay based on different conditions.

#### Delay Models

*   **Non-Linear Delay Model (NLDM):** NLDM models the delay of a cell as a function of the input slew and output load.
*   **Composite Current Source (CCS):** CCS model represents the delay of the cells more accurately than NLDM.
*   **Effective Current Source Model:** An enhanced current source model can accurately represent the delay of the cell with better accuracy.

## STA Flow and Methodology

The STA flow typically involves several steps to perform timing analysis effectively.

### Netlist and Constraints

*   **Gate-Level Netlist:** STA uses the gate-level netlist of the design.
*   **Timing Constraints:** Timing constraints are given in Synopsys Design Constraints (SDC) format.
*   **Environment Setup:** Setting up the STA environment with all required files.

### STA Flow Diagram

Here’s a flow diagram outlining the STA process, including inputs, analysis, and ECO generation:

```mermaid
graph TD
    A[Netlist & Constraints] --> B(STA Engine);
    B --> C{Timing Met?};
    C -- Yes --> D[Timing Verified];
    C -- No --> E(Timing ECO);
    E --> B;
    style A fill:#f9f,stroke:#333,stroke-width:2px
    style D fill:#ccf,stroke:#333,stroke-width:2px
    style C fill:#9cf,stroke:#333,stroke-width:2px
    style E fill:#fcc,stroke:#333,stroke-width:2px
    subgraph "Inputs for Analysis"
        style A fill:#f9f,stroke:#333,stroke-width:2px
        A1(Gate-Level Netlist)
        A2(SDC Constraints)
        A3(Timing Libraries)
        A --> A1
        A --> A2
        A --> A3
    end
    subgraph "Inputs for ECO"
        style E fill:#fcc,stroke:#333,stroke-width:2px
        E1(ECO Script)
        E2(Placement/Routing Data)
        E --> E1
        E --> E2
    end
    subgraph "STA Engine Analysis"
        style B fill:#cdf,stroke:#333,stroke-width:2px
        B1(Path Enumeration)
        B2(Delay Calculation)
        B3(Timing Checks)
        B --> B1
        B --> B2
        B --> B3
    end
    subgraph "Analysis Results"
        style C fill:#9cf,stroke:#333,stroke-width:2px
        C1(Setup/Hold Violations)
        C2(Path Delays)
        C3(Slack Values)
        C --> C1
        C --> C2
        C --> C3
    end
    ```

*   **Inputs:** Gate-level netlist, SDC timing constraints, and timing libraries.
*   **Analysis:** Path enumeration, delay calculation, and setup/hold time checks.
*   **ECOs:** If timing violations are found, ECOs (Engineering Change Orders) are made, and the analysis is run again.

### Timing Analysis Engine

*   **Path Enumeration:** Enumerating all the timing paths in the design.
*   **Delay Calculation:** Calculating the delay of all the paths using the timing libraries.
*   **Timing Checks:** Checking the setup and hold time requirements for each path.

### Timing Report Generation

*   **Violation Reports:** Generating reports on setup and hold violations.
*   **Path Delays:** Detailed reports of all the timing paths including their delay.
*   **Critical Paths:** Lists all the critical paths with the violations.

### Timing Closure

*   **Optimization:** Optimizing the design by making changes to the design or to the constraints to fix the timing violations.
*   **Iterative Process:** Timing closure is an iterative process which involves analysis, optimization and then running STA again.
*   **ECO:** Making engineering change orders (ECOs) to the design to fix the timing violations.

### Sign-Off STA

*   **Final Verification:** Final verification after all the timing violations have been resolved.
*   **Robust Design:** Ensures the design meets the timing requirements and it will be reliable.
*   **Tapeout Ready:** The design passes the sign-off STA before sending for tapeout.

## Advanced STA Concepts

### On-Chip Variation (OCV)

*   **Process Variations:** Accounts for variations in manufacturing processes that can affect transistor performance.
*   **Derating Factors:** Applies derating factors to cell delays based on the process variation.
*   **Worst-Case Analysis:** OCV captures the worst-case timing due to process variations.

### Advanced On-Chip Variation (AOCV)

*   **Path Based Derating:** AOCV is a path-based derating methodology.
*   **Reduced Pessimism:** AOCV reduces the pessimism of the analysis when compared to OCV.
*   **Better Accuracy:** AOCV is a more accurate approach when compared to OCV.

### Parametric On-Chip Variation (POCV)

*   **Statistical Variations:** POCV models statistical variations in cell delays based on parameters such as temperature and process.
*   **Gaussian Distributions:** Uses Gaussian distributions to model the variations.
*   **More Accurate Analysis:** POCV provides better timing analysis compared to OCV and AOCV.

### Statistical Static Timing Analysis (SSTA)

*   **Statistical Analysis:** SSTA performs timing analysis by using statistical methods.
*   **Probability Distributions:** SSTA represents cell and net delays as probability distributions.
*   **Realistic Timing Analysis:** Provides a more realistic analysis compared to deterministic STA.

### Crosstalk Delay Analysis

*   **Capacitive Coupling:** Analysis of the delay variations caused due to capacitive coupling between adjacent nets.
*   **Noise Analysis:** Crosstalk analysis can also be used to check for noise issues.
*   **Signal Integrity:** Ensures signal integrity by accounting for the effect of crosstalk on delay.

### Signal Integrity Analysis

*   **Signal Quality:** Ensures the signal quality of the design.
*   **Reflection:** Analyzes the reflections due to impedance mismatch in the routes.
*   **Noise Margins:** Checks for the noise margins in the designed circuit.

## Tools for Static Timing Analysis

Several tools are available in the industry for performing STA.

### PrimeTime (Synopsys)

*   **Industry Standard:** PrimeTime is the industry-standard tool for STA.
*   **Comprehensive Analysis:** Provides comprehensive timing analysis features with good accuracy.
*   **Advanced Features:** Supports advanced features like OCV, AOCV, POCV, and SSTA.

### Tempus (Cadence)

*   **Advanced Features:** Tempus supports advanced analysis and optimization features.
*   **Integration:** Tightly integrated with the Cadence design flow.
*   **High Performance:** Offers high performance and scalability.

### Questa STA (Siemens EDA)

*   **Comprehensive STA:** Provides comprehensive static timing analysis.
*   **Integration:** Integrates well with Siemens EDA tools.
*   **Good Accuracy:** Offers good accuracy with different delay models.

## Conclusion

Static Timing Analysis (STA) is an essential step in the ASIC design flow, ensuring that the designed circuit meets its timing requirements. This detailed overview of STA concepts, components, methodologies, and tools, along with the STA flow diagram, provides a strong foundation for understanding this crucial aspect of digital design. Understanding all the concepts of STA is critical for all ASIC designers. A well-designed circuit must meet all the STA requirements before the chip can be manufactured.
