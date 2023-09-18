 Advanced_Physical_Design_using_OpenLANE-Sky130
================================================

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

<details> 
     <summary> Library Binding & Placement </summary>
---
First and foremost, we need to bind the netlist with physical cells. We have shapes for OR, AND and every cell for pratice purpose. But in reality we dont have such shapes, we have give an physical dimensions like rectangles or squares weight and width and also different flavours of the same standard cell. This information is given in libs and lefs. Now we place these cells in our design by initilaising it. Now we look into Placement and its optimisation. 
      
Now we look into Placement and its optimisation.

---
![Optimized_Placement](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/f298ebcf-86c8-4374-a124-3d84b96a9bd6)

As you can see, the cells are placed such that the data input and output pins are as close as possible to reduce the resistance of the connecting wires so that noise error will not occur. In some cases, their might not be a way to place the cells close to their data pins. To avoid the noise margin issue in the longer connecting wires, we will use Repeaters or Buffers for the signal integrity so that the logic is not compromised. In the above design, their are few abutted cells which will have near to no delayand this is called as abuttment.

![Steps](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/4f92afb1-f313-4e85-9e89-6a1750bde004)

---
**Library Charaterization:** Library characterization is the process of characterizing electronic components and gates, such as logic gates, flip-flops, and other building blocks, to create models that accurately represent their behavior under various conditions. This characterization provides information about how components respond to different inputs, delays, power consumption, and more.

**Library modeling:** Library modeling involves creating mathematical or algorithmic representations of the behavior and characteristics of components. These models are used by EDA tools to simulate, analyze, and optimize digital circuits during the design phase.

PLACEMENT
==========

**Legalization:** In the context of detailed placement in digital integrated circuit (IC) design, "legalization" refers to the process of ensuring that the locations and orientations of individual standard cells (or logic gates) meet certain design rules, constraints, and physical requirements. The main goal of legalization is to transform an initial placement of cells into a valid placement that adheres to specific rules while optimizing factors like area, wirelength, and other performance metrics. 

In this step of OpenLANE ASIC flow,The synthesized netlist is to be placed on the floorplan.It occurs in two stages:

1. Global Placement
2. Detailed Placement

- Global Placement finds optimal position for all cells which may be not legal at the time and overlap.
- Detailed Placemnent changes this particular placement and make it legal.It is important from a timing point of view

```
run_placement
```
![Run_placement](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/9d91d4c2-0030-4ce8-bad2-55eafd458a67)

```
magic -T /home/akul/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read
picorv32.def &
```
![run_placement_magic](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/ca28cf01-61a9-412b-905d-e5181203203b)

Their are no DRC's and all the standard cell are placed at the standard cell rows. Floorplan ensured that their is DECAP at the boundaries of the standard cell . The Tap cells are properly placed and the IO patches are correctly placed. 
</details>

<details> 
      <summary> Cell Design & Characterization Flows </summary>

---

Under this section, we will go through a thorough insight into the Characterizatiob flow and various steps involved, what are my inputs given, my intermediate outputs and final results we get.

Standard cell design flow involves the following

Inputs:

**PDKs:**
 1. DRC & LVS rules
 2. SPICE models
 3. Libraries
 4. User-defined specifications.

Note: In standard cell libraries used in digital integrated circuit (IC) design, "drive strength" refers to the ability of a standard cell to source or sink current when driving a signal. It characterizes how much current a specific standard cell can provide (drive) to its output or draw from its input while maintaining proper signal integrity.

**Cell Design Flow:**

Cell design flow, also known as standard cell design flow, is the process of creating and optimizing standard cell libraries used in digital integrated circuit design. These libraries contain fundamental building blocks, such as logic gates and flip-flops, that are used to design complex digital circuits.

1. Specification and Requirements: Begin by defining the specifications and requirements for the standard cell library. This includes factors like technology node, voltage levels, speed requirements, and power constraints.

2. Cell Architecture Selection: Choose the architecture and topology for the standard cells. This involves deciding on the logical functions each cell will implement and the number of input and output pins.

3. Schematic Design: Create schematic designs for each standard cell. This involves designing the logical function of the cell using gates and interconnections. Tools like schematic capture software are used for this step.

4. Simulation and Verification: Simulate the designed cells to verify that they meet the specified functionality and timing requirements. This step may include functional simulation, static timing analysis (STA), and power analysis.

5. Layout Design: Create physical layouts for the cells based on the schematic designs. This involves specifying the dimensions, placement of transistors, and routing of metal layers.

6. DRC and LVS Checks: Perform Design Rule Check (DRC) and Layout vs. Schematic (LVS) checks to ensure that the layout adheres to the manufacturing rules and is consistent with the schematic.

7. Extraction and Characterization: Extract parasitic components from the layout, including resistances and capacitances. These parasitics impact the timing and power characteristics of the cells. Characterize the cells by measuring their performance under various conditions, such as different input vectors and operating voltages.

8. Timing Analysis: Conduct detailed timing analysis to determine parameters like propagation delay, setup time, hold time, and clock-to-q delay for flip-flops.

9. Library Validation: Validate the entire standard cell library by using it in test chip or design test cases to ensure that it meets performance and functionality requirements.

![cell_design_flow](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/b3f907ea-9fcc-4e10-b90c-ead7c1d986b4)

**Characterization Flow:** Characterization in VLSI refers to the process of analyzing and documenting the electrical behavior of electronic components, such as transistors, logic gates, memory cells, and standard cells, under various operating conditions. Characterization is essential for accurate circuit simulation and helps ensure that integrated circuits (ICs) meet their performance, power, and timing requirements.




**Design steps:**

