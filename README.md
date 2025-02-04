# VSD SoC Design Program - Day 1

## **Day 1: Inception of Open-Source EDA, OpenLANE, and Sky130 PDK**

Welcome to Day 1 of the VSD SoC Design and Physical Design using OpenLANE workshop. On this day, we focus on understanding the basics of open-source EDA tools and setting up the environment to synthesize the `picorv32a` design.

---

### **Theory and Background**

#### **What is OpenLANE?**
OpenLANE is a fully automated RTL to GDSII flow that leverages several open-source EDA tools to create application-specific integrated circuits (ASICs). It integrates tools for synthesis, floorplanning, placement, clock tree synthesis (CTS), and routing, among others.

#### **Sky130 PDK**
Sky130 PDK is an open-source Process Design Kit (PDK) provided by SkyWater Technology. It allows designers to develop and fabricate ASICs using a 130nm technology node.

---

### **Key Objectives for Day 1:**

1. Introduction to the OpenLANE and Sky130 environment.
2. Setting up the Docker container for OpenLANE.
3. Synthesizing the `picorv32a` design.
4. Flop ratio and DFF percentage calculation.

---

## **Step-by-Step Process**

### **1. Launch OpenLANE Interactive Mode**

1. Open a terminal and navigate to the OpenLANE directory:

    ```bash
    cd ~/Desktop/work/tools/openlane_working_dir/openlane
    ```

2. Launch the Docker container by using the aliased command:

    ```bash
    docker
    ```

3. Check the current directory within the Docker container:

    ```bash
    pwd
    ```

4. Run OpenLANE in interactive mode:

    ```bash
    ./flow.tcl -interactive
    ```

---
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

### **2. Load OpenLANE Package and Prepare the Design**

1. Ensure OpenLANE is initialized with the required package:

    ```tcl
    package require openlane 0.9
    ```

2. Set up the necessary files and directories for the `picorv32a` design:

    ```tcl
    prep -design picorv32a
    ```

> **Note:** After preparation, OpenLANE creates directories like `runs`, `results`, and `logs` for design files and reports.

---

### **3. Run Synthesis**

Start synthesis using the following command:

```tcl
run_synthesis
```

> OpenLANE will perform logic synthesis and static timing analysis (STA). This step converts RTL code into a gate-level netlist and optimizes it for area, performance, and power.

---

### **4. Calculate Flop Ratio and DFF Percentage**

**Definition:** The flop ratio is the ratio of D flip-flops (DFFs) to the total number of cells in a design.

- Total number of D flip-flops: `1613`
- Total number of cells: `14876`

**Calculation:**

```math
Flop Ratio = \frac{1613}{14876} \approx 0.1084
```

**DFF Percentage:**

```math
DFF Percentage = 0.1084 \times 100 \approx 10.84%
```

---

## **Key Takeaways from Day 1:**

- Familiarized with OpenLANE and Sky130 PDK.
- Successfully synthesized the `picorv32a` design.
- Calculated the flop ratio and DFF percentage.
- Gained insights into the importance of logic synthesis and STA.
# VSD SoC Design Program - Day 2

## **Day 2: Good Floorplan vs Bad Floorplan and Introduction to Library Cells**

On Day 2, we delve into defining and reviewing the floorplan of the `picorv32a` design, calculating the die area, and performing congestion-aware placement. We also explore the importance of using library cells and review the floorplan in the Magic tool.

---

### **Theory and Background**

#### **What is Floorplanning?**
Floorplanning is the process of defining the physical layout of an ASIC design. It involves placing functional blocks, allocating spaces for routing channels, and determining the overall chip dimensions. Good floorplanning ensures optimal placement and reduces design congestion.

#### **Library Cells**
Library cells are pre-designed and verified components used in ASIC design. They include logic gates, flip-flops, and other basic components. Using library cells ensures consistency, reduces design time, and improves performance.

---

### **Key Objectives for Day 2:**

1. Define the floorplan using OpenLANE.
2. Calculate the die area based on the floorplan output.
3. Review the floorplan in Magic.
4. Perform congestion-aware placement.

---

## **Step-by-Step Process**

### **1. Define the Floorplan**

Run the floorplan utility from OpenLANE to define the floorplan of the `picorv32a` design:

```bash
run_floorplan
```

> This command initializes the floorplan by placing the functional blocks and routing channels.

---

### **2. Calculate the Die Area**

