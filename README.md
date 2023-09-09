# Advanced-Physical-Design
--------------------------

"Advanced Physical Design" represents an intricate domain within digital integrated circuit engineering. It entails the intricate process of transforming a high-level logical description of an electronic circuit into a meticulously optimized layout, considering performance, power efficiency, and area utilization. OpenLANE, an open-source ASIC design flow, combined with the Sky130 Process Design Kit (PDK), has become a powerful toolkit for realizing this goal. Starting with RTL design, the process encompasses synthesis, floor planning, placement, clock tree synthesis, routing, physical verification, and timing closure. Advanced physical design goes a step further, incorporating sophisticated techniques such as custom library cell design and power network optimization. These tools and methodologies have empowered engineers and researchers to explore cutting-edge semiconductor design, enabling innovation in the development of complex integrated circuits.

## DAY-1: Inception of open-source EDA, OpenLANE and Sky130 PDK
<details> 
      <summary> How to talk to computers </summary>

---
At some part of our life, we have all used an ARDUINO board. We all know that an Arduino board is a popular open-source hardware platform designed for electronics enthusiasts, hobbyists, students, and professionals to create and prototype a wide range of embedded systems and electronic projects. Arduino boards are known for their ease of use and versatility, making them a valuable tool for learning about electronics and programming.

![arduino](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/2c4d152e-9a76-47c3-9631-0a137711f6b7)

---

The Block diagram of an ARDUINO board is as shown below.

---
![Arduinno_Block](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/c63c0919-4e07-4757-80e7-d0fafa25d769)

In this course, instead of looking into the embedded design we will be focusing more on the chip used inside the embedded systems.

---
![Chip](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/443610f2-ab16-41ce-b448-e2ee2bdf7dcc)

- **Package:** In chip design, a "package" refers to the protective outer casing that houses and safeguards the integrated circuit (IC). Packages serve critical roles in chip manufacturing by providing physical protection to the silicon die, establishing electrical connections between the chip and external components or a printed circuit board (PCB), aiding in thermal management by dissipating heat, and offering mechanical support. These packages come in various types, such as Dual In-line Packages (DIP), Surface-Mount Device (SMD) packages, Small Outline Integrated Circuit (SOIC) packages, and more, each tailored to specific applications and requirements, making package selection a crucial consideration in chip design and manufacturing.

- **Chip:** A "chip," also known as an integrated circuit (IC) or microchip, is a miniature electronic device that consists of a collection of electronic components, such as transistors, resistors, and capacitors, etched onto a single semiconductor material, typically silicon. These components are interconnected to perform specific functions, such as processing data, storing information, or controlling electrical signals.

The "chip" is the silicon-based microelectronic component that contains electronic circuits, while the "package" is the protective outer casing that houses the chip, provides electrical connections, and offers physical protection and thermal management.
  
</details>