1. Circuit design
2. Layout design (Art of layout Euler's path and stick diagram)
3. Extraction of parasitics
4. Characterization (timing, noise, power).

**Outputs:**

1. CDL (circuit description language)
2. LEF
3. GDSII
4. extracted SPICE netlist (.cir)
5. timing, noise and power .lib files
6. Standard Cell Characterization Flow

Standard Cell Characterizarion Flow
===================================

The industry-standard process for characterizing standard cells typically consists of the following stages:

1. Read in the models and tech files
2. Read extracted spice Netlist
3. Recognise behavior of the cells
4. Read the subcircuits
5. Attach power sources
6. Apply stimulus to characterization setup
7. Provide neccesary output capacitance loads
8. Provide neccesary simulation commands
9. For characterization an opensource software called GUNA is used.
10. All the steps from 1 to 8 are fed into GUNA,which in turn generates timing,noise and power models.

Now all these 8 steps are fed in together as a configuration file to a characterization software called GUNA. This software generates timing, noise, power models. These .libs are classified as Timing characterization, power characterization and noise characterization.

![GUNA](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/7f5866eb-194d-4ef7-bb12-98c987f28a16)

TIMING CHARACTERIZATION
=======================
In standard cell characterisation, One of the classification of libs is timing characterisation.

**Timing defintion Value**

1. slew_low_rise_thr - 20% value
2. slew_high_rise_thr - 80% value
3. slew_low_fall_thr	- 20% value
4. slew_high_fall_thr - 80% value
5. in_rise_thr - 50% value
6. in_fall_thr - 50% value
7. out_rise_thr - 50% value
8. out_fall_thr - 50% value

Propagation Delay and Transition Time
=====================================
**Propagation Delay :** The time difference between when the transitional input reaches 50% of its final value and when the output reaches 50% of its final value. Poor choice of threshold values lead to negative delay values. Even thought you have taken good threshold values, sometimes depending upon how good or bad the slew, the dealy might be still +ve or -ve.
```
Propagation delay = time(out_thr) - time(in_thr)
```
**Transition Time:** The time it takes the signal to move between states is the transition time , where the time is measured between 10% and 90% or 20% to 80% of the signal levels.
```
Rise transition time = time(slew_high_rise_thr) - time (slew_low_rise_thr)

Low transition time = time(slew_high_fall_thr) - time (slew_low_fall_thr)
```

</details>


## DAY-3: Design Library Cell using ngspice simulations

<details>
      <summary> CMOS inverter ngspice simulations </summary>

---

``ngspice`` is opesoure engine where simulations are done.

**IO Placer Revesion:** PnR is a iterative flow and hence, we can make changes to the environment variables in the fly to observe the changes in our design.
Let us say If I want to change my pin configuration along the core from equvi distance randomly placed to someother placement, we just set that IO mode variable on command prompt as shown below

```
set ::env(FP_IO_MODE) 2
```

SPICE Deck Creation and Simulation for CMOS inverter
====================================================

- Before performing a SPICE simulation we need to create SPICE Deck. SPICE Deck provides information about the following:
- Component connectivity - Connectivity of the Vdd, Vss,Vin, substrate. Substrate tunes the threshold voltage of the MOS.
- Component values - values of PMOS and NMOS, Output load, Input Gate Voltage, supply voltage.
- Node Identification and naming - Nodes are required to define the SPICE Netlist For example M1 out in vdd 
```
vdd pmos w = 0.375u L = 0.25u , cload out 0 10f
```
- Simulation commands
- Model file - information of parameters related to transistors Simulation of CMOS using different width and lengths. From the waveform, irrespective of switching the shape of it are almost 
  same.


![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/0b1c5a67-245f-4298-b94a-8b5588dbea42)

From the waveform we can see the characteristics are maintained across all sizes of CMOS. So CMOS as a circuit is a robust device hence use in designing of logic gates. Parameters that define the robustness of the CMOS are

Switching Threshold Vm
=====================
- The Switching Threshold of a CMOS inverter is the point where the Vin = Vout on the DC Transfer characreristics.
- At this point, both the transistors are in saturation region, means both are turned on and have high chances of current flowing driectly from VDD to Ground called Leakage current.


![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/89fa1791-a1c8-4383-9f1a-df80ec4853e1)

Through transient analysis, we calculate the rise and fall delays of the CMOS by SPICE Simulation. As we know delays are calculated at 50% of the final values.

Lab steps to git clone vsdstdcelldesign
=======================================

First, clone the required mag files and spicemodels of inverter,pmos and nmos sky130. The command to clone files from github link is:

```
git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```
once I run this command, it will create vsdstdcelldesign folder in openlane directory.

Inorder to open the mag file and run magic go to the directory

For layout we run magic command

``magic -T sky130A.tech sky130_inv.mag &``

Ampersand at the end makes the next prompt line free, otherwise magic keeps the prompt line busy. Once we run the magic command we get the layout of the inverter in the magic window


![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/43d23d97-eb6c-4067-85a5-bea4de45ce56)


</details>


<details> 
     <summary> Inception of Layout and CMOS Fabrication Process </summary>

---

Under this section we will look into the Fabrication process. We will look into the various steps for 16-mask fab procedure

16-MASK CMOS Process
====================
1. Selecting a substrate
   - We choose an appropriate substrate as per requirement.
   - We go with the most common substrate available - P-type.

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/f6b76a38-19d6-48e8-9d79-a7fde3c2f628)


