# VSD Physical Design Summary | Kanika Taneja
<details><summary> <h1> D1_SK1 - How to talk to computers </summary><p>


<details><summary> <h2> :book: L1 - Introduction to QFN-48 Package, chips, pads, core, die and IPs</summary><p>		
The QFN-48 Package with a chip in the center is shown below. Connections are made to the boundaries of the chip through wirebonds.
		
![QN-48 Package with chip](https://user-images.githubusercontent.com/57150778/218431254-de324341-4bf1-43e9-97b5-6472ea4ce357.png)

Contents of a chip : 

i) Pads : These cells act as interfaces for signals travelling in and out of the chip.
ii) Core : This is where all the digital logic is placed.
iii) Die : It is the size of the entire chip; manufactured on Si wafer. 


![image](https://user-images.githubusercontent.com/57150778/218431961-da71e90b-a5a7-4345-aa84-be8762b517bd.png)

The two kinds of blocks on an SoC are Macros and IPs (Intellectual Properties). Macros are completely digital logic, while IP are blocks that need some level of intelligence to build.

</p>
</details>

<details><summary> <h2> :book: L2 - Introduction to RISC-V</summary><p>

### RISC-V Instruction Set Architecture (ISA)
It is the language of the computer. For a C-program to run on the hardware, the C-program is first compiled in its assembly language program. This assembly language program is converted to machine language program, (binary) which is basically electronic signals (0s and 1s) which are understood by the hardware of the computer. 

A hardware description language (HDL) is needed to replicate the Instruction Set Architecture using some RTL. HDL is the interface present between RISC-V architecture and the layout. 

![image](https://user-images.githubusercontent.com/57150778/218434578-debae85b-4371-4700-8541-730d258c081d.png)
</p>
</details>

<details><summary> <h2> :book: L3 - From Software Applications to Hardware </summary><p>	

Applications or software in our devices are implemented in the hardware chips, as described below:


Applications or software in out laptops / mobile phones are implemented in the hardware chips, like shown. 

System Software converts the application program into binary language. There are various layers of a system software.
The major components of a system software are:

1) OS: 
It handles IO operation, allocates memories. Majorly, it converts the application software to assembly language program and finally to binary language program so that it is understood by the hardware.
The outputs of the operating system are small functions in C/C++ or Java. These are input to the compiler.
		
2) Compiler : 
It converts the application software in C, C++ or Java into instructions (*.exe file),  the syntax of which depends on the type of hardware. For ex: If the hardware belongs to intel-X86, the instructions belong to X86; etc. 
		
3) Assembler:
An assembler takes in instructions and converts it into respective machine language program (Binary numbers: 1s and 0s). This binary language is fed to the hardware and accordingly, the hardware generates the output. 

![image](https://user-images.githubusercontent.com/57150778/218435817-f64b1f72-dc22-4d6e-abd6-749e1872040d.png)
</p>
</details>

</p>
</details>


<details><summary> <h1> D1-SK2 SoC design and OpenLANE  </summary><p>

<details><summary> <h2> :book: L1 - Intro to all components of open-source digital asic design  </summary><p>	

Digital ASIC design requires:

1) HDL : RTL of the function we want to implement including the RTL of all used Ips
2) CAD Tools used for electronic Design Automation (EDA)
3) Process Design Kits (PDK)

![image](https://user-images.githubusercontent.com/57150778/218443990-93629905-0fd2-46af-a6c7-cf76523a4cbd.png)


<h3> PDK </h3>

PDK acts as an interface between fabrication companies and designers. Earlier, design if an IC wad closely twined with process design kits owned by companies like TI. The need for separating design from fabrication technology led to creation of open-source PDKs (Processs Design Kits).

PDK is a collection of files for modelling the fabrication process for EDA tools. It consists of:
1) Process Design Rules - LVS, DRC checks
2) Device Models
3) Digital Standard Cell Libraries,
etc

Skywater 130nm open-pdk was introduced by Google and skywater, enabling complete ASIC design process to be open-source. 

Sky-130 nm tech node, despite being old; is still in use because of two main reasons:
1) Several applications don't need faster, advanced nodes. 130nm process has good enough performance to fit such applications. A pipelined version of RV32i CPU can achieve GHz clock. 
2) 130nm fabrication process is cheaper compared to advanced nodes.

</p>
</details>

<details><summary> <h2> :book: L2 - Simplified RTL-GDS Flow  </summary><p>

Major Implementation Steps in ASIC design flow are:
* Synthesis
* Floor/Power Planning
* Placement
* Clock Tree SYnthesis
* Routing
* Sign Off

