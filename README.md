# Advanced-Physical-Design
--------------------------

"Advanced Physical Design" represents an intricate domain within digital integrated circuit engineering. It entails the intricate process of transforming a high-level logical description of an electronic circuit into a meticulously optimized layout, considering performance, power efficiency, and area utilization. OpenLANE, an open-source ASIC design flow, combined with the Sky130 Process Design Kit (PDK), has become a powerful toolkit for realizing this goal. Starting with RTL design, the process encompasses synthesis, floor planning, placement, clock tree synthesis, routing, physical verification, and timing closure. Advanced physical design goes a step further, incorporating sophisticated techniques such as custom library cell design and power network optimization. These tools and methodologies have empowered engineers and researchers to explore cutting-edge semiconductor design, enabling innovation in the development of complex integrated circuits.

## DAY-1: Inception of open-source EDA, OpenLANE and Sky130 PDK
<details> 
      <summary> How to talk to computers </summary>

---
At some part of our life, we have all used an ARDUINO board. We all know that an Arduino board is a popular open-source hardware platform designed for electronics enthusiasts, hobbyists, students, and professionals to create and prototype a wide range of embedded systems and electronic projects. Arduino boards are known for their ease of use and versatility, making them a valuable tool for learning about electronics and programming.

![arduino](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/2c4d152e-9a76-47c3-9631-0a137711f6b7)



The Block diagram of an ARDUINO board is as shown below.

---
![Arduinno_Block](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/c63c0919-4e07-4757-80e7-d0fafa25d769)

In this course, instead of looking into the embedded design we will be focusing more on the chip used inside the embedded systems.

---
![Chip](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/443610f2-ab16-41ce-b448-e2ee2bdf7dcc)

- **Package:** In chip design, a "package" refers to the protective outer casing that houses and safeguards the integrated circuit (IC). Packages serve critical roles in chip manufacturing by providing physical protection to the silicon die, establishing electrical connections between the chip and external components or a printed circuit board (PCB), aiding in thermal management by dissipating heat, and offering mechanical support. These packages come in various types, such as Dual In-line Packages (DIP), Surface-Mount Device (SMD) packages, Small Outline Integrated Circuit (SOIC) packages, and more, each tailored to specific applications and requirements, making package selection a crucial consideration in chip design and manufacturing.

In the example above, we have used QFN 48. The "QFN 48" package is a specific type of semiconductor package commonly used for integrated circuits (ICs). "QFN" stands for "Quad Flat No-Lead," and "48" refers to the number of pins or leads on the package. 

- **Chip:** A "chip," also known as an integrated circuit (IC) or microchip, is a miniature electronic device that consists of a collection of electronic components, such as transistors, resistors, and capacitors, etched onto a single semiconductor material, typically silicon. These components are interconnected to perform specific functions, such as processing data, storing information, or controlling electrical signals. Chips and packages can be connected through wire bonds in some packaging methods, but it's important to note that wire bonding is just one of several methods used for making electrical connections between the chip and the package.

The "chip" is the silicon-based microelectronic component that contains electronic circuits, while the "package" is the protective outer casing that houses the chip, provides electrical connections, and offers physical protection and thermal management.

---
![PADS_Die_Core](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/5fbbe595-d2ad-47bf-a0c8-3c4051a9119c)

- Pads: It refer to the input and output connection points on the chip's package that interface with the external world, such as a printed circuit board (PCB) or other devices. These pads serve as the electrical interfaces through which the SoC communicates with other components or systems.  

- Core: Core refers to a central processing unit (CPU) or a processing unit that performs computations and executes instructions. SoCs are highly integrated semiconductor devices that combine various components and subsystems on a single chip, and one of the critical components within an SoC is the processing core.

- Die: "Die" refers to the actual silicon chip or semiconductor wafer that contains all the integrated circuits and components of the SoC.

Now, let's take example of a  sample SOC using RISC-V as their ISA. 