2. Creation of Active regions for transistors.
 - We have to make isolation for each pocket, this is done by growing Silicon Dioxide of 40nm over the P-type substrate, then deposit an 80nm layer of Silicon nitride.
 - Now deposit 1micron of photoresist. On this we make Mask1 and Mask 2 for the pockets and shower it with UV lights
 - The photoresist under the masks are protected and remaining is etched away with some chemical reaction. Now the mask is removed.
 - Now we etch off the extra silicon nitride, thus only silicon nitride left are the ones protected by the photoresist. Now Remove left photoresist.
 - Now, place the entire thing in oxidation furnace. Silicon nitride protects the SiO2 underneath from growing further.
 - The growth between the nitride layer acts as the isolation as they don't allow the transistor areas to communicate. This growth is also called bird's beak.
 - The remaining nitride layer is etched off.
 - This whole process is called LOCOS - Local oxidation of Silicon

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/4c5c545c-e07b-4542-b4e9-128d41e4da58)

3. Formation of N-Well and P-Well
 - The N-well and P-well regions are created separately.
 - P-well formation involves photolithography and ion implantation of p-type Boron material into the p-substrate. Energy required is 200keV.
 - N-well is formed similarly with n-type Phosphorus material. Energy requirement is 400keV.
 - This ion implantation damages the SiO2 layer.
 - High-temperature furnace processes drive-in diffusion to establish well depths, known as the twin-tub process.


![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/d91d6c4f-dba1-4bfb-95d7-f9296293c377)


4. Formation of Gate Terminal:
- Gate is the most important terminal as here we control the input voltage.
- Important parameters for gate formation include oxide capacitance and doping concentration.
- A polysilicon layer is deposited and photolithography techniques are applied to create NMOS and PMOS gates.
- The SiO2 layers over Nwell and Pwell are etched off using polysulpuric acid and fresh layer is made with goof thickness.

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/8f151080-b698-4bf7-b978-1760085e47e0)

5. Lightly-Doped Drain(LDD) Formation:
- This is done to achieve a doping profile --> P+, P-, N for NMOS and N+, N- and P for PMOS.
- LDD is created to control hot electron and short channel effects.

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/5e4b9928-b14e-4045-84eb-87d28c92a706)

6. Source and Drain Formation:
- Thin oxide layers are added to avoid channel effects during ion implantation.
- N+ and P+ implants are performed using Arsenic implantation and high-temperature annealing.

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/6646ca75-a00b-4cc6-9875-1aaca08f679e)


7. Local Interconnect Formation:
- Thin screen oxide is removed through etching in HF solution.
- Titanium deposition through sputtering is initiated.
- Heat treatment results in chemical reactions, producing low-resistant titanium silicon dioxide for interconnect contacts and titanium nitride for top-level connections, enabling local communication.

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/4a2fb12c-1524-41e0-9391-50a83e38a92d)

8. Higher Level Metal Formation:
- To achieve suitable metal interconnects, non-planar surface topography is addressed.
- Chemical Mechanical Polishing (CMP) is utilized by doping silicon oxide with Boron or Phosphorus to achieve surface planarization.
- TiN and blanket Tungsten layers are deposited and subjected to CMP.
- An aluminum (Al) layer is added and subjected to photolithography and CMP.
- This constitutes the first level of interconnects, and additional interconnect layers are added to reach higher-level metal layers.

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/dc08f85b-8b33-47dc-8acd-40d5f2c2bd89)

9. Dielectric Layer Addition:
- Finally, a dielectric layer, typically Si3N4, is applied to safeguard the chip.

This complex process results in the creation of advanced integrated circuits with multiple layers of interconnects, essential for modern electronic devices.

Introduction to SKY130 Basic Layout and LEF
==========================================

From Layout, we see the layers which are required for CMOS inverter. Inverter is, PMOS and NMOS connected together.

- Gates of both PMOS and NMOS are connected together and fed to input(here ,A), NMOS source connected to ground(here, VGND), PMOS source is connected to VDD(here, VPWR), Drains of PMOS and NMOS are connected together and fed to output(here, Y).
- The First layer in skywater130 is localinterconnect layer(locali) , above that metal 1 is purple color and metal 2 is pink color.
- If we want to see connections between two different parts, place the cursor over that area and press S one times. The tkson window gives the component name.

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/44b0691d-8f32-4206-9fe4-458f661a7f1b)

**LEF - Library Exchange File**

- The layout of a design is defined in a specific file called LEF.
- It includes design rules (tech LEF) and abstract information about the cells.
   - Tech LEF - Technology LEF file contains information about the Metal layer, Via Definition and DRCs. 
   - Macro LEF - Contains physical information of the cell such as its Size, Pin, their direction.

**Designing standard cell**

- First we need to provide bounding box width and height in tkson window. lets say that width of BBOX is 1.38u and height is 2.72u. The command to give these values to MAGIC is property Fixed BBOX (0 0 1.32 2.72)
- After this, Vdd, GND segments which are in metal 1 layer, their respective contacts and atlast logic gates layout is defined Inorder to know the logical functioning of the inverter, we extract the spice and then we do simulation on the spice.

**SPICE extraction in MAGIC**

To extract it on spice we open TKCON window, the steps are :

- Know the present directory - pwd
- Create an extration file - the command is extract all and sky130_inv.ext files has been created
- Create spice file using .ext file to be used with our ngspice tool - the commands are
 1. ext2spice cthresh 0 rthresh 0 - extracts parasatic capcitances also since these are actual layers - nothing is created in the folder
 2. ext2spice - a file sky130_inv.spice has been created.

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/cbf82ecc-2dd5-44b0-a23f-4f14552fc0e6)


</details>

<details>
      <summary> Lab on SKY130 Tech File </summary>

---
Under this section, we will go over how to infer the spice deck file and how to run the transient analysis using NGspice. Once the simulation is done, we will characterise the simulation plot.