The event of producing final layout is called "Tapeout".
![image](https://user-images.githubusercontent.com/57150778/218445851-b6ac990e-6b20-444f-9d43-2164cd7ab79a.png)

<details><summary><h3> Synthesis </h3></summary><p>

Design has to be translated into circuits having components from standard cell libraries (SCL).
The resultant circuit is a gate level netlist. It is functionally equivalent to the RTL.

![image](https://user-images.githubusercontent.com/57150778/218446318-07d5a251-0fbb-441b-b0c9-ab6d3353850b.png)

Standard Cells have a regular layout; typically all the same height with varying widths. It is an integer multiple of units called the "site width".
Each cell comes with different models or views utilized by different tools:

	1) Liberty view : electrical models like delay, power model
	2) HDL behavioral models for the cells
	3) SPICE or CDL view
	4) Layout View:
		i. GSDII (detailed View)
		ii. LEF View (abstract view)
        
![image](https://user-images.githubusercontent.com/57150778/218446473-f6b09bdd-dd3c-44ed-8125-99ee1ddeb7cb.png)
</p>
</details>

<details><summary><h3> Floor & Power Planning </h3></summary><p>

Floor and Power planning mean different things based on whether we are implementing a single component of the design (Macro) or the whole chip.
The objective is to plan the silicon area and create a robust Power Distribution Network to power the circuits.

<h4> Floorplanning </h4>

Chip Floor-Planning : The chip die is partitioned between different blocks and IO Pads are placed.

Macro Floor Planning : Macro dimensions, pin location and rows are defined. 

<h4> Power Planning </h4>

The power delivery network is constructed. The chip is powered by multiple VDD and Ground Pins. The power pins are connected to all components through rings and vertical and horizontal straps. Such parallel structures are meant to reduce the resistance and thus the IR drop; also the electromigration. Typically, the PDN used upper layers as they are thicker than lower metal layers.

![image](https://user-images.githubusercontent.com/57150778/218448464-1f20e51a-648b-4433-b8c2-a57de4fe4fe9.png)

</p>
</details>

<details><summary><h3> Placement  </h3></summary><p>

For macros we place std cells into rows, aligned with sites. Connected cells should be placed close to each other to reduce interconnect delays and to enable successful routing. 

![image](https://user-images.githubusercontent.com/57150778/218447701-c0d632c5-c5ff-4046-ad2e-c79d71d8c066.png)

Typically placement is done in 2 steps:

1) Global Placement : Finding optimal position for all cells. These positions are not necessarily legal.
2) Detailed Placement: The placement from global placement is minimally altered to be legal.
    
![image](https://user-images.githubusercontent.com/57150778/218447934-5bbc9237-41a2-457b-863d-88c51ac8ac75.png)

</p>
</details>

<details><summary><h3> Clock Tree Synthesis  </h3></summary><p>

Clock tree routing needs to be done before signal routing by creating a cock distribution network.
The goal is to deliver clock to all sequential cells while minimizing skew. This is typically implemented using an H-tree, X-tree or a fishbone structure.

![image](https://user-images.githubusercontent.com/57150778/218449271-6966e5c4-8bcd-446f-a6b6-48e5daf75597.png)

</p>
</details>

<details><summary><h3> Signal Routing  </h3></summary><p>

For each metal layer, the PDK defines:
	
i. Thickness
ii. Pitch
iii. Tracks
iv. Min width
v. Vias

![image](https://user-images.githubusercontent.com/57150778/218451009-82be9d51-83c5-4ed7-b309-04505eed0f63.png)


Most routers are grid routers, i.e., they create routes over existing tracks. Since the routing grid can be huge, the signal routing is done in two setps:

* Global Routing : Routing guides are generated.
* Detailed Routing : Routing guides are used to create actual wires. 

</p>
</details>

<details><summary><h3> Sign-Off  </h3></summary><p>

Signoff involves-

* Physical verifications:
    - Design Rule Checking (DRC) : To check that all design rules are satisfied.
    - Layout Versus Schematic Checks (LVS) : To ensure that layout is functionally equivalent to gate netlist.
* Timing Verification:
    - Static Timing Analysis (STA) : To make sure that all timing constraints are met and circuit will run at designated frequency.

</p>
</details>

</p>
</details>

<details><summary><h2> 📖 L3 - OpenLANE & Strive Chiplets  </h2></summary><p>

Using open source EDA tools presents a set of potential problems such as tool qualification, calibration, or missing tools for certain intermediate steps.
OpenLANE encounters this problem by presenting an Open-Source Flow for a True Open Source Tape-out experiment. 

An example of open everything SoCs is striV3. It makes use of open pdk, EDA tools as well as RTL. 

![image](https://user-images.githubusercontent.com/57150778/218457206-f20b49b0-c61e-474c-b614-d288fee14021.png)

Its SoC are present in various versions as described below:
	
![image](https://user-images.githubusercontent.com/57150778/218457450-010732c8-3932-4bc8-ba44-c1980f2af094.png)

	
The main objective of an open source EDA FLow is to produce a clean GDSII with no human intervention. This implies:

* No LVS Violations
* No DRC Violations
* No Timing Violations

OpenLANE presents a containersed set of tools that are containerized to function out of the box. It has two modes of operation:

* Autonomous
* Interactive
	
It can also be used for finding the best set of flow configurations for a particular design.
	
</p>
</details>

<details><summary><h2> 📖 L4 - Introduction to OpenLANE detailed ASIC Design Flow </h2></summary><p>

OpenLANE ASIC Flow : 
![OpenLANE ASIC Flow](https://user-images.githubusercontent.com/57150778/218458792-20a4b5e4-7e34-4f43-b772-46b8921b07e8.png)


<h3> RTL Synthesis </h3>
	
RTL is fed to yosys using design constraints. Yosys translates the RTL into a logic circuit using generic components.
This circuit is optimised and mapped into a standard cell library using abc. ABC has to be guided using abc scripts.
OpenLANE comes with various abc scripts referred to as "synthesis strategies". The strategies target the best area or could target the best timing, etc.

<h3> Synthesis Exploration (Utility) </h3>
	
Used to generate reports that show how the design delay and area is affected by the synthesis strategy (S1, S2, …S8). Based on this, we can pick the best strategy to continue with. 

![image](https://user-images.githubusercontent.com/57150778/218459424-8bcb6fbc-f96f-4aeb-9118-1c6949088772.png)


<h3> Design Exploration (Utility) </h3>
	
Used to sweep design configurations (>16). It generates a report as shown below that has more than 35 design metrics. Also shows the number of violations generated after generating the final layout. 
This is useful to find the best configuration for openLANE for any given design. Thus it is recommended to explore the design first and then used the obtained best configuration for this design going forward.
Ex : Exploration to find a configuration that gets a clean layout.


![image](https://user-images.githubusercontent.com/57150778/218459824-daa5b467-466a-4a04-b979-380a829ddabb.png)


<h3> Design For Testing </h3>

After synthesis we can insert a testing structure is we want our design to be ready for testing after fabrication. We can insert a scan chain using open src project Fault. It can perform:

* Scan insertion
* Automatic test Pattern Generation (ATPG)
* Test Patterns COmpaction
* Fault Coverage
* Fault Simulation

![image](https://user-images.githubusercontent.com/57150778/218460233-ea5be554-235f-4896-a62f-8c5eadfe271e.png)

	
<h3> Physical Implementation </h3>
	
It is also called automated PnR (Place and Route). It has several steps performed by the OpenROAD App.
* Floor/Power Planning
* End Decoupling Capacitors and Tap cell insertion
* Placement : Global and Detailed
* Post placement Optimization
* Clock Tree Synthesis (CTS)
* Routing : Global and Detailed
	

<h3> Logic Equivalence Checking | by Yosys </h3>
	
Every time a netlist is modified by CTS, post route optimization, etc; it must be checked for funtionality against the gate level netlist post synthesis.
	
	
<h3> Dealing with Antenna Rules Violation </h3>
	
A fabricated metal wire connected to transistor gates acts as an antenna. Charge can accumulate on it and can damage transistor gate during fabrication. To address this, the length of the wires must be limited. This is ensured by the router. However, if the router fails, there are two solutions : 
	
1) Bridging : Attatchign a higher intermediary layer
	
![image](https://user-images.githubusercontent.com/57150778/218462046-f78a338a-1eaa-4665-a191-db4a44164f75.png)

2) Adding Antenna diode cell to leak away charges
	
![image](https://user-images.githubusercontent.com/57150778/218462377-3f6ff250-d9b5-4a6f-a049-90d9e3087740.png)

As a preventative approach, OpenLANE adds a fake antenna diode next to all cells after placement. Antenna checks are done post routing (Magic). Finally, the fake antenna cells next to violating instance pins are replaced with real antenna cells from the SCL. 
	
![image](https://user-images.githubusercontent.com/57150778/218463312-fac9b846-9c9b-4b80-b792-aa142479170d.png)

	
<h3> SignOff </h3>
	
SignOff involves STA, DRC and LVS checks.

Timing Signoff is done by performing RC extraction to generate spef file. Then, STA is done using OpenSTA to generate timing reports.

![image](https://user-images.githubusercontent.com/57150778/218463959-af2c8a36-c684-441a-a90f-3732bdea81f7.png)
	
	
DRC is perfomed in magic. LVS checks are performed in Netgen and Magic.
	
	
	
</p>
</details>
	
	
</p>
</details>
