# nasscom-vsd-soc-design-program-K
# Section 1 - Inception of Open-Source EDA, OpenLANE, and Sky130 PDK (DAY-1)

## Introduction to ASIC Design

An **ASIC (Application-Specific Integrated Circuit)** is a type of integrated circuit designed for a specific function or a set of functions, making it more efficient and tailored to the application than general-purpose ICs. ASICs are often used in embedded systems, telecommunications, automotive, and consumer electronics due to their optimized performance, power consumption, and size. 

The three main types of ASICs are:

- **Full-Custom ASICs**: These are designed from scratch, offering the highest performance but requiring significant time, expertise, and cost.
- **Semi-Custom ASICs**: These utilize pre-designed blocks (IP cores) and integrate them with custom-designed logic, balancing performance and development effort.
- **Programmable ASICs (FPGAs)**: These chips are reconfigurable, offering flexibility but typically at a lower level of performance compared to full-custom designs.

## ASIC Design Flow

The **ASIC design flow** is a multi-step process that can be categorized into **Front-End** and **Back-End** stages:

### Front-End Design

- **Specification and Architecture**: Defining the functionality and requirements of the ASIC.
- **RTL Design**: Writing Register Transfer Level (RTL) code (usually in Verilog or VHDL) to describe the design.
- **Simulation**: Verifying the functionality of the RTL design using simulation tools like ModelSim.
- **Synthesis**: Converting RTL code to a gate-level netlist using synthesis tools like Yosys or Synopsys.

### Back-End Design

- **Static Timing Analysis (STA)**: Ensuring the design meets timing requirements.
- **Floorplanning**: Defining the physical layout of functional blocks and power distribution.
- **Placement and Routing**: Determining the optimal placement of cells and routing connections between them using tools like OpenLANE.
- **Physical Verification**: Performing checks like DRC (Design Rule Check) and LVS (Layout vs. Schematic) to ensure the design is manufacturable.
- **GDSII Generation**: Producing the final GDSII file for fabrication.

## OpenLANE and Its Role in ASIC Design

**OpenLANE** is an open-source **RTL-to-GDSII flow** designed for ASIC design automation. It is optimized for the **SkyWater 130nm PDK** and integrates a variety of open-source tools to automate the key steps in the ASIC design process. 

### Tools Integrated in OpenLANE

- **Yosys**: Used for RTL synthesis to convert Verilog or VHDL code into a gate-level netlist.
- **ABC**: Performs technology mapping to optimize the design for area, power, or timing.
- **OpenSTA**: Used for static timing analysis (STA) to ensure that the design meets timing requirements.
- **Magic & KLayout**: Used for physical verification (DRC, LVS, and antenna checks).
- **TritonCTS**: Handles clock tree synthesis (CTS) to reduce clock skew and latency.
- **FastRoute and TritonRoute**: Used for global and detailed routing.

The primary goal of OpenLANE is to automate the entire flow from RTL design to GDSII generation without human intervention, though it also supports an interactive mode for more granular control.

## Structure of OpenLANE

The **OpenLANE directory structure** includes the following key components:

- **`designs/`**: Contains design-specific directories. Each design (e.g., `picorv32a`) has its own folder with Verilog files, SDC files (for timing constraints), and configuration files.
- **`pdks/`**: Stores Process Design Kit (PDK) files. For SkyWater 130nm, this includes:
  - `skywater-pdk/`: Contains the necessary files for the SkyWater PDK.
  - `open-pdks/`: Scripts to make commercial PDKs compatible with open-source tools.
  - `sky130A/`: The specific PDK variant for OpenLANE compatibility, containing libraries and technology files.
- **`tools/`**: Contains the EDA tools required for synthesis, placement, routing, and verification.
- **`run/`**: Output directory where tool logs, reports, and results are stored after each flow step. Each design has its own `run` folder for easy tracking of progress and results.