**Spice Deck:**

- The design is scaled to 0.01u
- The NMOS and PMOS are defined as
``cell_name drain_node gate_node source_node model_file_name``
```
M1000 Y A VGND VGND nshort_model.0 w=35 l=23
M1001 Y A VPWR VPWR pshort_model.0 w=37 l=23
```
- We will include the model files for NMOS and PMOS from the libs directory.
```
 .include ./libs/nshort.lib
 .include ./libs/pshort.lib
```
- Now, we set up the connections to the nodes with ground, Vdd and input pulses.
  - VGND to VSS 0V
  - Supply voltage VPWR to GND.
  - Sweeping a pulse input.

- Now we set the transient analysis.
```
VDD VPWR 0 3.3V
VSS VGND 0 0V
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)
.tran 1n 20n
.control
run
.endc
.end
```
- Final Spice deck for simulation.

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/b4290178-f0d8-4ffc-a3c9-6a05bdfd91c5)


**NGpsice Simulation and Characterization**

- Code to run the simulation
```
ngspice sky130_inv.spice
```

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/54bb2f39-c958-4139-b37f-f2bb5083a389)

- To get the plot for output against time with the sweeping input
```
plot y vs time av
```

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/05225cc7-ed84-409d-82ba-0c7b8d3aa64b)

- Now we have to characterise the plot.
- There are four timing parameters used to characterize the inverter standard cell:
  - Rise transition - Time taken for the output to rise from 20% to 80% of max value => 2.240 - 2.143 = 0.067ns
  - Fall Transition - Time taken for the output to fall from 80% to 20% of max value => 4.0921 - 4.049 = 0.0431ns
  - Cell Rise delay - Difference in time(50% output rise) to time(50% input fall) => 2.17333 - 2.13 = 0.0433ns
  - Cell Fall delay - Difference in time(50% output fall) to time(50% input rise) => 4.076 - 4.0501 = 0.0259ns

DRC Challenges
==============

Under this section, we will go over

- In-depth overview of Magic's DRC engine
- Introduction to Google/Skywater DRC rules
- Lab : Warm-up exercise : Fixing a simple rule error
- Lab : Main exercie : Fixing or create a complex error

Introdution to Magic and Skywater PDK
====================================
For running the DRC we need to have an understanding of the technology node we are working on. For this one can refer the following