---
![FOUNDRY_IP Macros](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/58b201ef-c8eb-42f7-a2df-b1d0dd33d7b5)

**Foundry IP's :** Foundry IP, short for Foundry Intellectual Property, refers to a set of pre-designed and pre-verified semiconductor intellectual property (IP) blocks or components that are licensed to semiconductor companies (fabless semiconductor companies) for integration into their own custom integrated circuits (ICs). These IP blocks are typically developed by semiconductor foundries or third-party IP providers and can be crucial for accelerating the design and production of complex chips. 

**Macro's:** Macros short for "macrocells" or "macro functions," refer to predefined and reusable functional blocks or components that can be incorporated into custom IC designs. Macros are a form of semiconductor intellectual property (IP) and play a crucial role in simplifying and speeding up the process of designing complex digital circuits. 


From Software Application to Hardware  
======================================

  In this section we will learn what exactly is the Instruction Set Architecture (ISA) role in a device and why it is required.  

![Screenshot from 2023-08-21 10-46-39](https://github.com/akul-star/RISC-V/assets/75561390/ae4ea0da-5b23-4771-90d3-4ef404471e51)

Let's explore how applications communicate with hardware components through various layers, including the operating system (OS), compiler, assembler, and a Register Transfer Language (RTL) snippet.

1. Operating System (OS):
    The operating system provides an abstraction layer between applications and hardware. It manages the hardware resources, such as memory, processors, and I/O devices, and provides services that applications can use. 

2. Compiler:
    The compiler translates high-level programming code written in languages like C, C++, or Java into machine code that the hardware can execute. During compilation, the compiler maps high-level code constructs to appropriate machine instructions. For instance, if an application contains a loop, the compiler generates machine instructions that correspond to looping constructs supported by the ISA (RISC-V in our case).

3. Assembler:
    An assembler converts assembly language code (a human-readable representation of machine code) into actual machine code. Assembly language is a low-level representation of the ISA, and each assembly instruction typically corresponds to a single machine instruction. Assemblers take care of translating assembly mnemonics into binary machine code that the hardware understands. The ISA acts as a abstract interface between the high level language like C, C++ and JAVA & the hardware.

4. RTL Snippet (Register Transfer Language):
RTL is a description of digital circuits using registers, data paths, and control logic. It's used in hardware design to describe the behavior of digital systems at a low level. 

</details>

<details>
      <summary> SoC design & OpenLane </summary>
      

Introduction to all components of open-source digital asic design 
=============================================================

![ASIC](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/2e769d73-b066-41fe-a0ed-8a158713cd4d)

1. **RTL Design:** RTL (Register-Transfer Level) design is essential for ASICs (Application-Specific Integrated Circuits) because it provides a hardware-specific description of the desired functionality, bridging the gap between high-level behavior and low-level gate-level implementation. It specifies how data is transferred between registers and processed by combinational logic, defines timing constraints, and serves as input to RTL synthesis tools for automatic conversion into gate-level representations. RTL design allows for optimization, simulation-based verification, portability, and clear documentation of the ASIC's design intent, ensuring a solid foundation for subsequent stages of ASIC development and ultimately delivering custom integrated circuits tailored to specific applications.

2. **EDA Tools:** ASIC (Application-Specific Integrated Circuit) design relies on Electronic Design Automation (EDA) tools because these tools provide the essential infrastructure for designing, verifying, and optimizing custom integrated circuits. EDA tools facilitate the creation of hardware descriptions, synthesis of high-level designs into manufacturable gate-level representations, simulation to ensure functionality and correctness, timing analysis for meeting critical performance requirements, and physical implementation to optimize layout and manufacturing. They streamline the complex ASIC design process, ensuring efficiency, accuracy, and successful production of application-specific integrated circuits tailored to specific functions and applications.

3. **PDK Data:** A Process Design Kit (PDK) for ASIC manufacturing is a comprehensive package provided by semiconductor foundries to ASIC designers. It contains vital information, design rules, device models, and a library of components necessary to design and fabricate custom integrated circuits. PDKs ensure that designers adhere to manufacturing guidelines, use accurate device models, and efficiently utilize foundry-specific processes during the ASIC design process, facilitating successful and manufacturable custom chip production.

**SkyWater 130nm Process Design Kit (PDK):** 
The SkyWater 130nm Process Design Kit (PDK) is a comprehensive set of resources offered by SkyWater Technology Foundry for integrated circuit designers. It encompasses essential information about the 130-nanometer semiconductor manufacturing process, design rules, device models, a library of components, and technology files. This PDK enables designers to create custom integrated circuits tailored to specific applications using SkyWater's 130nm process technology, promoting accessibility and cost-effective semiconductor fabrication.

**RIL Design Flow (RTL to GDS2):**
The RTL (Register-Transfer Level) to GDS2 design flow is the process of creating and manufacturing integrated circuits (ICs). It involves steps like designing the circuit's functionality in RTL, simulating and synthesizing it into gate-level logic, creating a physical layout, verifying the design, generating manufacturing masks, fabricating the ICs, and finally, testing and packaging them. The GDS2 file is generated to describe the layout and is used for manufacturing. This flow ensures that ICs meet specifications and can be mass-produced.

Simplified RTL to GDSII Flow
=============================

![Openlane_ASICflow](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/4a23a0b6-776c-42e0-ae25-eab6d2638929)

1. **Synthesis:** In the RTL to GDS2 flow, synthesis is a crucial step where RTL (Register Transfer Level) code is transformed into a gate-level netlist. This process involves mapping RTL constructs to standard cell libraries, optimizing the design for area, power, and timing, handling clock domains, and applying timing constraints. Static Timing Analysis (STA) is then performed to ensure that the design meets timing requirements. Once synthesis is complete, the synthesized design serves as the foundation for subsequent steps in the flow, including physical design, placement, routing, and ultimately the generation of GDS2 files for fabrication. This synthesis stage plays a pivotal role in achieving a balance between design functionality and performance while preparing the design for manufacturing.

    A. **Liberty View (Liberty Format):**
        Purpose: Liberty view is used primarily for static timing analysis (STA) during the synthesis process. It provides information about the timing characteristics of standard cells from the cell library, such as setup times, hold times, rise/fall times, and capacitance values.
        Contents: It includes timing constraints, delay information, and other timing-related data for the cells in the standard cell library.
        Format: Typically written in a standard format called Liberty (.lib) format, which can be read by synthesis tools and STA tools.

    B. **HDL Behavioral View:**
        Purpose: The HDL (Hardware Description Language) Behavioral View represents the high-level description of the digital design in RTL (Register Transfer Level) or a higher abstraction level. It's the original RTL code created by designers.
        Contents: It contains behavioral descriptions of the logic functions, data paths, control structures, and the intended functionality of the design.
        Format: The format depends on the hardware description language used, such as VHDL or Verilog.

    C. **SPICE View (Simulation View):**
        Purpose: SPICE (Simulation Program with Integrated Circuit Emphasis) View is used for detailed transistor-level simulation. It provides a transistor-level representation of the design and is essential for accurate circuit-level simulations.
        Contents: SPICE View includes transistor-level models, parasitic elements, and detailed information about how the gates and interconnections in the design are implemented at the transistor level.
        Format: Typically written in a SPICE-compatible format (e.g., SPICE netlists) that can be used by circuit simulators for accurate transistor-level simulations.

2. **Floor Planning:** Floor planning in the RTL to GDS2 (GDSII) flow is the initial step of physical design. It involves allocating space and defining the approximate locations of major components and functional blocks on the semiconductor chip. The goal is to create a layout that meets area, power, and performance targets while ensuring that signal routing between these blocks is feasible. Floor planning sets the foundation for subsequent steps like placement and routing and plays a crucial role in achieving a successful chip design.

   A. **Chip Floor Planning:**
        Purpose: Chip floor planning is the high-level organization of the entire semiconductor chip. It defines the placement of major components and functional blocks on the chip's silicon die.
        Scope: It encompasses decisions related to core logic placement, I/O ring location, clock distribution, and other global aspects of the chip's physical design.
        Goals: The primary goals of chip floor planning are to optimize chip area, minimize power consumption, and ensure that the chip meets its performance requirements. It provides a high-level view of how different parts of the chip will interact.

   B. **Macro Floorplanning:**
        Purpose: Macro floorplanning focuses on the placement and organization of large functional blocks or macros within the chip. These macros can include CPU cores, memory blocks, or other complex IP blocks.
        Scope: It deals with the internal layout and arrangement of these macros and how they interface with each other and the rest of the chip.
        Goals: The main objectives of macro floorplanning are efficient use of space, ensuring proper connectivity between macros, and optimizing for performance and power within the macro boundaries.

   C. **Power Planning:**
        Purpose: Power planning is a critical aspect of chip design that focuses on managing and distributing power throughout the chip. It ensures that each component receives the required power supply and that power delivery is efficient to minimize voltage drop and power dissipation.
        Scope: Power planning involves decisions about the placement of power grid elements (such as power rails and decoupling capacitors) and the routing of power distribution networks.
        Goals: The key goals of power planning are to maintain voltage stability, reduce power noise, and meet power delivery requirements, all while minimizing the impact on chip area and performance. Effective power planning is essential for reliable chip operation and to avoid voltage drop-related issues.

3. **Cell Placement:** Cell placement is a crucial step in the physical design of integrated circuits (ICs) within the RTL to GDS2 (GDSII) flow. It involves determining the specific locations on a semiconductor chip's silicon die where individual standard cells, macros, and other functional blocks will be positioned. 

   A. **Chip Floor Planning:**
        Purpose: Chip floor planning is the high-level organization of the entire semiconductor chip. It defines the placement of major components and functional blocks on the chip's silicon die.
        Scope: It encompasses decisions related to core logic placement, I/O ring location, clock distribution, and other global aspects of the chip's physical design.
        Goals: The primary goals of chip floor planning are to optimize chip area, minimize power consumption, and ensure that the chip meets its performance requirements. It provides a high-level view of how different parts of the chip will interact.

   B. **Macro Floorplanning:**
        Purpose: Macro floorplanning focuses on the placement and organization of large functional blocks or macros within the chip. These macros can include CPU cores, memory blocks, or other complex IP blocks.
        Scope: It deals with the internal layout and arrangement of these macros and how they interface with each other and the rest of the chip.
        Goals: The main objectives of macro floorplanning are efficient use of space, ensuring proper connectivity between macros, and optimizing for performance and power within the macro boundaries.

   C. **Power Planning:**
        Purpose: Power planning is a critical aspect of chip design that focuses on managing and distributing power throughout the chip. It ensures that each component receives the required power supply and that power delivery is efficient to minimize voltage drop and power dissipation.
        Scope: Power planning involves decisions about the placement of power grid elements (such as power rails and decoupling capacitors) and the routing of power distribution networks.
        Goals: The key goals of power planning are to maintain voltage stability, reduce power noise, and meet power delivery requirements, all while minimizing the impact on chip area and performance. Effective power planning is essential for reliable chip operation and to avoid voltage drop-related issues.

4. **Clock Tree Sysnthesis:** CTS stands for "Clock Tree Synthesis." It is a crucial step in the physical design of integrated circuits, particularly digital designs, within the RTL to GDS2 (GDSII) flow. The primary goal of CTS is to create an efficient and optimized network of clock distribution paths throughout the chip.

5. **Routing:** Routing, in the context of semiconductor chip design within the RTL to GDS2 (GDSII) flow, refers to the process of establishing physical connections between different components, such as standard cells, macros, and input/output pads, on the silicon die. These connections are created using metal layers, which serve as interconnects to facilitate data transmission and signal propagation. Grid routers are a type of routing algorithm used in semiconductor chip design within the context of the RTL to GDS2 (GDSII) flow. These routers are designed to navigate and establish connections between components on a chip layout using a grid-based approach. Grid routers are especially suitable for digital integrated circuits with a regular and structured layout, where the chip design is aligned with a grid pattern.

      A. **Global Routing:**  
Global routing is the initial phase of routing in chip design. It determines high-level routing paths for nets between macroblocks or functional units on the chip, focusing on channel assignments and chip-level optimization. The outcome is a routing framework or guides for subsequent detailed routing.

      B. **Detailed Routing:**
Detailed routing follows global routing and defines precise paths for individual wires within nets. It works at a lower, detailed level, considering cell positions, design rules, and minimizing wirelength. The result is the completed layout of physical interconnections, adhering to global routing guidelines.

7. **Sign-Off:**

      A. **Physical Verification:** Physical verification is a critical step in the semiconductor chip design process, specifically in the RTL to GDS2 (GDSII) flow. It involves a series of checks and analyses to ensure that the physical layout of the chip adheres to design rules, manufacturing constraints, and reliability criteria. Physical verification helps identify and rectify potential issues in the layout that could lead to manufacturing defects, performance problems, or reliability issues.
      
      1. LVS (Layout vs. Schematic) : It is a crucial step in semiconductor manufacturing that compares the physical layout of semiconductor components on a chip to the intended circuit schematic. Its primary purpose is to ensure that the physical design matches the expected design, verifying that connections are correct, there are no short circuits or open circuits, and component dimensions are within tolerances. If discrepancies are found, they are corrected to ensure the chip can be manufactured and will function correctly. LVS helps catch errors early in the design process, ensuring high-quality semiconductor products and reducing manufacturing costs. 

      2. Design Rule Check (DRC): It's an essential step in semiconductor design and manufacturing that verifies if the physical layout of integrated circuits adheres to specific design rules and manufacturing guidelines. It identifies violations, such as inadequate spacing, feature size deviations, or unintended connections, and prompts designers to correct them to ensure that the chip can be manufactured reliably with fewer defects, leading to better-quality electronic devices.
     
     B. **Timing Verification:** Timing verification is a critical step in semiconductor chip design within the RTL to GDS2 (GDSII) flow. It focuses on ensuring that the design meets its timing requirements, particularly in terms of clock-to-q delays, setup times, hold times, and maximum clock frequency. Timing verification helps guarantee that the chip will operate correctly and within its specified performance limits.

OpenLANE
=========

OpenLane is an open-source toolchain for chip design that automates the process of creating custom digital integrated circuits, from high-level RTL code to manufacturable GDSII files. Developed by efabless, it streamlines the design flow by integrating various open-source EDA tools, allowing users to explore different design options, meet manufacturing requirements, and even experiment with custom chip designs. OpenLane's scripted flow, community-driven development, and accessibility make it a valuable resource for both educational purposes and small design teams looking to create custom ASICs while adhering to industry best practices.

OpenLANE ASIC Flow
=================

OpenLane is a fully automated process, spanning from RTL (Register-Transfer Level) to GDSII (Graphics Data System II), and relies on various components, including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, KLayout, and a set of specialized scripts for design exploration and enhancement. This comprehensive flow covers every step of ASIC implementation.

OpenLANE utilises a variety of opensource tools in the execution of the ASIC flow:

1. RTL Synthesis & Technology Mapping: yosys, abc
2. Floorplan & PDN : init_fp, ioPlacer, pdn and tapcell
3. Placement : RePLace, Resizer, OpenPhySyn & OpenDP
4. Static Timing Analysis : OpenSTA
5. Clock Tree Synthesis : TritonCTS
6. Routing : FastRoute and TritonRoute
7. SPEF Extraction : SPEF-Extractor
8. DRC Checks, GDSII Streaming out : Magic, Klayout
9. LVS check : Netgen
10. Circuit validity checker : CVC

</details>


<details>
      <summary> Open Source EDA Tools </summary>

OpenLANE Installation
====================

Prior to the installation of the OpenLane install the dependencies and packages using the command shown below :

```
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git
```

**Commands to install Docker:**
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world

sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 


# Check for installation
sudo docker run hello-world
```

**Steps to install OpenLane, PDKs and Tools:**

```
cd $HOME
git clone https://github.com/The-OpenROAD-Project/OpenLane --recurse-submodules 
cd OpenLane
make
make test
cd /home/kanish/OpenLane/designs/ci
cp -r * ../
```


**Invoking OpenLANE**

```
cd OpenLane
make mount
```

Inside the openlane container:
```
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
```
![Picorv32](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/50c57c6b-3c65-4334-8de9-91197deea5bf)

![picorv32_synth](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/62d0c362-bbca-492c-b6d6-ad821dd3e666)

The netlist generated is shown below:
```
cd OpenLane/designs/picorv32a/runs/RUN_2023.09.10_07.47.37/results/synthesis/
gvim picorv32.v
```
To view report:
```
cd OpenLane/designs/picorv32a/runs/RUN_2023.09.10_07.47.37/reports/synthesis/
gvim 1-synthesis.AREA_0.stat.rpt
```
```
Flop ratio = Number of D Flip flops = 1596  = 0.1579
             ______________________   _____
             Total Number of cells    10104
```
</details>


## Day-2: Good floorplan vs bad floorplan and introduction to library cells

<details> 
      <summary> Chip Floor-Planning Consideration </summary>

---

Let's start with a netlist that defines a basic circuit as shown below.

![Basic_Netlist_Ex](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/b68db8ea-cfe1-45b4-8e11-75e745d433b3)
The circuit has 4 standard cell's comprising of Flip-Flop and Gate's. Let's assume each standard cell has area of 1 unit square. So, not assuming the connecting wires that will also take some area inside the core, we can say the total area consumed by the circuit will be 4 unit square. If core also has the area of 4 unit square, then the utilization factor will be 100%.

---
![Utilization factor](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/d89051c2-b419-4650-a993-b02864f85b9a)

We will look into two parameters, Utilization factor and Aspect ratio, but before that we must look into the important terms in chip design.

- Die : It is a small semiconductor material specimen that houses the core and the fundamental circuit is fabricated over this.
- Core : It is the section of the chip where the fundamental design is placed.

**Utilisation Factor**


The ratio of area occupied by the cells in the netlist to the total area of the core.
Best practice is to set the utilisation factor less than 50% to 60% so that there will be space for optimisations, routing, inserting buffers etc.,

**Aspect Ratio**

- Aspect ratio is the ratio of height to the width of the die.
- Aspect Ratio of 1 indicates that the die is a square die.
- These two Parameters are important to derive the width and height of the core and die, and now we can move ahead to define the location of preplaces cells.

**Pre-placed Cells**

---
![PrePlacedcells](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/b101985f-2105-4564-932a-dc1b63d9d28e)

- Whenever there is a complex logic which is repeated multiple times or a design given by a third-party it can be perceived as abstract black box with input and output ports, clocks etc. We can also create black boxes ourselves for the design in case as per the requirements. They can be IPs or Macros. 
- These Macros and IPs are placed in the core at first before placing the standard cells and power planning. They are placed before automated placement and routing and are called as pre-placed cells. These are optimally such that the cells which are more connected to each other are placed nearby and oriented for input and ouputs.
- Once they have been placed, the location are not altered later on for routing. Thus they have been fixed on the chip. These pre-placed cells have to be surrounded with de-coupling capacitors.


**De-coupling Capacitors**

- The resistances and capacitances associated with long wire lengths can cause the power supply voltage to drop significantly before reaching the logic circuits. This can lead to the signal value entering into the undefined region, outside the noise margin range. (Noise margin in RTL or Resistor-Transistor Logic refers to the amount of tolerance a digital logic circuit has to withstand noise or voltage fluctuations while still correctly interpreting logical "0" and "1" states. It is a critical parameter in digital circuit design to ensure the robustness and reliability of logic operations.)
- De-coupling capacitors are huge capacitors charged to power supply voltage and placed close the logic circuit. Their role is to decouple the circuit from power supply by supplying the necessary amount of current to the circuit. They pervent crosstalk and enable local communication.
- The decoupled capacitor will be connected to all the pre-placed cells in the core, as shown below.

  
  ![DecoupledCAP](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/2abf75dc-c96d-4ea5-914f-47091bf35f23)


**Power Planning**

- Each block on the chip, however, cannot have its own decoupled capacitor unlike the pre-placed cells. Thus, when multiple units are discharging, we observe a ground bumb and in case of multiple charing units, we see a voltage droop.
- Ground bounce is a phenomenon that can occur on an N-bit bus in digital electronic circuits, particularly when many components are switching simultaneously. It's a transient voltage fluctuation on the ground (or ground reference) of the circuit. Ground bounce is typically associated with noise or voltage fluctuations that affect the reliability of digital signals. Below is shown a 16bit bus which experiences a ground bounce when all the bits are discharging.
  
  ![Ground bounce](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/50eaf219-e1b8-4952-b1fb-643c2db5c4ec)

- A similar phenomena that we observe is Voltage droop on an N-bit bus, also known as "bus voltage droop" or simply "voltage droop," which is a phenomenon that can occur in digital electronic circuits when there is a momentary decrease in voltage on a multi-bit data bus during high-speed or simultaneous switching of components.
- When these are under noise range designed, we won't face any issue, but if they get beyond the defined noise range, we experience undesired behaviour from the design.
- To fix this issue, we will go for a better power plan for the chip, such that each unit can use the Vdd and Gnd near to it.
- A common way to accomplish this is to have VDD and VSS pads connected to the horizontal and vertical power and GND lines which form a power mesh. A "power mesh" refers to a network of metal or wire traces that distribute power (VDD) and ground (VSS) throughout the integrated circuit (IC). The primary purpose of the power mesh is to ensure a stable and uniform distribution of power and ground across the entire chip, which is crucial for proper functionality and reliability.

**Pin Placement**

The input, output and Clock pins are placed optimally such that there is less complication in routing or optimised delay. 

**Note -** CLK needs least resistive path, as they provide signals to all the flops continuously, thus have bigger IO ports.
There are different styles of pin placement in openlane like random pin placement, uniformly spaced etc.,

**Run Floorplan on OpenLane**

Importance files in increasing priority order:

- floorplan.tcl - System default envrionment variables present in the configuration folder.
- conifg.tcl - Present in the deign folder.
- sky130A_sky130_fd_sc_hd_config.tcl - Present in the design folder.

**Floorplan envrionment variables or switches:**

1. FP_CORE_UTIL - floorplan core utilisation
2. FP_ASPECT_RATIO - floorplan aspect ratio
3. FP_CORE_MARGIN - Core to die margin area
4. FP_IO_MODE - defines pin configurations (1 = equidistant/0 = not equidistant)
5. FP_CORE_VMETAL - vertical metal layer
6. FP_CORE_HMETAL - horizontal metal layer

Now, we will look into how to generate the floorplan using OpenLane.
```
run_floorplan
```

![run_floorplan](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/ddfb5040-aaec-4586-ac6f-50980092f017)

 - We may review floorplan files by checking the floorplan.tcl. The system defaults will have been overriden by switches set in conifg.tcl and further overriden by switches set in sky130A_sky130_fd_sc_hd_config.tcl.

 - Post the floorplan run, a .def file (design exchange format file) will have been created within the results/floorplan directory. It has the various informations such as the die area and unit lenghts used.

![floorplan_def_file](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/9797f91f-a50c-4e08-a5c4-64b65837f139)

Viewing the floorplan using MAGIC:

```
 magic -T ~/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32.def &
```
![MAGIC_Floorplan](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/fef2c7ca-582c-454e-bbfe-96441142fab0)



</details>
