---
title: "PrimeTime Timing Path Analysis Guide"
parent: STA
---

# PrimeTime Timing Path Analysis Guide

## Table of Contents

1.  [Introduction to Timing Path Analysis](#introduction-to-timing-path-analysis)
2.  [Basic Timing Path Components](#basic-timing-path-components)
3.  [Full Timing Report Example with Clock Details](#full-timing-report-example-with-clock-details)
4.  [Timing Analysis with Two Clocks and MCP](#timing-analysis-with-two-clocks-and-mcp)
    *   [Setting Up the Multicycle Path](#setting-up-the-multicycle-path)
    *   [Timing Report with Two Clocks and MCP](#timing-report-with-two-clocks-and-mcp)
5.  [Timing Analysis of Half-Cycle Paths](#timing-analysis-of-half-cycle-paths)
    *   [Key Concepts for Half-Cycle Paths](#key-concepts-for-half-cycle-paths)
    *   [Timing Analysis for Rising-to-Falling Edge Path](#timing-analysis-for-rising-to-falling-edge-path)
    *   [Example Timing Report (Setup Check)](#example-timing-report-setup-check)
    *   [Hold Check in a Half-Cycle Path](#hold-check-in-a-half-cycle-path-rising-to-falling)
        *   [Example Timing Report (Hold Check)](#example-timing-report-hold-check)
6.  [Key Timing Report Parameters](#key-timing-report-parameters)
7.  [Clock Skew and Latency](#clock-skew-and-latency)
8.  [Practical Implications](#practical-implications)
9.  [PrimeTime Commands](#primetime-commands)
    *   [Reporting Commands](#reporting-commands)
    *   [Clock Definitions and Timing Constraints Commands](#clock-definitions-and-timing-constraints-commands)
    *   [Delay and Timing Update Commands](#delay-and-timing-update-commands)
10. [Timing Report Analysis Summary](#timing-report-analysis-summary)
11. [Conclusion](#conclusion)

## Introduction to Timing Path Analysis

A timing path report analyzes signal propagation in a digital circuit, ensuring data arrives correctly [1]. It’s essential for verifying circuit performance and identifying setup/hold violations [2].

## Basic Timing Path Components

*   **Timing Path:** The route a signal takes from its startpoint to its endpoint, including logic gates and interconnections [1, 2, 3].
*   **Startpoint:** A flip-flop's clock pin or an input port [2, 3, 4, 5].
*   **Endpoint:** A flip-flop's data input pin or an output port [2, 3, 4, 5].
*   **Clock Path:** The path a clock signal takes to reach a flip-flop [6].
*   **Data Path:** The path taken by the data signal [6].
*   **Slack:** The difference between required and actual arrival times; positive is good, negative is a violation [7, 8].
    *   **Setup Time:** Minimum time data must be stable before the clock edge [2, 7].
    *   **Hold Time:** Minimum time data must be stable after the clock edge [3, 7].

## Full Timing Report Example with Clock Details

```tcl
# Full Timing Report Example with Clock Details
# ============================================================================
# Report : timing
#         -path full_clock_expanded
#         -delay max
# Design : Example_Design
# Version: V-2023.12
# Date : Tue Oct 24 14:30:00 2024
# ****************************************
# Startpoint:   regA/Q (rising edge-triggered flip-flop clocked by clk)
# Endpoint:     regB/D (rising edge-triggered flip-flop clocked by clk)
# Path Group:   clk
# Path Type:    max
# ----------------------------------------------------------------------------
# Point                         Incr       Path
# ----------------------------------------------------------------------------
# clock clk (rise edge)           0.00       0.00
#   clock source latency          0.00       0.00
# clk (in)                        0.00       0.00 r
#   clock network delay (propagated) 0.10   0.10 r
# regA/CP (DFF1X)                 0.00       0.10 r
# regA/Q (DFF1X)                  0.15       0.25 f
# U1/Z (INV1X)                    0.08       0.33 r
# U2/Z (AND2X)                    0.05       0.38 r
# regB/D (DFF1X)                  0.00       0.38 r
# ----------------------------------------------------------------------------
# data arrival time                                  0.38
# ----------------------------------------------------------------------------
# clock clk (rise edge)           10.00      10.00
#   clock source latency           0.00       10.00
# clk (in)                         0.00      10.00 r
#   clock network delay (propagated)  0.12    10.12 r
# regB/CP (DFF1X)                 0.00      10.12 r
#   clock uncertainty           -0.05      10.07
# library setup time                -0.10     9.97
# ----------------------------------------------------------------------------
# data required time                                 9.97
# ----------------------------------------------------------------------------
# data required time                            9.97
# data arrival time                              -0.38
# ----------------------------------------------------------------------------
# slack (MET)                                      9.59
# ============================================================================
```

*   **Startpoint & Endpoint**: Identifies start and end points of the timing path (regA/Q to regB/D).
*   **Path Group & Type**: Specifies the clock group (`clk`) and analysis type (`max` for setup check).
*   **Clock Path (Launch):** Traces clock signal from source to `regA/CP` including source latency and network delay.
*   **Data Path (Launch):** Traces the data from `regA/Q` through logic (`U1/Z`, `U2/Z`) to `regB/D`.
*   **Data Arrival Time**: Total time for data to reach `regB/D` (0.38 ns).
*   **Clock Path (Capture):** Traces the clock signal from source to `regB/CP`, including the clock network delay.
*   **Data Required Time**: Time data must arrive at `regB/D` to meet setup requirements (9.97 ns).
*   **Slack**: Difference between data required and data arrival times (9.59 ns, meets setup).

## Timing Analysis with Two Clocks and MCP

### Setting Up the Multicycle Path

Use `set_multicycle_path -setup 2 -from [get_clocks CLK_A] -to [get_clocks CLK_B]` for a two-cycle setup path and `set_multicycle_path -hold 1 -from [get_clocks CLK_A] -to [get_clocks CLK_B]` for the corresponding hold path.

### Timing Report with Two Clocks and MCP
```tcl
# Timing Report with Two Clocks and MCP
# ============================================================================
# Report : timing
#         -path full_clock_expanded
#         -delay max
# Design : MultiClock_Design
# Version: V-2023.12
# Date : Tue Oct 24 14:30:00 2024
# ****************************************
# Startpoint:   regA/Q (rising edge-triggered flip-flop clocked by CLK_A)
# Endpoint:     regB/D (rising edge-triggered flip-flop clocked by CLK_B)
# Path Group:   CLK_B
# Path Type:    max
# ----------------------------------------------------------------------------
# Point                         Incr       Path
# ----------------------------------------------------------------------------
# clock CLK_A (rise edge)         0.00       0.00
#   clock source latency          0.00       0.00
# CLK_A (in)                      0.00       0.00 r
#   clock network delay (propagated) 0.08   0.08 r
# regA/CP (DFF1X)                 0.00       0.08 r
# regA/Q (DFF1X)                  0.15       0.23 f
# U1/Z (INV1X)                    0.07       0.30 r
# U2/Z (AND2X)                    0.04       0.34 r
# regB/D (DFF1X)                  0.00       0.34 r
# ----------------------------------------------------------------------------
# data arrival time                                  0.34
# ----------------------------------------------------------------------------
# clock CLK_B (rise edge)         20.00      20.00
#   clock source latency          0.00      20.00
# CLK_B (in)                      0.00      20.00 r
#   clock network delay (propagated) 0.10   20.10 r
# regB/CP (DFF1X)                 0.00      20.10 r
#   clock uncertainty           -0.05      20.05
# library setup time                -0.10     19.95
# ----------------------------------------------------------------------------
# data required time                                 19.95
# ----------------------------------------------------------------------------
# data required time                                19.95
# data arrival time                                -0.34
# ----------------------------------------------------------------------------
# slack (MET)                                       19.61
# ============================================================================
```

*   Two clock domains (CLK_A and CLK_B).
*   Launch clock path shows CLK_A, capture clock path shows CLK_B.
*   Data required time is calculated based on the second edge of CLK_B due to MCP.
*   Slack of 19.61 ns indicates the timing requirements are met.

## Timing Analysis of Half-Cycle Paths

### Key Concepts for Half-Cycle Paths

*   Data launched on one clock edge (e.g., rising) and captured on the opposite edge (e.g., falling) [1, 2].
*   Reduced time for data to propagate (half of clock period) [6].
*   Setup and hold checks are performed with respect to the half-cycle capture edge [7, 8].

### Timing Analysis for Rising-to-Falling Edge Path

*   **Launch Edge**: Data launches from FF1 on the rising edge.
*   **Capture Edge**: Data arrives at FF2 before the falling edge.
*   **Setup Check**: Measures data arrival before the falling edge.
*   **Hold Check**: Verifies data stability after the falling edge with an extra half-cycle margin.

### Example Timing Report (Setup Check)
```tcl
# Example Timing Report (Setup Check)
# ============================================================================
# Startpoint: UFF5 (falling edge-triggered flip-flop clocked by CLKP)
# Endpoint: UFF3 (rising edge-triggered flip-flop clocked by CLKP)
# Path Group: CLKP
# Path Type: max
# Point                     Incr   Path
# -------------------------------------------------
# clock CLKP (fall edge)       6.00   6.00
# clock source latency         0.00   6.00
# CLKP (in)                  0.00   6.00 f
# UCKBUF4/C (CKB)              0.06   6.06 f
# UCKBUF6/C (CKB)              0.06   6.12 f
# UFF5/CKN (DFN)               0.00   6.12 f
# UFF5/Q (DFN)                 0.16   6.28 r
# UNAND0/ZN (ND2)              0.03   6.31 f
# UFF3/D (DFF)                 0.00   6.31 f
# data arrival time                  6.31
# -------------------------------------------------
# clock CLKP (rise edge)      12.00   12.00
# clock source latency        0.00    12.00
# CLKP (in)                 0.00    12.00 r
# UCKBUF4/C (CKB)              0.07    12.07 r
# UFF3/CK (DFF)                0.00    12.07 r
# clock uncertainty          -0.30   11.77
# library setup time           -0.03   11.74
# data required time                11.74
# -------------------------------------------------
# data required time                11.74
# data arrival time                   -6.31
# -------------------------------------------------
# slack (MET)                         5.43
# ============================================================================
```

*   Startpoint (UFF5) is falling-edge triggered, endpoint (UFF3) is rising-edge triggered.
*   Data arrival time at `UFF3/D` is 6.31ns.
*   Data required time at `UFF3/D` is 11.74ns.
*   Positive slack (5.43ns) indicates setup timing is met.

### Hold Check in a Half-Cycle Path (Rising-to-Falling)

* The hold check is performed against an edge a half cycle earlier than a typical single cycle hold check.

#### Example Timing Report (Hold Check)
```tcl
# Example Timing Report (Hold Check)
# ============================================================================
# Startpoint: UFF5(falling edge-triggered flip-flop clocked by CLKP)
# Endpoint: UFF3 (rising edge-triggered flip-flop clocked by CLKP)
# Path Group: CLKP
# Path Type: min
# Point                   Incr   Path
# -------------------------------------------------
# clock CLKP (fall edge)     6.00   6.00
# clock source latency      0.00   6.00
# CLKP (in)              0.00   6.00 f
# UCKBUF4/C (CKB)          0.06   6.06 f
# UCKBUF6/C (CKB)          0.06   6.12 f
# UFF5/CKN (DFN)            0.00   6.12 f
# UFF5/Q (DFN)             0.16   6.28 r
# UNAND0/ZN (ND2)           0.03  6.31 f
# UFF3/D (DFF)              0.00  6.31 f
# data arrival time               6.31
# -------------------------------------------------
# clock CLKP (fall edge)   6.00   6.00
# clock source latency     0.00   6.00
# CLKP (in)              0.00   6.00 f
# UCKBUF4/C (CKB)          0.06   6.06 f
# UFF3/CK (DFF)            0.00   6.06 f
# library hold time        -0.01   6.05
# data required time              6.05
# -------------------------------------------------
# data required time               6.05
# data arrival time                -6.31
# -------------------------------------------------
# slack (**VIOLATED**)                    -0.26
# ============================================================================
```

*   Startpoint (UFF5) is falling-edge triggered, and the endpoint (UFF3) is rising-edge triggered.
*   Hold check is performed against the falling edge, and the clock edge is specified as 6.00ns.
*   The slack is negative (-0.26 ns), which indicates a hold violation.

## Key Timing Report Parameters

*   **Clock Insertion Delay:** The delay from the clock source to the clock pin of a flip-flop.
*   **Clock Latency:** The total delay from the clock source to the register clock pin.
*   **Multicycle Path:** A path that requires more than one clock cycle to propagate data.
*   **Clock Uncertainty:** Variations in clock arrival times, including jitter and skew.
*   **Clock Skew:** The difference in arrival time of the same clock signal at different points.
*  **Common Period:** When multiple clocks are present, the tool expands the clocks to their least common multiple period for timing analysis.
*   **Transition Time:** The time it takes for a signal to switch from low to high or vice versa.

## Clock Skew and Latency

*   **Clock Skew:** The difference in arrival times of the same clock signal at different points or relative arrival time between different clocks at their respective registers [10, 11, 12, 13, 14, 15].
*   **Clock Latency:** The total delay from the clock source to the register clock pin [9, 12]. It includes source latency and network latency [9].

## Practical Implications

*   Identify timing violations and their causes [1, 8, 37].
*  Optimize design for performance and power [38].
*  Ensure correct function under various conditions [38].

## PrimeTime Commands

### Reporting Commands

*   `report_timing`: Generates detailed timing reports [22, 23, 24, 25, 45].
*   `report_clock_timing`: Reports clock network details including latency and skew [12, 26, 27, 28, 29].
* `report_constraint`: Check for constraint violations [22, 30, 31].
*   `check_timing`: Checks for various timing and constraint issues [30, 32, 33].
*   `report_delay_calculation`: Details how delays are calculated [14, 34, 35, 36].

### Clock Definitions and Timing Constraints Commands

*   `create_clock`: Defines clock source [20, 48].
*   `set_input_delay`, `set_output_delay`: Specifies delays at ports [19, 48].
*   `set_multicycle_path`: Defines multi-cycle paths [41].
*   `set_false_path`: Defines paths to ignore [49, 50].

### Delay and Timing Update Commands

*   `read_sdf`: Reads delay info from SDF files [21].
*   `update_timing`: Updates timing info [50, 51].
*   `get_timing_paths`: Creates collection of timing paths [24, 35].
*   `analyze_paths`: Identifies pins involved in specified paths [52, 53].

## Timing Report Analysis Summary

A timing report provides details on clock and data paths, startpoints, endpoints, slack, and timing violations.

## Conclusion

Understanding timing path analysis is crucial for ASIC designers. This guide covered various aspects, from basic components to complex scenarios, enabling a thorough understanding of timing analysis for robust designs.