- Magic --> [link]([https://www.github.com](http://opencircuitdesign.com/magic/))
- Skywater PDK 
- Github Repo for Skywater PDK --> [github](https://github.com/google/skywater-pdk)

Lab Setup
========

- Setup to view the layouts
- For extracting and generating views, Google/skywater repo files were built with Magic
- Technology file dependency is more for any layout. hence, this file is created first.
- Since, Pdk is still under development, there are some unfinished tech files and these are packaged for magic along with lab exercise layout and bunch of stuff into the tar ball
```
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
```
- Once we have downloaded the archive in the home directory, we extract it to get the lab .mag files
- There is a hidden file ``.magicrc`` which directs to the various resources for the lab work ahead.

MAGIC
=====

- Run Magic.For better graphic use, the command belwo is used:
```
magic -d XR
```
- To open a file we can load the file as such:
![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/1a5c9ee6-4bc9-4010-8949-fec441ed40d8)

- Other way to load it is by defining the name while running magic.
```
magic -d XR <file_name>.mag
```

- We will open up met3.mag
- We see multiple independent example metal layouts with some DRC errors. We can refer these errors in the the Skywater PDK design rules which are flageed in the DRC engine.
- We can make a frame around a metal region and in command window write drc why --> this gives us the DRC violated.
![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/64ced32f-ff4b-49a0-87d7-de23971032ec)


- Magic uses a lot of derived layers. To see these layers we can make a large box area and use following commands to see metal cut
```
cif see VIA2
```
LAB
===

**Exercise-1**
- Load the poly.mag
- Check the drc violation for poly.9
- Refer the error using skywater pdk design rules
   - We find that distance between regular polysilicon & poly resistor should be 22um but it is showing 17um and still no errors . We should go to sky130A.tech file and modify as follows to detect this error.
- In line this,
```
*******************************************************
spacing npres *nsd 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
*******************************************************
```
- Edit as shown.
```
*******************************************************
spacing npres allpolynonres 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
*******************************************************
```

- Now the second edit. In line this.
```
*******************************************************
spacing xhrpoly,uhrpoly,xpc alldiff 480 touching_illegal \
	"xhrpoly/uhrpoly resistor spacing to diffusion < %d (poly.9)"
*******************************************************
```
- Edit as shown.

```
*******************************************************
spacing xhrpoly,uhrpoly,xpc allpolynonres 480 touching_illegal \
	"xhrpoly/uhrpoly resistor spacing to diffusion < %d (poly.9)"
*******************************************************
```
- After this, we tech load ``sky130.tech`` file and execute ``drc check``

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/baacdb4a-831c-4cc4-aad1-12e46bba55e9)

- We can select poly.9 and ``run drc`` why to check for errors. Now it fine.
![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/f65ef446-ab80-46d2-9c38-32c9f590324c)

</details>

## DAY-4: Pre-layout Timing Analysis and Importance of Good Clock Tree

<details>
      <summary> Timimg Modelling using Delay Models </summary>
---

Standard Cell LEF generation
=============================

During Placement, entire mag information is not necessary. Only the PR boundary, I/O ports, Power and ground rails of the cell is required. This information is defined in LEF file. The main objective is to extract lef from the mag file and plug into our design flow.

Grid into Track
==============

Track: A path or a line on which metal layers are drawn for routing. Track is used to define the height of the standard cell.

Guidelines for making a standard cell
======================================

- I/O ports must lie on the intersection on Horizontal and vertical tracks.
- Width of standard cell is odd mutliples of Horizontal track pitch or X direction pitch.
- Height of standard cell is odd mutliples of Vertical track pitch or y direction pitch.

The information regarding the tracks is given in ``/home/shant/.volare/sky130A/libs.tech/openlane/sky130_fd_sc_hd/tracks.info``
```
li1 X 0.23 0.46
li1 Y 0.17 0.34
met1 X 0.17 0.34
met1 Y 0.17 0.34
met2 X 0.23 0.46
met2 Y 0.23 0.46
met3 X 0.34 0.68
met3 Y 0.34 0.68
met4 X 0.46 0.92
met4 Y 0.46 0.92
met5 X 1.70 3.40
met5 Y 1.70 3.40
```
- It tells us about all the metal layers as such.
- We learnt that the input port and output for should be on the intersection of horizontal and vertical tracks, to verify this we set the grids as
```
grid 0.46um 0.34um 0.23um 0.17um
```
- Now we see the layout on Magic again.
![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/0e460a5b-0f42-4b6d-b71a-5cd73461e0fc)

- The second condition is also verified. The X-pitch is 0.46 and we can see that the standard cell is 3 times that, thus an odd multiple.
- The same can be verified for the height of the standard cell.

Creation of Ports
=================
- Once the layout is ready, the next step is extracting LEF file for the cell.

- Certain properties and definitions need to be set to the pins of the cell. For LEF files, a cell that contains ports is written as a macro cell, and the ports are the declared as PINs of the macro.

- Our objective is to extract LEF from a given layout (here of a simple CMOS inverter) in standard format. Defining port and setting correct class and use attributes to each port is the first step.

- Method for definng ports

     - In Magic Layout window, first source the .mag file for the design (here inverter). Then Edit >> Text which opens up a dialogue box.
     - For each layer (to be turned into port), make a box on that particular layer and input a label name along with a sticky label of the layer name with which the port needs to be associated. Ensure the Port enable checkbox is checked and default checkbox is unchecked.
 
  ![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/61914e83-af9f-46af-b741-07797e45fde3)

- Port A (input port) and port Y (output port) are taken from locali (local interconnect) layer. Also, the number in the textarea near enable checkbox defines the order in which the ports will be written in LEF file (0 being the first).
- For power and ground layers, the definition could be same or different than the signal layer. Here, ground and power connectivity are taken from metal1 (Notice the sticky label).


Port Class and Port Use Attributes
==================================

- After defining ports, the next step is setting port class and port use attributes.

- Select port A in magic:
```
port class input
port use signal
```
- Select Y area
```
port class output
port use signal
```

- Select VPWR area
```
port class inout
port use power
```

- Select VGND area
```
port class inout
port use ground
```
![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/7034226d-4585-48ab-82bd-77fb1bed3dca)

**Extraction of LEF file**

- Name the custom cell through tkcon window as sky130_shant.mag.
- We generate lef file by command:

```
lef write
```
- Upon checking the directory, we can see the lef file being generated.

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/ced0bec1-89dd-4f75-a1dc-b9935d83f655)

- lef file generated.
![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/2ef1009a-8f0c-4058-98ce-7fe48fc6f849)

**Including Custom Cell ASIC Design:**

- First, we transfer the lef file generated sky130_shant.lef into the /home/shant/OpenLane/designs/picorv32a/src directory.

- Then we will transfer the ``sky130_fd_sc_hd__fast.lib``, ``sky130_fd_sc_hd__slow.lib`` and ``sky130_fd_sc_hd__typical.lib`` into the same directory.

- For this, we edit the ``config.json`` file as below:
  ```
  {
  "DESIGN_NAME": "picorv32",
  "VERILOG_FILES": "dir::src/picorv32a.v",
  "CLOCK_PORT": "clk",
  "CLOCK_NET": "clk",
  "FP_SIZING": "relative",
  "GLB_RESIZER_TIMING_OPTIMIZATIONS": true,
  "LIB_SYNTH" : "dir::src/sky130_fd_sc_hd__typical.lib",
  "LIB_FASTEST" : "dir::src/sky130_fd_sc_hd__fast.lib",
  "LIB_SLOWEST" : "dir::src/sky130_fd_sc_hd__slow.lib",
  "LIB_TYPICAL":"dir::src/sky130_fd_sc_hd__typical.lib",
  "TEST_EXTERNAL_GLOB":"dir::/src/*",
  "SYNTH_DRIVING_CELL":"sky130_vsdinv",
  "pdk::sky130*": {
    "FP_CORE_UTIL": 35,
    "CLOCK_PERIOD": 24,
    "scl::sky130_fd_sc_hd": {
      "FP_CORE_UTIL": 30
    }
  }
  }
  ```
- Now, we integrate standard cell on OpenLane flow after make mount, and follow up
```
prep -design picorv32a -tag RUN_2023.09.11_06.05.06 -overwrite 
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
run_synthesis
```
![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/b1fa6837-a01e-4c8e-a24e-f6877a834470)

- Synthesis log file

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/8ffe929e-88e1-4ca9-b485-581838b6c1da)

- Static timing analysis (STA) log file:
![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/ad822f8d-ac64-4892-8291-fd0c64381766)
 

Delay Table
==========
Delay is a parameter that has huge impact on our cells in the design. Delay decides each and every other factor in timing. For a cell with different size, threshold voltages, delay model table is created where we can it as timing table.

- Delay of a cell depends on input transition and out load.