After successful completion of the floorplan command, navigate to the results folder and inspect the file `picorv32a.floorplan.def`.

#### **Given Data:**
- **Distance Units per Micron:** 1000
- **Die Width in Distance Units:** `660685`
- **Die Height in Distance Units:** `671405`

#### **Die Area Calculation:**

```math
Die Width = \frac{660685}{1000} \approx 660.685 \text{ microns}
```
```math
Die Height = \frac{671405}{1000} \approx 671.405 \text{ microns}
```
```math
Die Area = 660.685 \times 671.405 \approx 443587.21 \text{ square microns}
```

---

### **3. Review the Floorplan in Magic**

To visualize the floorplan using the Magic tool, execute the following commands:

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/<timestamp>/results/floorplan
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

> Use `s` to select sections for zoom and `z` to zoom in within Magic.

---

### **4. Perform Congestion-Aware Placement**

Run the placement utility in OpenLANE using:

```bash
run_placement
```

> This step legally places standard cells within defined rows while considering congestion.

---

## **Key Takeaways from Day 2:**

- Defined the floorplan and calculated the die area.
- Visualized and reviewed the floorplan in Magic.
- Performed congestion-aware placement for better design optimization.
- Understood the importance of library cells in ASIC design.
# VSD SoC Design Program - Day 3

## **Day 3: Design Library Cell Using Magic Layout and ngspice Characterization**

On Day 3, we focus on designing a CMOS inverter standard cell using Magic Layout and characterizing it using ngspice. This session involves understanding different placement modes in Magic, extracting SPICE files, and performing CMOS inverter characterization.

---

### **Theory and Background**

#### **What is a CMOS Inverter?**
A CMOS inverter is a basic logic gate that converts an input logic level to its opposite output logic level. It consists of a p-channel and an n-channel MOSFET connected in a complementary manner. The main advantage of CMOS technology is its low power consumption, as power is only consumed during switching events.

**Operating Principle:**
- When the input is low, the PMOS transistor is on, and the NMOS transistor is off. This drives the output to a high voltage (logic 1).
- When the input is high, the PMOS transistor is off, and the NMOS transistor is on. This drives the output to a low voltage (logic 0).

#### **Magic Tool for Layout Design**
Magic is an open-source VLSI layout tool that supports physical design and verification. It allows designers to create, visualize, and validate circuit layouts. Magic provides an interactive interface for designing layouts and checking for design rule violations (DRCs).

#### **SPICE and Circuit Simulation**
SPICE (Simulation Program with Integrated Circuit Emphasis) is a simulation tool used for analyzing analog and digital circuits. It provides insights into the circuit's performance, including propagation delays, power consumption, and signal integrity.

#### **Importance of Characterization**
Characterization helps determine the dynamic and static properties of a circuit. For the CMOS inverter, it involves analyzing rise/fall times, propagation delays, and power dissipation, which are crucial for ensuring optimal performance in complex digital circuits.

---

### **Key Objectives for Day 3:**

1. Observe different placement modes in Magic.
2. Clone and explore a CMOS inverter layout from a repository.
3. Extract SPICE files from Magic.
4. Perform CMOS inverter characterization using ngspice.

---

## **Step-by-Step Process**

### **1. Observe Different Placement Modes in Magic**

1. Open Magic and set the placement mode using the `FP_IO_MODE` variable:

   ```bash
   % run_floorplan
   ```

2. Observe the placement of IO pins around the bottom left-hand corner.

---

### **2. Clone CMOS Inverter Standard Cell Design**

1. Navigate to the OpenLANE working directory:

   ```bash
   cd Desktop/work/tools/openlane_working_dir/openlane
   ```

2. Clone the CMOS inverter design repository:

   ```bash
   git clone https://github.com/nickson-jose/vsdstdcelldesign.git
   ```

3. Verify the cloned files using:

   ```bash
   ls -ltr
   ```

---

### **3. Explore the Inverter Layout in Magic**

1. Navigate to the cloned directory and copy the Sky130 technology file:

   ```bash
   cd ../../pdks/sky130/A/libs.tech/magic/
   cp sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/
   ```

2. Open the CMOS inverter layout in Magic:

   ```bash
   cd /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/
   magic -T sky130A.tech sky_inv.mag &
   ```

> **Note:** Check the connectivity of design nodes by selecting them in Magic.

---

### **4. Extract SPICE Files from Magic**

