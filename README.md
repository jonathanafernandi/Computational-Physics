# Computational-Physics

Python Power Electronics (PPE) circuit simulation files as a case study for the SCIE6063001 – Introduction to Computational Physics course. The project solves two electrical circuit problems using both a numerical (manual calculation) approach and a computational (PPE simulation) approach.

## Table of Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Problem 1: Resistor Network](#problem-1-resistor-network)
- [Problem 2: RLC Rectifier for LED Circuit](#problem-2-rlc-rectifier-for-led-circuit)
- [Simulation Files Explained](#simulation-files-explained)
- [How to Run the Simulation](#how-to-run-the-simulation)
- [Documentation](#documentation)
- [Author](#author)

## Overview

This project applies computational physics principles to solve two electrical circuit problems using **Python Power Electronics (PPE)**, a Python-based circuit simulation tool. Each problem is solved twice: first through manual numerical calculation, then verified through a PPE circuit simulation, with the results compared for consistency.

## Project Structure

```
Computational-Physics/
├── Problem1/
│   ├── Problem1.csv
│   └── Problem1.xlsx
├── Problem2/
│   ├── Problem2.csv
│   └── Problem2.xlsx
├── output/
│   ├── TotalCurrentFlowinTheCircuit.png
│   ├── PotentialDifferenceatEachEndofTheInstance.png
│   ├── TheAmountofCurrentThatPassesThroughResistance2andResistance3.png
│   ├── VoltageatEachResistance.png
│   └── TheAmountofCurrentatEachResistance.png
├── __control.py
├── ckt_output.dat
├── plotkey.txt
├── docs/
│   └── 2602089143-JonathanAlvindoFernandi_AoL-CaseStudy.pdf
└── README.md
```

## Problem 1: Resistor Network

**Circuit**: A DC source (3 V) connected to a resistor network with R1 = 2 Ω in series with a parallel combination of R2 = 2 Ω and R3 = 2 Ω.

### Numerical Approach

1. **Total resistance**: Rp (R2 parallel R3) = 1 Ω, Rs = R1 + Rp = 4 Ω, so Rt = 4 Ω.
2. **Total current**: It = V / Rt = 3 / 4 = 0.75 A.
3. **Voltage across each resistor**: V1 = V2 = V3 = It x R = 1.5 V (since the source voltage divides evenly between the series resistor and the parallel combination in this configuration).
4. **Current through R2 and R3**: I2 = V2 / R2 = 0.5 A, I3 = V3 / R3 = 0.25 A.

### Computational Approach (PPE Simulation)

The circuit is modeled in `Problem1.csv` using labeled components: `VoltageSource_dcsource`, `Resistor_source`, `Resistor_load1/2/3`, `Ammeter_source/load1/2/3`, and `Voltmeter_load1/2/3`, connected via `wire` cells in a grid layout.

Key simulation parameters:

- `VoltageSource_dcsource` peak value = V x sqrt(2) = 3 x sqrt(2) approx 4.243 V.
- `Resistor_source` = 0.01 Ω (kept low to maintain voltage and current stability).
- `Voltmeter_load` voltage level = 1000 V (set high to accommodate simulation range).
- Ammeter positive current direction: clockwise.

Simulation output graphs (in `output/`) confirm the numerical results: total current flow, potential difference at each resistor, and current through R2 and R3.

## Problem 2: RLC Rectifier for LED Circuit

**Scenario**: Designing a basic RLC rectifier to power three parallel green LEDs (minimum operating voltage 2 V, maximum current 20 mA) from a micro-hydro generator (5 V, 20 Hz output), using only available discrete components (each with 0.1 Ω internal resistance).

### Numerical Approach

Available component values were tested systematically to find combinations satisfying the LED current constraint (I <= 20 mA):

- **LED-side resistors**: Testing resistor values from 3 Ω to 510 Ω, only the **510 Ω** resistor (adjusted via parallel LED configuration to an effective 170 Ω) produced a safe current of 0.012 A, so R1 = R2 = R3 = 510 Ω.
- **Load resistor (rload)**: Testing the same resistor set against the generator's 5 V output, **220 Ω** produced the current closest to the 0.02 A target (0.023 A) without exceeding practical design margins.

### Computational Approach (PPE Simulation)

The rectifier circuit is modeled in `Problem2.csv`, including `Ammeter_Isource`, `Resistor_Ramm`, `Diode_Dfilter`, `Inductor_Ifilter`, `Resistor_Rfilter`, `Capacitor_Cfilter`, `VoltageSource_Vsource`, `Resistor_Rsource`, and `Resistor_Rload`.

Key simulation parameters:

- `Capacitor_Cfilter` = 1 uF (1e-06 F).
- `Inductor` value = 1 mH (0.001 H).
- `Resistor` (internal/source) = 0.01 Ω.
- Diode and Voltmeter voltage level = 1000 V.
- Diode cathode direction: clockwise. Voltmeter positive voltage direction: counter-clockwise.
- `VoltageSource` peak voltage = V x sqrt(2) = 5 x sqrt(2) approx 7.071 V.

Simulation output graphs (in `output/`) show the voltage at each resistance and the current at each resistance, verifying the rectifier design.

## Simulation Files Explained

- **`Problem1.csv` / `Problem2.csv`**: Grid-based circuit topology definitions, where each cell represents a component (resistor, voltage source, ammeter, voltmeter, capacitor, inductor, diode) or a `wire` connection. This layout is read directly by the PPE simulation engine.
- **`Problem1.xlsx` / `Problem2.xlsx`**: Excel versions of the same circuit topology, used for easier visual editing before exporting to CSV.
- **`__control.py`**: The main PPE control script that parses the circuit topology, runs the simulation, and generates the output data and plots.
- **`ckt_output.dat`**: Raw simulation output data generated after running `__control.py`.
- **`plotkey.txt`**: Maps each column index in `ckt_output.dat` to its corresponding measurement (e.g., column 1 = time, column 2 = `Voltmeter_load1`, column 7 = `Ammeter_load1`), used for plotting and interpreting results.

## How to Run the Simulation

1. Ensure Python 3.7+ and the PPE (Python Power Electronics) library are installed.
2. Place the desired circuit topology file (`Problem1.csv` or `Problem2.csv`) in the working directory.
3. Run the control script:
   ```bash
   python __control.py
   ```
4. The script generates `ckt_output.dat`, which can be interpreted using the column mapping in `plotkey.txt` to plot voltage, current, and other measured parameters over time.

## Documentation

The full case study report, including the assessment form, numerical derivations, circuit topology diagrams, parameter tables, and simulation output graphs, is available in `docs/2602089143-JonathanAlvindoFernandi_AoL-CaseStudy.pdf`.

## Author

**Jonathan Alvindo Fernandi**  
Computer Science, School of Computer Science, Bina Nusantara University  
Course: SCIE6063001 – Introduction to Computational Physics