Lets say two scenarios, we have long wire and the cell(X1) is sitting at the end of the wire : the delay of this cell will be different because of the bad transition that caused due to the resistance and capcitances on the long wire. we have the same cell sitting at the end of the short wire: the delay of this will be different since the tarn is not that bad comapred to the earlier scenario. Eventhough both are same cells, depending upon the input tran, the delay got chaned. Same goes with o/p load also.

VLSI engineers have identified specific constraints when inserting buffers to preserve signal integrity. They've noticed that each buffer level must maintain consistent sizing, but their delays can vary depending on the load they drive. To address this, they introduced the concept of "delay tables", which essentially consist of 2D arrays containing values for input slew and load capacitance, each associated with different buffer sizes. These tables serve as timing models for the design.

When the algorithm works with these delay tables, it utilizes the provided input slew and load capacitance values to compute the corresponding delay values for the buffers. In cases where the precise delay data is not readily available, the algorithm employs a technique of interpolation to determine the closest available data points and extrapolates from them to estimate the required delay values.

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/1cd2f6d0-4b7e-4152-9857-213c1c9ba8dc)

**Custom Cell inclusion in OpenLane Flow**

- We have seen till the synthesis for the custom standard cell in OpenLane flow, and verified the synthesis and STA log files. We will pick it from there now.

- First check the slack for the synthesis.

- The slack was positive, therefore we can proceed, else would have to work on the slack.

Now we run the floorplan and placement processes.

```
run_floorplan
run_placement
```

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/4e35db1f-f573-4692-82cd-9018b38ddc1e)

- Now, we check for legality &To check the layout invoke magic from the results/placement directory

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/970beb1a-739f-4755-b2a0-6b3f03df3366)


</details>


<details> 
      <summary> Timing Analysis with Ideal Clocks using OpenSTA </summary>
---

**Set-up Timing Analysis**

- Right now, we will consider the ideal clocks, thus the clock tree are not yet made.

- We take a single clock and anlysis launch and capture flops.


![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/fff84b09-a2a8-4407-83d0-fe48be83b897)

- In this, we assume that launch flop is triggered at the first posedge of clk and the capture flop recieves the value at the next posedge.

- Suppose there was some combinational logic between the two, the delay of the logic should be less than the time period of the clock.

- Thus the clock frequency and time period, and the combinational logic are designed with correspondence to each other.

- Therefore my setup time for the combinational logic should be less than the time period of the clock.

- Now, we will look into more real and practical conditions.

- We look into the capture flop. It is made of multiple gates and muxes, which will have there mosfets, resistances and capacitances.

- Thus will have delay associated to them.

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/bb961f8b-110d-44dd-913b-d3a83c305e29)

- Suppose the flop was developed with 2 muxes as shown. We have to condsider the delays.

- This affect the combinational logic delay requirement. Now, the clock period T is not avaiable. The capture flop needs some setup time.

- Thus the time avaiable for the combinational logic now is T - setupTime of capture flop.

- Clock Jitter - clock is generated from PLL which has inbuilt circuit which cells and some logic. There might variations in the clock generation depending upon the ckt. These variations are collectivity known as clock uncertainity. In that jitter is one of the parameter. It is uncertain that clock might come at that exact time withought any deviation.

- That is why it is called clock_uncertainity Skew, Jitter and Margin comes into clock_uncertainity

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/53e83f95-8df0-4fae-a1c7-c972921178f7)

Post-Synthesis Analysis using OpenSTA
====================================

Timing analysis is carried out outside the OpenLANE flow using OpenSTA tool. For this, pre_sta.conf is required to carry out the STA analysis. Invoke OpenSTA outside the openLANE flow as follows:

```
sta pre_sta.conf
```
Since clock tree synthesis has not been performed yet, the analysis is with respect to ideal clocks and only setup time slack is taken into consideration. The slack value is the difference between data required time and data arrival time. The worst slack value must be greater than or equal to zero. If a negative slack is obtained, following steps may be followed:

Change synthesis strategy, synthesis buffering and synthesis sizing values
Review maximum fanout of cells and replace cells with high fanout
sdc file for OpenSTA is modified.
``base.sdc`` is located in ``vsdstdcelldesigns/extras`` directory. So, we copy it into our design folder using ``cp my_base.sdc /home/emil/OpenLane/designs/picorv32a/src/``

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/3b7e7bb4-61be-416f-8c2d-a2f865eb1e04)

From the timing report, we can improve slack by upsizing the cells i.e., by replacing the cells with high drive strength and we can see significant changes in the slack. Since there were no timing violations, we can skip this step.Since clock is propagated only once we do CTS, In placement stage, clock is considered to be ideal. So only setup slack is taken into consideration before CTS.

</details>

<details>
      <summary> Clock tree synthesis TritonCTS and signal integrity </summary>

---

Clock Tree Synthesis (CTS) plays a vital role in the creation of integrated circuits (ICs), particularly in the realm of digital electronics, where precise timing is of utmost importance. CTS involves the establishment of an organized network or structure of pathways for distributing the clock signal within the IC. This meticulous process guarantees that the clock signal effectively reaches all the sequential components, such as flip-flops and registers, in a synchronized and punctual fashion.

It can be implemeted in various ways and the choice of the specific technique depends on the design requirements, constraints, and goals.
Some of the different types of approches to clock tree synthesis are:

