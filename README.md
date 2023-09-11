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

      A. **Global Routing:**  G
Global routing is the initial phase of routing in chip design. It determines high-level routing paths for nets between macroblocks or functional units on the chip, focusing on channel assignments and chip-level optimization. The outcome is a routing framework or guides for subsequent detailed routing.

      B. **Detailed Routing:**
Detailed routing follows global routing and defines precise paths for individual wires within nets. It works at a lower, detailed level, considering cell positions, design rules, and minimizing wirelength. The result is the completed layout of physical interconnections, adhering to global routing guidelines.

   7. **Sign-Off:**

      A. **Physical Verification:** Physical verification is a critical step in the semiconductor chip design process, specifically in the RTL to GDS2 (GDSII) flow. It involves a series of checks and analyses to ensure that the physical layout of the chip adheres to design rules, manufacturing constraints, and reliability criteria. Physical verification helps identify and rectify potential issues in the layout that could lead to manufacturing defects, performance problems, or reliability issues.

     B. **Timing Verification:** Timing verification is a critical step in semiconductor chip design within the RTL to GDS2 (GDSII) flow. It focuses on ensuring that the design meets its timing requirements, particularly in terms of clock-to-q delays, setup times, hold times, and maximum clock frequency. Timing verification helps guarantee that the chip will operate correctly and within its specified performance limits.

OpenLANE
=========

OpenLane is an open-source toolchain for chip design that automates the process of creating custom digital integrated circuits, from high-level RTL code to manufacturable GDSII files. Developed by efabless, it streamlines the design flow by integrating various open-source EDA tools, allowing users to explore different design options, meet manufacturing requirements, and even experiment with custom chip designs. OpenLane's scripted flow, community-driven development, and accessibility make it a valuable resource for both educational purposes and small design teams looking to create custom ASICs while adhering to industry best practices.

OpenLANE ASIC Flow
=================

OpenLane is a fully automated process, spanning from RTL (Register-Transfer Level) to GDSII (Graphics Data System II), and relies on various components, including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, KLayout, and a set of specialized scripts for design exploration and enhancement. This comprehensive flow covers every step of ASIC implementation.

OpenLANE utilises a variety of opensource tools in the execution of the ASIC flow:

1. RTL Synthesis & Technology Mapping: yosys,abc
2. Floorplan & PDN:init_fp, ioPlacer, pdn and tapcell
3. Placement:RePLace, Resizer, OpenPhySyn & OpenDP
4. Static Timing Analysis:OpenSTA
5. Clock Tree Synthesis:TritonCTS
6. Routing:FastRoute and TritonRoute
7. SPEF Extraction:SPEF-Extractor
8. DRC Checks, GDSII Streaming out:Magic, Klayout
9. LVS check:Netgen
10. Circuit validity checker:CVC

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



</details>