1. From the tkcon window in Magic, run the following commands:

   ```bash
   % extract all
   % ext2spice cthresh 0 rthresh 0
   ```

2. Verify that the SPICE file has been created:

   ```bash
   ls -ltr
   ```

---

### **5. Perform CMOS Inverter Characterization with ngspice**

1. Run the SPICE simulation:

   ```bash
   ngspice sky130_inv.spice
   ```

2. Plot the output signals using:

   ```bash
   ngspice 1 -> plt y vs time a
   ```

#### **Rise Time Calculation:**
The rise time is the difference between the time instant when the output signal reaches 80% of VPWR and when it reaches 20% of VPWR.

#### **Rise Propagation Delay Calculation:**
The rise propagation delay is the time difference between the input and output signals at 50% of VPWR.

---

## **Key Takeaways from Day 3:**

- Explored placement modes in Magic.
- Successfully cloned and explored a CMOS inverter layout.
- Extracted SPICE files from Magic.
- Characterized the CMOS inverter using ngspice.
# **VSD SoC Design Program - Day 4**

## **Pre-layout Timing Analysis and Importance of a Good Clock Tree**

Welcome to **Day 4** of the **VSD SoC Design Program**. On this day, we will focus on **pre-layout timing analysis**, the significance of a well-structured clock tree, and running **Clock Tree Synthesis (CTS)** using **TritonCTS**.

---

## **Theory and Background**

### **What is Timing Analysis?**
Timing analysis is a critical step in ASIC design that ensures the design meets timing constraints. There are two major types of timing analysis:

1. **Pre-Layout Timing Analysis**: Conducted **before** placement and routing, using ideal clocks.
2. **Post-Layout Timing Analysis**: Conducted **after** placement and routing, considering actual wire delays.

### **Clock Tree Synthesis (CTS)**
CTS is essential for distributing the clock signal uniformly to all sequential elements, minimizing **clock skew** and **latency**.

- **Clock Skew**: The difference in arrival times of the clock signal at different flip-flops.
- **Clock Latency**: The time taken by the clock signal to reach a flip-flop from the clock source.

CTS helps balance clock distribution by inserting **buffers** and **inverters** to achieve better synchronization【95:1†git hub.docx】.

---

## **Objectives for Day 4**
1. **Integrate a custom inverter cell** into the OpenLANE flow.
2. **Perform timing analysis using ideal clocks** with OpenSTA.
3. **Run Clock Tree Synthesis (CTS)** using TritonCTS.
4. **Perform timing analysis using real clocks** with OpenSTA after CTS【95:15†git hub 2.docx】.

---

## **Step-by-Step Implementation**

### **1. Integrate Custom Inverter Cell into OpenLANE**

Ensure your custom inverter cell meets these conditions:
- **Input/Output ports** should align with standard cell grid tracks.
- **Standard cell width and height** should be multiples of the routing pitch【95:15†git hub 2.docx】.

#### **Steps to Open Custom Inverter in Magic**
```bash
cd ~/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
magic -T sky130A.tech sky130_inv.mag &
```

Verify the grid settings in Magic:
```tcl
grid 0.46um 0.34um 0.23um 0.17um
```

#### **Save and Export the Custom Inverter Layout**
```tcl
save sky130_vsdinv.mag
lef write
```

---

### **2. Timing Analysis Using Ideal Clocks (Pre-CTS)**

Perform **Static Timing Analysis (STA)** to identify setup and hold time violations【95:1†git hub.docx】.

```bash
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
report_checks -path_delay min_max -fields {slew trans net cap input_pin}
report_tns
report_wns
```

---

### **3. Run Clock Tree Synthesis (CTS) with TritonCTS**

```tcl
run_cts
```

Inspect the clock tree buffers added to reduce clock skew:

```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech \
  lef read ../../tmp/merged.lef def read picorv32a.cts.def &
```

---

### **4. Perform Timing Analysis After CTS**

Re-run **STA** after CTS to ensure clock buffers and net delays are correctly analyzed【95:14†git hub 2.docx】.

```tcl
report_checks -path_delay min_max -fields {slew trans net cap input_pin}
report_tns
report_wns
```

---

## **Key Takeaways from Day 4**
- Understood **pre-layout and post-layout timing analysis**.
- Learned the importance of **Clock Tree Synthesis (CTS)**.
- Ran **STA before and after CTS** to analyze setup/hold time violations.
- Successfully implemented **clock tree buffering** using TritonCTS【95:5†git hub.docx】.