- Balanced Tree CTS: The clock signal is spread out evenly, like branches of a tree. This helps ensure that all parts of the chip get the clock at about the same time, reducing timing problems. It's a straightforward method, but it might not save as much power as other methods.
- H-tree CTS: It is like a tree shape with the letter "H." It's great for spreading out clock signals across big chips. This tree structure helps make sure the timing is good and saves power, especially in large areas of the chip.
- Star CTS: In a star CTS, the clock signal is distributed from a single central point (like a star) to all the flip-flops. This approach simplifies clock distribution and minimizes clock skew but may require a higher number of buffers near the source.
- Mesh CTS: In a mesh CTS, clock wires are arranged in a mesh-like grid pattern, and each flip-flop is connected to the nearest available clock wire. It is often used in highly regular and structured designs, such as memory arrays. Mesh CTS can offer a balance between simplicity and skew minimization.
- Adaptive CTS: Adaptive CTS techniques adjust the clock tree structure dynamically based on the timing and congestion constraints of the design. This approach allows for greater flexibility and adaptability in meeting design goals but may be more complex to implement.

Crosstalk in VLSI
=================

Crosstalk in VLSI refers to unwanted interference or coupling between adjacent conductive traces or wires on an integrated circuit (IC) or chip. It occurs when the electrical signals on one wire influence or disrupt the signals on neighboring wires.Uncontrolled crosstalk can lead to data corruption, timing violations, and increased power consumption. Mitigation: VLSI designers employ various techniques to mitigate crosstalk, such as optimizing layout and routing, using appropriate shielding, implementing proper clock distribution strategies, and utilizing clock gating to reduce dynamic power consumption when logic is idle

Clock net sheilding in VLSI
===========================

Clock net shielding in VLSI refers to a technique used to protect the clock signal from interference or crosstalk. The clock signal is critical for synchronizing the operations of various components on a chip, and any interference can lead to timing issues and performance problems.
VLSI designers may use shielding techniques to isolate the clock network from other signals, reducing the risk of interference. This can include dedicated clock routing layers, clock tree synthesis algorithms, and buffer insertion to manage clock distribution more effectively.
VLSI designs often have multiple clock domains. Shielding and proper clock gating help ensure that clock signals do not propagate between domains, avoiding metastability issues and maintaining synchronization.

CTS LAB
=======
The below command is used to run CTS in OpenLANE.
```
run_cts
```
![run_cts](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/dbdd3e21-b2c4-4994-8196-4eb1a6b15eb0)


![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/427a0679-0ee3-4f14-adca-175ef6719174)

After CTS run, my slack values are ``setup:12.36, Hold:0.38``
Here also both values are not violating.

</details>

<details>
      <summary> Timing analysis with real clocks </summary>

Setup Timing Analysis using real clocks
=======================================

Analyzing setup time is a crucial element of designing digital circuits, especially in synchronous digital systems. It pertains to the duration during which a signal must remain steady and valid prior to the arrival of the clock edge. Guaranteeing the fulfillment of setup time prerequisites is vital for averting data errors and securing the correct functioning of the digital circuit.


![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/67f61014-e9e2-4fea-9557-10fc3d02e17a)

To ensure the setup time requirements are met we need to make sure of some things:

1. Selecting proper Filp flops or latches.
2. Optimize combinational logic
3. Clock Skew Analysis
4. Timing constraints

Meeting setup time requrirements is cruical for a good digital circuit operation. If not done can result in data errors and multifunctioning of the circuit.

Holding Timing Analysis using real clock
=======================================
Analysis of hold time is an equally vital component of digital circuit design, especially in synchronous systems. It concerns the minimum duration during which a data input (D) needs to maintain its stability and validity after the clock edge before any changes can occur. Ensuring that hold time requirements are met is essential to prevent data corruption and ensure the proper operation of digital circuits.


![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/f9437a1c-f6e1-4ea6-ab64-7392c6aae91a)

Since, clock is propagated, from this stage, we do timing analysis with real clocks. From now post cts analysis is performed by operoad within the openlane flow

```
openroad
read_lef /home/emil/OpenLane/designs/picorv32a/runs/RUN_2023.09.17_04.44.22/tmp/merged.nom.lef 
read_def /home/emil/OpenLane/designs/picorv32a/runs/RUN_2023.09.17_04.44.22/results/cts/picorv32.def 
read_verilog /home/emil/OpenLane/designs/picorv32a/runs/RUN_2023.09.17_04.44.22/results/synthesis/picorv32.v
write_db pico_cts.db
read_db pico_cts.db
read_verilog /home/emil/OpenLane/designs/picorv32a/runs/RUN_2023.09.17_04.44.22/results/synthesis/picorv32.v
link_design picorv32
read_liberty $::env(LIB_SYNTH_COMPLETE)
read_sdc /home/emil/OpenLane/designs/picorv32a/src/my_base.sdc
set_propagated_clock (all_clocks)
report_checks -path_delay min_max -format full_clock_expanded -digits 4
```

</details>


## DAY-5:  Final steps for RTL2GDS using TritonRoute and OpenSTA

<details>
      <summary> Maze routing and Lee's Algorithm </summary>

---
Routing is the process of establishing a physical connection between two pins. Algorithms designed for routing take source and target pins and aim to find the most efficient path between them, ensuring a valid connection exists.

The Maze Routing algorithm, such as the Lee algorithm, is one approach for solving routing problems.Here a grid similar to the one created during cell customization is utilized for routing purposes.
The Lee algorithm starts with two designated points, the source and target, and leverages the routing grid to identify the shortest or optimal route between them.

Lee's Algorithm has its limitations. It can be time consuming when dealing with millions of pins.It essentially constructs a maze and then numbers its cells from the source to the target. here are alternative algorithms that address similar routing challenges.

Here in this case he shortest path is one that follows a steady increment of one.There might be multiple paths, but the best path that the tool will choose is one with less bends.The route should not be diagonal and must not overlap an obstruction such as macros. The Lee algorithm prioritizes selecting the best path, typically favoring L-shaped routes over zigzags. If no L-shaped paths are available, it may resort to zigzag routes. This approach is particularly valuable for global routing tasks.

This algorithm however has high run time and consume a lot of memory thus more optimized routing algorithm is preferred .

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/4ab58f1b-3999-42ff-b722-f30ac2bcda45)

Design Rule Check
==================

Design rule checks are physical checks of metal width, pitch and spacing requirement for the different layers which depend on different technology nodes.It verifies whether a design meets the predefined process technology rules given by the foundry for its manufacturing.

The layout of a design must be in accordance with a set of predefined technology rules given by the foundry for manufacturability. After completion of the layout and its physical connection, an automatic program will check each and every polygon in the design against these design rules and report any violations.

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/90753419-6485-48ab-9da4-84cfa30318f3)


</details>

<details> 
      <summary> Power Distribution Network Generation </summary>

---
Unlike the general ASIC flow, Power Distribution Network generation is not a part of floorplan run in OpenLANE. PDN must be generated after CTS and post-CTS STA analyses:
We can check whether PDN has been created or no by check the current ``def environment variable: echo $::env(CURRENT_DEF)``

```
prep -design picorv32a -tag <RUN file name>
gen_pdn
```

![gen_PDN](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/835b436a-1422-46d9-927a-d9d898bc3847)

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/ccf6797b-8691-49ff-81fe-df8d586ea40a)

- gen_pdn Generates the power distribution network.

- The power distribution network has to take the design_cts.def as the input def file.

- Power rings,strapes and rails are created by PDN.

- From VDD and VSS pads, power is drawn to power rings.

- Next, the horizontal and vertical strapes connected to rings draw the power from strapes.

- Stapes are connected to rings and these rings are connected to std cells. So, standard cells get power from rails.

- Here are definitions for the straps and the rails. In this design, straps are at metal layer 4 and 5 and the standard cell rails are at the metal layer 1. Vias connect accross the layers as required.

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/2af5911d-f608-44a8-9d1b-fe47b5ab5de2)

</details>

<details> 
     <summary> Routing </summary>

---

In the realm of routing within Electronic Design Automation (EDA) tools, such as both OpenLANE and commercial EDA tools, the routing process is exceptionally intricate due to the vast design space. To simplify this complexity, the routing procedure is typically divided into two distinct stages: Global Routing and Detailed Routing.

The two routing engines responsible for handling these two stages are as follows:

1. Global Routing: In this stage, the routing region is subdivided into rectangular grid cells and represented as a coarse 3D routing graph. This task is accomplished by the "FASTE ROUTE" engine.

2. Detailed Routing:

Here, finer grid granularity and routing guides are employed to implement the physical wiring. The "tritonRoute" engine comes into play at this stage. "Fast Route" generates initial routing guides, while "Triton Route" utilizes the Global Route information and further refines the routing, employing various strategies and optimizations to determine the most optimal path for connecting the pins.

Key Features of TritonRoute
==========================

1. Initial Detail Routing: TritonRoute initiates the detailed routing process, providing the foundation for the subsequent routing steps.

2. Adherence to Pre-Processed Route Guides: TritonRoute places significant emphasis on following pre-processed route guides. This involves several actions:

3. Initial Route Guide Analysis: TritonRoute analyzes the directions specified in the preferred route guides. If any non-directional routing guides are identified, it breaks them down into unit widths.

4. Guide Splitting: In cases where non-directional routing guides are encountered, TritonRoute divides them into unit widths to facilitate routing.

5. Guide Merging: TritonRoute merges guides that are orthogonal (touching guides) to the preferred guides, streamlining the routing process.

6. Guide Bridging: When it encounters guides that run parallel to the preferred routing guides, TritonRoute employs an additional layer to bridge them, ensuring efficient routing within the preprocessed guides.

Assumes route guide for each net satisfy inter guide connectivity Same metal layer with touching guides or neighbouring metal layers with nonzero vertically overlapped area( via are placed ).each unconnected termial i.e., pin of a standard cell instance should have its pin shape overlapped by a routing guide( a black dot(pin) with purple box(metal1 layer))

TritonRoute problem statement
============================

```
Inputs : LEF, DEF, Preprocessed route guides
Output : Detailed routing solution with optimized wire length and via count
Constraints : Route guide honoring, connectivity constraints and design rules.
```
The space where the detailed route takes place has been defined. Now TritonRoute handles the connectivity in two ways.

- Access Point(AP) : An on-grid point on the metal of the route guide, and is used to connect to lower-layer segments, pins or IO ports,upper-layer segments. Access Point Cluster(APC) : A union of all the Aps derived from same lower-layer segment, a pin or an IO port, upper-layer guide.

**TritonRoute run for routing**

Make sure the CURRENT_DEF is set to ``pdn.def``

- Start routing by using
```
run_routing
```
![run_rounting](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/042314bf-b7d5-47a4-b07e-c6cb761871d5)

- Log File:

  ![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/dda09d31-51a5-422d-a47a-e8570c16d22c)

  Layout in magic tool post routing
  =================================

  The design can be viewed on magic within results/routing directory. Run the follwing command in that directory:
```
magic -T ~/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32.def &
```
![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/c20a0670-ea61-41d8-ac14-3b0f55dae4f8)

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/38b527b9-fc26-43eb-9a04-20c55f4c42ae)


</details>


## Acknowledgement
1. Kunal Ghosh, VSD Corp. Pvt. Ltd.
2. ChatGPT
3. Alwin Shaju, Colleague, IIITB
4. Emil J. Lal , Colleague, IIITB
5. Shant Rakshit, Colleague IIITB
