# VSD Physical Design Summary | Documentation
<details><summary> <h1> D1 - Inception of open-source EDA, OpenLANE ans Sky130 PDK </summary><p>

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


<details><summary><h1> D1-SK3 - Get familiar to open-source EDA tools  </h1></summary><p>
<details><summary><h2> :computer: L1 - OpenLANE Directory structure in detail </h2></summary><p>

Working dir : 

![image](https://user-images.githubusercontent.com/57150778/218501720-dd6a2e5e-a965-4620-9926-41837ffa2314.png)



![image](https://user-images.githubusercontent.com/57150778/218501794-855f4a1a-8866-45c4-a727-b34393be8e32.png)

skywater-pdk : has all pdk related files - tech lef, tech files, std cell lefs, etc; made to work with commercial EDA tools.

open_pdks : It has a set of scripts and files to convert foundry level pdk to be compatible with open source EDA tools. 

sky130A : PDK variant made compatible to open source environment.

![image](https://user-images.githubusercontent.com/57150778/218515570-11530b38-b588-4e60-b6b2-dffaff619760.png)
![image](https://user-images.githubusercontent.com/57150778/218516877-6ee3ceca-89c2-46ed-a33d-b41e87738373.png)

Similarly, std cell lef and tech lef are present in:
![image](https://user-images.githubusercontent.com/57150778/218518481-aa224e8e-87b5-4314-b380-c63ea0cd0030.png)

work dir : 

![image](https://user-images.githubusercontent.com/57150778/218501861-fd5e6c65-e2e6-4640-a747-548ca08ab4ad.png)

	
</p>
</details>


<details><summary><h2> :computer: L2 - Design Preparation Step </h2></summary><p>

In the work directory openlane/designs many designs are present. The current design is picorv32A.

![image](https://user-images.githubusercontent.com/57150778/218528721-fa5f883b-28a3-473f-b5f2-622ed2cb680b.png)

The src directory consists of rtl netlist for the design and sdc file.

![image](https://user-images.githubusercontent.com/57150778/218530365-66ba4807-0614-4d84-a3f9-e9e933aaf039.png)

sdc file contents : The target clock period for the design is 20ns (50 MHz)

![image](https://user-images.githubusercontent.com/57150778/218530684-10c8bc4b-30d7-4cd8-aca2-e1a3443c5e6b.png)

config.tcl overrides default switches of openlane flow:

![image](https://user-images.githubusercontent.com/57150778/218531097-d94e1a30-582b-4d74-97a2-08fa649b5057.png)

The settings in sky130_fd_sc_hd_config.tcl override the switches in config.tcl such that the final clock period setting is 24.73 ns.

![image](https://user-images.githubusercontent.com/57150778/218532159-617bc5b9-b0ae-4d4f-abb0-4379174b4794.png)

<h3> Design Preparation </h3>

![image](https://user-images.githubusercontent.com/57150778/218534533-54cef366-b7c2-40c7-b29b-36aba797a1b6.png)



</p>
</details>

<details><summary><h2> :computer: L3 - Review Files After Design prep and run synthesis </h2></summary><p>

All the input lefs are merged into one file using mergeLef.py. The resultant merged.lef is present in:

![image](https://user-images.githubusercontent.com/57150778/218534885-4e97a7f0-2e91-4764-bd95-edf3c1c6598c.png)

The output config file containing all the switches applied is also present in the runs/<timestamp> directory:

![image](https://user-images.githubusercontent.com/57150778/218542857-2fa831d4-a522-4ce9-806a-bd3d69254825.png)

The cmds.tcl file contains commands run in the tool:

![image](https://user-images.githubusercontent.com/57150778/218542959-c8e43e54-dc2e-45dc-93eb-7b072804d136.png)

<h3> run_synthesis </h3>

![image](https://user-images.githubusercontent.com/57150778/218543049-b500a557-f3ab-4f73-95b6-889bf1d8683a.png)


</p>
</details>

<details><summary><h2> :computer: L5 - Steps to characterize synthesis results </h2></summary><p>

The resultant synthesized gate netlist and the mapped lef file are present in the runs/13-02_17-46/results/synthesis dir:

![image](https://user-images.githubusercontent.com/57150778/218543575-095518b9-93f4-486f-a4f1-3cdb521a2629.png)

synthesized netlist : 

![image](https://user-images.githubusercontent.com/57150778/218545405-7d262cca-6de3-40a5-a1bf-56dddc506b11.png)

The cell stats and timing reports can be seen in runs/13-02_17-46/reports/synthesis dir:

![image](https://user-images.githubusercontent.com/57150778/218543757-0cccd44a-87d6-4498-8e21-0459ef4e4972.png)

The cell stats are present in 1-yosys_4.stat.rpt

![image](https://user-images.githubusercontent.com/57150778/218545105-b2333545-b793-4a99-8bd0-ebd0dfe6d727.png)

The timing status post synthesis can be seen in 2-opensta.timing.rpt.The worst paths are reported in a descending order or negative slack.

![image](https://user-images.githubusercontent.com/57150778/218545476-02550855-cb17-49c5-8453-defd056edcc3.png)

Since the timing is violated for setup, synthesis is performed again by increasing target clock_period to 50. 

![image](https://user-images.githubusercontent.com/57150778/219638404-ac9b9929-67cf-474d-bca1-97c1eaefd3ba.png)

The new timing rpt for max path is shown below:

![image](https://user-images.githubusercontent.com/57150778/219643149-17393532-7176-4162-a0dc-db67b7ac4d7a.png)

New cell stats : 
	
![image](https://user-images.githubusercontent.com/57150778/219644413-f7518fe5-0bbc-4d82-8596-cdbc410b2c64.png)

area : 
	
![image](https://user-images.githubusercontent.com/57150778/219644786-a94a7a70-7d5e-494c-9813-e4b75ee65696.png)
	
	
</p>
</details>

</p>
</details>

</p>
</details>

<details><summary><h1>D2 - Good floorplan vs bad floorplan and introduction to library cells</h1></summary><p>

<details><summary><h1> D2-SK1 - Chip Floor planning Considerations </h1></summary><p>

<details><summary><h2> :book: L1 - Utilization Factor and Aspect Ratio </h1></summary><p>

1) Define width and height of core and die:
First step of physical design flow is to define the width and height of the core and the die.

<img src="https://user-images.githubusercontent.com/57150778/219647019-f9e03b89-7916-480d-ba27-91d2f7481272.png" width="450">

Consider a basic netlist as shown : 
	
<img src="https://user-images.githubusercontent.com/57150778/219647202-c0c18d24-fc05-41d8-ac63-b5f48e52720d.png" width="350">
			
We use the physical dimensions of the std cells to calculate the total area occupied by the netlist on the silicon wafer.  For ex : min area occupied by the current netlist:
	
<img src="https://user-images.githubusercontent.com/57150778/219647401-6835ae82-95f9-4528-b556-554751e79702.png" width="250">

Consider a silicon wafer with many dies. A die is a small semiconductor material specimen on which the fundamental circuit is fabricated. The die contains the core on which all digital logic is placed.

<img src="https://user-images.githubusercontent.com/57150778/219648341-878d6791-d2fd-4df3-b93f-7cd9e0a38c8e.png" width="350">
	
Suppose we select the core area such that the netlist occupied the core completely (100% utilization).

<img src="https://user-images.githubusercontent.com/57150778/219648277-902d44c8-1d27-4dab-81af-2cba7bc5df83.png" width="200">
		
Utilization Factor = (Area occupied by the netlist) / (Total area of the core)
		
In this case, the utilization factor = 1.
Practically, we go for 50-60% utilization. U.F. = 0.5-0.6. The remaining area is left for optimization, placing additional cells, etc.
		
Aspect ratio = Height / Width
				
In this case, aspect ratio = 1, implying that the chip is a square shape.


</p>
</details>

<details><summary><h2> :book: L2 - Concept of Pre-placed cells </h1></summary><p>

1) Defining Locations of Pre Places Cells
	
	a. What are pre placed cells?
		Consider some combinational logic cloud that translates to a large number of gates (50K-100K).
		We need not implement this as a part of the main circuitry. We can implement is separately; or even granularize the circuit itself (dividing the 100K gates into two blocks each of 50K gates).

<img src="https://user-images.githubusercontent.com/57150778/219649097-e6fea8df-ed5d-4061-8d1c-7670c018b5e8.png" width="500">

<img src="https://user-images.githubusercontent.com/57150778/219649178-a151fe8e-eefb-48fa-b1c9-cfdac5d76c7e.png" width="300">

We can now implement both these blocks independently. The IO pins are extended to the boundary and then we can Blackbox the two modules such that the internal circuitry is no longer visible.

<img src="https://user-images.githubusercontent.com/57150778/219650594-e6f49c3e-2488-4622-a718-a1e431ba2b3e.png" width="450">

<img src="https://user-images.githubusercontent.com/57150778/219650718-4dc8e988-cd29-4abf-a2b8-b1f04ab8f375.png" width="400">
	
These blocks when implemented separately, can be re-used in the top-level netlist multiple times.
			
Similarly IPs like memory, clock gating cell, comparator, mux, etc are available which can be implemented once and instantiated multiple times onto the netlist. 
These cells are placed onto the chip and their placement is fixed before the actual placement of std cells. Thus these are referred to as Pre-Placed Cells.
			 
b. Defining placement of Pre-Placed Cells:
	
We look at the placement of IO pins for the entire block and the interaction of the blocks with the remaining core logic to decide where to fix the position of the pre-placed cells.

<img src="https://user-images.githubusercontent.com/57150778/219650924-803af858-2216-49ff-bc4c-37a932de81f8.png" width="500">


</p>
</details>

<details><summary><h2> :book: L3 - De-coupling Capacitors </h1></summary><p>

Once the positions of pre-placed cells are fixed, we need to surround them with decoupling capacitance.
	
<h3>Need for decaps </h3>
	
When a logic cell switches, suppose it goes from 0 ->1, its internal capacitors need to be charged to represent logic 1. And this charge is provided by the supply voltage. Thus the VDD supply needs to supply the charge to all cells switching from logic 0 to logic 1.
Similarly, the VSS is responsible to handle all the discharge current for cells switching from logic 1 to logic 0.
But since there is a voltage drop across the power grid, the voltage that appears at std cells is lower (0.7 or 0.8 volts, say). Thus the internal capacitances cannot be charged to more than 0.8 volts. In order for this 0.8 volts to be detected as logic 1, it should be within the noise margin range of output logic. 

<img src="https://user-images.githubusercontent.com/57150778/219651832-0103ae31-05b1-47a3-a12d-a090d4a6809d.png" width="500">

<h3> Noise Margin </h3>
For any signal to be detected as logic 1, it needs to lie between Vih and Voh range, and so on….
		
Vil to Vih is undefined region as a signal appearing in this range can convert to any logic level. This is an issue due to a large physical distance from the main power supply to the std cells under consideration.

![image](https://user-images.githubusercontent.com/57150778/219652078-4c26d6fd-b871-4697-8478-180cdc2facd0.png)

Decoupling Capacitors:
	These are huge capacitors which are completely charged to the power supply. When the circuit switched, it can get the required current from the decoupling capacitor, since these are placed physically close to the logic circuitry and help to decouple the logic from power supply. 
	The decaps replenish the charge when surrounding cells are not switching. 

<img src="https://user-images.githubusercontent.com/57150778/219652229-524d13b7-cabd-4401-896f-adddfc64c9f3.png" width="500">

Pre placed cells are thus surrounded by decaps.

<img src="https://user-images.githubusercontent.com/57150778/219652318-9326b451-4053-4d71-b159-88c2266041d1.png" width="400">

	
</p>
</details>

	
<details><summary><h2> :book: L4 - Power Planning </h1></summary><p>

Decaps take care of local communication. For global communication, we need power planning. Suppose a macro o/p (16 bit bus) is input to another macro, where it is inverted. The goal is to ensure that the shape of the signal is maintained from the driver to the receiver.
	
<img src="https://user-images.githubusercontent.com/57150778/219652942-5fa64e7c-8819-49d2-9609-fcfa5e504d66.png" width="600">

	
All power lines are tapped to VDD and all ground lines are tapped to ground. Since we can't have many decaps placed all over the chip, the power supply needs to supply the power to retain the signal shape from driver to receiver. The power supply is distance from the signal line so there is possibility of <b>voltage drop</b>. Assuming the signal to be a 16 bit bus being inverted. Initially, each bit of the line is a capacitor charged to VDD or discharged to ground. When all VDD caps discharge to 0 and all caps at 0 charge to VDD; since we have a single ground line for all bits, we observe a bump in the voltage. If this bump voltage exceeds the noise margin, it may lead to undefined state. This phenomena is called <b>ground bounce</b>.
	
<img src="https://user-images.githubusercontent.com/57150778/219652991-ea961534-9516-4738-8dc1-f945c0fe6f2a.png" width=500>

	
Similarly when many caps charge at the same time through the same line, we may observe a voltage droop. This can also lead to an undefined state if it goes lower than noise margin. 
	
<img src="https://user-images.githubusercontent.com/57150778/219653122-47a28e6d-6644-40a7-9402-ac42adc4ea6e.png" width="500">

	
Both these problems arise since the power supply comes from only one point. This can be solved by having multiple power supplies to provide charging current and multiple ground lines to drain discharging current. This is a PG mesh.
	
<img src="https://user-images.githubusercontent.com/57150778/219653231-eac58bf8-7eb6-4d7d-a26a-47fc48963863.png" width="450">

<img src="https://user-images.githubusercontent.com/57150778/219653334-a5969fca-6eba-4790-9d7f-c0d348429462.png" width="450">


</p>
</details>	

<details><summary><h2> :book: L5 - Pin placement and logical cell placement blockage </h1></summary><p>

Consider the following circuit where blocks a, b and c are preplaces cells. The connectivity information of different gates is available in the netlist. 
	
<img src="https://user-images.githubusercontent.com/57150778/219654331-089cac84-43fd-4ccb-a2f0-4aab4cf94a19.png" width="400">

	
Suppose we put all IP ports on left and OP ports on the left. The ordering of IP and OP ports depends on where we plan to place the cells. Pin placement needs good understanding of the functionality of the design. This creates a handshaking between the frontend and backend team. 
	
<img src="https://user-images.githubusercontent.com/57150778/219654443-bb9bfb95-4446-4713-bcb5-e61dd3d5ed77.png" width="400">
	
The clock ports are bigger than signal ports since these drive the flops in the complete chip continuously. Bigger ports offer lower resistance. 

<h3>Logical Cell Placement Blockage:</h3>

Next, we add a logical cell placement blockage in the area outside the core since this area  is reserved for IO pins.

<img src="https://user-images.githubusercontent.com/57150778/219654549-e3afdd47-ff60-4206-97b4-3f0abf3a8201.png" width="400">

</p>
</details>

<details><summary><h2> :computer: L6 Steps to run floorplan using OpenLANE </h1></summary><p>

Information about all the available switches is present in README.md:

![image](https://user-images.githubusercontent.com/57150778/219656292-5a294f12-f695-4133-a909-798c1a97f559.png)

floorplan.tcl in configuration/ contains all the switches applied:

![image](https://user-images.githubusercontent.com/57150778/219657567-9e89c978-e5c2-4fff-aba9-f596ae1da759.png)

![image](https://user-images.githubusercontent.com/57150778/219658100-9afde7ff-e1ce-45af-9337-e0f5c7e2f8c6.png)

Running floorplan in OpenLANE:

![image](https://user-images.githubusercontent.com/57150778/219659187-bb008a0a-c630-4d92-8726-e949418568be.png)

	
</p>
</details>

<details><summary><h2> 💻: L7 - Review floorplan files and steps to view floorplan </h1></summary><p>

The core and die_area can be viewed in reports:

![image](https://user-images.githubusercontent.com/57150778/219694028-d4d6036d-9eaa-48a0-b9f1-ae03b00e4a09.png)

Floorplan def contents:
	
1) Die Area and std cell rows:
	
<img src="https://user-images.githubusercontent.com/57150778/219695202-aa1882b6-00a9-43bd-bf75-633a3daa4b43.png" width=500>
	
2) Tracks for metal routes

<img src="https://user-images.githubusercontent.com/57150778/219695587-18536270-d04c-4b5e-a8a3-ea6ab15f0402.png" width=400>
	
3) Std cells - unplaced

<img src="https://user-images.githubusercontent.com/57150778/219695822-dea8f3d0-1d19-4543-900a-b414f0ec4730.png" width=350>

4)  Decaps and tap cells placed and fixed:

<img src="https://user-images.githubusercontent.com/57150778/219696270-5f613771-7934-44ad-82fc-7e5e3b38b675.png" width=450>
	
5) Placed IO Pins

![image](https://user-images.githubusercontent.com/57150778/219697054-957aeaa1-a496-4cf5-8014-6411d1857e90.png)

6) Signal nets with logical connectivity-
	
![image](https://user-images.githubusercontent.com/57150778/219697356-7b2067dc-f967-44ec-a3b4-57e09cb05482.png)

The finally applied switches for core utilization, and horizontal and vertical metals for IO pins can be viewed in config.tcl:
	
![image](https://user-images.githubusercontent.com/57150778/219699348-e55e1acb-6d53-4396-929a-bde9c553626f.png)

	
</p>
</details>

<details><summary><h2> 💻: L8 - Review floorplan layout in magic </h1></summary><p>

Launching magic to view floorplan:

![image](https://user-images.githubusercontent.com/57150778/219699843-5c4d0c79-e770-46f0-9545-3709257b524d.png)

Reviewing Floorplan:

![image](https://user-images.githubusercontent.com/57150778/219724780-adc88d8a-5c92-4a41-b7dc-02b868732050.png)

Zooming in to show pin placement, decap cells and tap cells. All the logic cells are unplaced.


![image](https://user-images.githubusercontent.com/57150778/219726746-74989591-5ad7-4726-a484-0b7aa0b3f16d.png)

	
</p>
</details>

</p>
</details>

<details><summary><h1> D2_SK2 - Netlist Binding and placement </h1></summary><p>

<details><summary><h2> :book: L1 - Netlist Bindign and initial place design </h2></summary><p>

Binding netlist with physical cells

All cells in the netlist are rectangular blocks, whose sizes, delays, functionality, etc are defined in the library.
	
<img src="https://user-images.githubusercontent.com/57150778/219709788-7c26d367-8fc7-4f57-bc5d-6bc0a29146c0.png" width=400>

The library also has various flavors of the cells, which might be varying in sizes and delays because of having lower resistance paths.
	
<img src="https://user-images.githubusercontent.com/57150778/219709839-872ea2a9-c01e-4a6e-a42e-17b0c92947f5.png" width=500>

	
The obtained shapes and sizes of each gate are placed on the floorplan. Pre-placed cells are already present. It is taken care that pre-placed cells are not touched, and no other cells are placed in that area.
Cells interacting to IO ports are placed close to them. Additionally, interacting gates are placed close to each other to minimize route lengths and thus signal delays. 

<img src="https://user-images.githubusercontent.com/57150778/219710014-b579f973-b98d-4f58-ba78-8f29e7107f89.png" width=500>
	
</p>
</details>

<details><summary><h2> :book: L2 - Optimize placement using estimated wirelength and capacitance  </hs></summary><p>
	
Since some interacting cells are placed far away, we calculate the capacitances of estimated wire lengths. Long routes can read to loss of signal strength. Thus, to maintain signal integrity, we add repeaters in long routes. This increases the area of the floorplan.
Higher the value of the wire cap, worse the slew since charge needed to charge the capacitor is high. 

<img src="https://user-images.githubusercontent.com/57150778/219710457-94168a36-2391-4750-aaf8-eba82c0f3e3b.png" width=500>

We need to optimize this to minimize the number of repeaters

	
</p>
</details>
	
<details><summary><h2> :book: L3 - Final Placement Optimization  </hs></summary><p>
	
The repeaters reproduce the signal and send it to the required logic cell. Certain logic can be abutted to minimize signal delays, if the logic works at very high speed, etc. 

<img src="https://user-images.githubusercontent.com/57150778/219710936-939da12f-ee29-443d-90db-2b286fe4082d.png" width=500>

Once placement is optimized by adding buffers on long routes, since the clock tree has not been built, we need to check the setup timing analysis of data path. This assumes ideal conditions that all route delays and clock arrival time to flipflops are zero.  We need to make timing meet at this stage since routing would make the timing worse.

	
</p>
</details>	

<details><summary><h2> :book: L4 - Need for libraries and characterization  </hs></summary><p>
	
Synthesis is the first step in ASIC design flow in which we reproduce the functionality of an RTL using legal hardware. The next step is floor-planning. Here we decide the size of the core and die. This completely depends on area covered by the gates in the netlist and thus, depends on the shapes and sizes of standard cells. Next, during placement, we need to place the logic cells such that the initial timing is met. Next, during CTS, we need zero skew on clock pins across the entire design. Here we need clock buffers to ensure that clock signal has equal rise and fall times. Finally while routing, certain properties of the cells need to be taken care of and determine the type of routing. In signoff Static Timing Analysis, we need timing tables for all std cells. In all the steps, the properties of logic gates are important. These gates are collectively available in a library. Hence, library characterization is important.
	
	
![image](https://user-images.githubusercontent.com/57150778/219711315-58055fa8-6c78-4e28-a133-74f39e4993d3.png)


</p>
</details>
	
<details><summary><h2> :computer: L5 - Congestion Aware Placement using RePlace  </h2></summary><p>		

Placement in OpenLANE occurs in 2 stages  - 
	
1) Global Placement : This is a coarse placement and there is no legalization. Main objective is reducing the wirelength. The parameter used for this is HPWL (Half parameter wire length).
	
2) Detailed placement : The std cells placement is legalized.
	
In openLANE, there are different tools available to run both these steps. 
Legalization implies that std cells should be exactly inside the rows and abutted with each other; and there should be no overlaps. Legalization is more required from a timing point of view. 

In "run_placement", first global placement is run. 

<img src="https://user-images.githubusercontent.com/57150778/219712069-88ab5a5b-50c6-46ac-b5ce-93135cb4bcce.png" width="300">

The objective is to converge the "overflow". If the overflow value decreases, the placement is going correctly.

Final placement stats:

<img src="https://user-images.githubusercontent.com/57150778/219713458-8548aa43-c993-4a81-8d9f-4b86c743e627.png">

Optimization of placement by buffer insertion, cell resizing:

<img src="https://user-images.githubusercontent.com/57150778/219716128-0b4d3492-3df6-41bf-b13d-d879c514d493.png">

Launching magic to view post placement def-

<img src="https://user-images.githubusercontent.com/57150778/219715005-d187cfc8-c9a7-4b60-823d-78aa71977bd7.png">

Magic Layout : 

![image](https://user-images.githubusercontent.com/57150778/219719682-54e624f4-4552-4a3c-a7cf-2bef7090efd3.png)


All the standard cells are placed in std cell rows. There are no DRCs.

<img src="https://user-images.githubusercontent.com/57150778/219723388-5cf08c87-4053-4cbb-9df0-da2ebde86825.png" width=500>
	
PDN : Vpwr, Vgnd vertical stripes added in metal4 and horizontal stripes in metal5. Followpins present in metal1 and vias are stacked from metal1 to metal4.

![image](https://user-images.githubusercontent.com/57150778/219720566-05f54d8e-0830-472a-95d6-52d271a9918b.png)

<img src="https://user-images.githubusercontent.com/57150778/219721621-a705f4a6-3782-4557-85f8-b44182559e15.png" width=400>


Placement Def showing tech via definitions and std cell placement added:

<img src="https://user-images.githubusercontent.com/57150778/219717278-f07496a3-33a0-4977-83e4-c52897f84a37.png" width=600>

Repeaters added for optimizing timing post placement:

<img src="https://user-images.githubusercontent.com/57150778/219717467-58ca1d78-3b51-40b6-9443-cadae5808e73.png" width=600>

PDN added :

![image](https://user-images.githubusercontent.com/57150778/219717755-1f809efc-6e70-45e0-a43b-94243537a813.png)


</p>
</details>

	
</p>
</details>

<details><summary><h1> D2_SK3 Cell Design and Characterization Flows  </h1></summary><p>		

	
<details><summary><h2>:book: L1: Inputs for cell design flow </h2></summary><p>		

Standard cell information is available in a library. A library also has information about decaps, macros, IPs, etc.
The library also has cells with different functionality and sizes. The varying sizes are die to varying drive-strengths. 
The cells may also vary in threshold voltages. This variation in threshold voltage determines the speed of the cell.
	
<img src="https://user-images.githubusercontent.com/57150778/219728804-be7fb654-45b9-4ae6-b274-63d7e3f3fa7e.png" width=500>
	

Cell Design Flow (Inverter Cell):

Inputs : 

1) Process Design Kits (PDKs)
	
2) DRC and LVS rules  :A few examples of DRC checks are shown below. These are needed specifications for the std cell to get fabricated. Actual value is the drawn value.
	
<img src="https://user-images.githubusercontent.com/57150778/219729006-e854dcd3-598d-417e-b275-fe381a32be37.png" width=600>

	
3) SPICE Models: The circled values are obtained from the foundry. These are spice model parameters. The spice model files are also provided by the foundry. 
	
<img src="https://user-images.githubusercontent.com/57150778/219729335-f85124a2-ca75-4cd1-9b0a-d89c8d77e268.png" width=500>
	
</p>
</details>	
	
<details><summary><h2>:book: L2: Circuit Design Step </h2></summary><p>		

Inputs to cell design flow:

4) Library and User defined specs
	
The separation between, the power and ground rail determines the cell height. And it is up to the library developer to see that the cell height is maintained. Cell width depends on the timing information - wider cell have higher drive strengths. Higher drive strength cells can drive longer wires.
Another user defined specification is supply voltage. The top level designer decides the supply voltage for a design and the library designer has to make sure that the std cells operate at this supply voltage. He has to take care of the noise margin levels with respect to this supply voltage.
There can be specifications for metal layers for certain libraries (ex : certain libraries being needed to be built under certain metal layers, contacts to be present on M3,4,5,etc).
Pin Locations might also have user defined specifications like being located near the power and ground rail. Library designer has to make sure of it.
Drawn gate length can also be specified by the user. 
	
<img src="https://user-images.githubusercontent.com/57150778/219730122-339ac35d-c3e6-45bd-a34e-5b47ac3721db.png" width=300>

	
Design Steps:

1) Circuit Design:
	First step is to implement the function itself. The next step is to ensure that the cell meets the library requirement. For example, the (Wp/Lp) / (Wn/Ln) ratio is formulated as : 
		
<img src="https://user-images.githubusercontent.com/57150778/219730283-6f7e1c85-92b7-4c79-95ea-ddff6163002e.png" width=250>
		
We can have designated values of this ratio based on the required values of the switching threshold (Vm~0.98) specified by the designer. Switching threshold is the value at which Vin = Vout.
Or, a library designer can have specifications such as drain current value. These are all circuit design steps, based on spice simulations. 
The output of the circuit design step is the circuit description language : CDL.
	

</p>
</details>

<details><summary><h2>:book: L3: Layout Design Step </h2></summary><p>		

Layout Design Step : 

i) Implement the function by a set of transistor connections.
ii) Derive the pmos network graph and the nmos network graph. 

<img src="https://user-images.githubusercontent.com/57150778/219731284-b8733b8e-722c-4e63-af06-22b4653cb1d1.png" width=400>

	
iii) Obtain the Euler's path : Euler's path is the path that is being traced only once. In this case the Euler's path is A-C-E-F-D-B.
iv) Next we draw a stick diagram out of this Euler's path where the polysilicon inputs are placed in the order of the Euler's path. And then the circuit connections are made.
v) Next the stick diagram is converted to a layout while adhering to DRC rules from the foundry and user defined specifications given by the top level designer. 
	
![image](https://user-images.githubusercontent.com/57150778/219731469-c9d9a924-872f-41d0-8013-6f5789a90a59.png)

	
This hand drawn layout is loaded into a tool like Magic : 
	
<img src="https://user-images.githubusercontent.com/57150778/219731630-8abd526b-15db-47a0-b7dc-0562263631bd.png" width=350>
	
	
With this final layout we have the cell height and width and other user defined specifications like drain current, pin locations, etc. 
vi)  The final step is to extract the parasitics of this layout and characterize it in terms of timing. The output of the layout design will be GDSII, Lef files and extracted spice netlist (.cir), containing resistance and capacitance of each element in the layout.

The next step is characterization of std cells to get timing, noise and power dotlibs. It also contains the functionality of the circuit. 

	
</p>
</details>

<details><summary><h2> :book: L4: Typical Characterization Flow </h2></summary><p>		

For a characterization flow (of, say, a buffer cell), we have the following inputs : 
1) Layout:
	
<img src="https://user-images.githubusercontent.com/57150778/219732755-1b309fcc-6a39-4068-80ec-9fe4afcaf1da.png" width=350>

2) Circuit description : 

![image](https://user-images.githubusercontent.com/57150778/219732979-ecaa0a67-4da7-443c-9c38-51b91c0ad13e.png)
	
3) Spice extracted netlist and subckt definitions. The subckt contains spice models containing characteristics of the nmos or pmos transistors-

![image](https://user-images.githubusercontent.com/57150778/219733048-df75ce89-2697-471d-b702-c0172c2faa0c.png)
	
	
Characterization Setup
Step 1) Read the spice model files.
Step 2) Read in the extracted spice netlist.
Step 3)  Define the behavior of the buffer.
Step 4) Read the subckt files of the inverter
Step 5) Attach the necessary power sources.
Step 6) Apply the stimulus.
Step7 ) Provide necessary output capacitances. For ex, in NLDM models, the output capacitances are varied in a range. 
Step 8) Provide the necessary simulation command. Ex: ."tran 10e-12 4e-09 0e-00", ".dc …."
Step 9) All these inputs are fed as a configuration file called GUNA . The software generates timing, noise and power liberties. 
	
	
</p>
</details>
	
</p>
</details>

<details><summary><h1> D2_SK4 General timing characterization parameters </h1></summary><p>		
<details><summary><h2> :book: L1: Timing threshold definitions </h2></summary><p>		

Timing threshold definitions:
These are variables pertaining to any input waveform that we apply:
Consider the IP and OP waveforms of an inverter as shown
• slew_low_rise_thr :  typically 20% - "low" implies values close to logic 0. The slope of rising waveform is calculated between slew_low_rise_thr and slew_high_rise_thr.

• slew_high_rise_thr: Typically, 20% from the logic 1 level

• slew_low_fall_thr :  20% from logic 0 level of a falling waveform

• Slew_high_fall_thr : 20% value from logic 1 of a falling waveform
 
![image](https://user-images.githubusercontent.com/57150778/219735528-24e75f72-7d52-426e-b3f3-b634436bf90e.png)

	
• in_rise_thr: Assume an input waveform used for transient simulation of  a buffer as shown. Propagation delay is defined between in_rise_thr and out_rise_thr for a rising waveform. It is typically 50%.

• out_rise_thr : 50% value of the output rise waveform.
	
![image](https://user-images.githubusercontent.com/57150778/219735295-9bd9b6b9-fc92-475e-9d76-be6124b0a173.png)

	
• in_fall_thr :  Consider a fall waveform input to buffer and output waveform as shown. This value is typically 50%

• out_fall_thr : Also, 50%. Fall delay = out_fall_thr - in_fall_thr
	
![image](https://user-images.githubusercontent.com/57150778/219735944-253afbe7-c74c-4e7b-9d62-e83126788738.png)

	
</p>
</details>	
	
<details><summary><h2> :book: L2: Propagation delay and transition time </h2></summary><p>		

Consider a buffer cell. Propagation delay, generally can be calculated as:
Td = out_rise_thr - in_rise_thr
Or,
Td = out_fall_thr - in_fall_thr

Consider the following waveform of an inverter:
When the in_rise_thr and out_fall_thr are both at 50%, the delay comes out to be 23ps.

![image](https://user-images.githubusercontent.com/57150778/219736870-455352e8-c718-482f-a35a-0603c8755a16.png)


But when the thresholds are moved to say 70%, the output threshold arrives before the input and thus the propagation delay comes out to be negative (-42ps). Thus correct choice of threshold is very important. 

![image](https://user-images.githubusercontent.com/57150778/219736938-9b0a8e04-13e4-485c-9226-f4a897cc6832.png)


Propagation delay can also be negative when slew of the input waveform is very high due to high wire delays. This could happen if driver and receiver cells are placed far apart. Thus even with the right threshold, poor circuit design can lead to negative propagation delays. 


![image](https://user-images.githubusercontent.com/57150778/219737100-45227d6e-ea38-4019-beb0-af9d203ac834.png)


Timing characterization | Transition time

For a rising w/f : 
Transition time = time(slew_high_rise_thr) - time(slew_low_rise_thr)

And for a falling w/f:
Transition time = time(slew_high_fall_thr) - time(slew_low_fall_thr)

![image](https://user-images.githubusercontent.com/57150778/219737208-8e5395f7-fb4c-43f6-8659-a20773d40faa.png)

	
</p>
</details>
	
</p>
</details>

</p>
</details>


<details><summary> <h1> D3 - Design Library cell using magic Layout and ngspice characterization </summary><p>

<details><summary> <h1> D1_SK1 - Labs for CMOS inverter ngspice simulation </summary><p>

<details><summary><h2> :computer: L0: IO Placer Revision </h2></summary><p>		

OpenLANE allows changing settings at different stages iteratively. For ex, earlier we had equidistance pin placement. We can change that and run floorplan again as shown to get new IO placement:

![image](https://user-images.githubusercontent.com/57150778/221815459-680bf82b-4f45-4f15-8aea-d2490afcaf5b.png)

The resultant pin placement when viewed in magic are as shown - stacked one on top of another (Hungarian Algorithm).
Thus we can reset variables and run steps again in OpenLANE flow.

![image](https://user-images.githubusercontent.com/57150778/221815533-45b657bd-0a5e-400f-bac2-9b11095948c2.png)

The pins are seen to be cramped in lower left corner instead of being placed uniformly across IO ring.

![image](https://user-images.githubusercontent.com/57150778/221815630-2aeb4aff-49b1-42a9-8251-87dcfd130e54.png)


</p>
</details>


<details><summary><h2> :book: L1: Spice deck creation for CMOS inverter </h2></summary><p>		

Create spice deck:

A spice deck has connectivity information (like a netlist). It has inputs to be provided for simulation and points at which output is tapped. Pmos and nmos are denoted with the arrow to substrate instead of the bubble because in spice deck, we need to define connections with the substrate as well. Generally a lot of calculations go into finding the value of the load capacitor. But here we assume a constant value since we are only looking at the static behavior.  The W/L values of pmos and nmos are as defined. Typically, pmos is taken to be (2x or 3x) wider than nmos. 
	
Component Values: The output load value is taken to be 10fF. This is actually computed load capacitance. We take the input gate voltage as 2.5V.  Usually this value is taken to be a multiple of the channel length. (Channel length 1u => Voltage 1V). Drain or main supply voltage is also taken to be 2.5V. 
	
Identify nodes: Nodes are potential points surrounding each element as shown (blue). All the active and passive elements are surrounded by nodes at each terminal. The nodes are names as in, out, vdd and 0.

![image](https://user-images.githubusercontent.com/57150778/221838005-9705e324-3056-4fb9-86ba-fb0cdc114100.png)


</p>
</details>

<details><summary><h2> :computer: L2: Spice simulation lab for CMOS inverter </h2></summary><p>		

The spice deck is as shown. We sweep the input voltage from 0 to 2.5 in steps of 0.05 in a dc simulation. Finally the model file is described, containing technology definition of pmos and nmos.

<img src="https://user-images.githubusercontent.com/57150778/221838645-e2aab61c-9dc3-4801-9c69-23ba0b073b17.png" width=600>

Spice simulation specs : 

<img src="https://user-images.githubusercontent.com/57150778/221838830-8a43b9e9-960b-4b17-8896-2833150612a4.png" width=400>

Model file: contains technological parmeters with respect to nmos, pmos

<img src="https://user-images.githubusercontent.com/57150778/221839237-6bae9c64-8fb1-4560-b0e2-eed90c888b10.png" width=450>

Launch ngspice; go to run area and source the circuit file:

<img src="https://user-images.githubusercontent.com/57150778/222346675-0076b589-5f2f-4877-a10b-8fcc5f0c6ccc.png" width=400>

Execute the circuit using run command. setplot shows which characteristics are run. 

<img src="https://user-images.githubusercontent.com/57150778/222346931-0c6604ac-0438-4029-8810-5a7e4ea0d091.png" width=400>

Run "display" to check the node voltages currently present. Next, run "plot out vs in"

<img src="https://user-images.githubusercontent.com/57150778/222347174-ed37d78a-8627-4751-b207-76aa937c0499.png" width=400>

The characteristics are not centrally aligned and are slightly to the left.

<img src="https://user-images.githubusercontent.com/57150778/222347376-5f6431a7-3b7f-4e05-843c-84859b908c8c.png" width=400>

Consider another case where pmos width = 2.5x nmos width:

<img src="https://user-images.githubusercontent.com/57150778/222347484-4d44e358-9a94-46b1-9b12-409567cb8ff8.png" width=350>

The spice deck for this specification is as shown:

<img src="https://user-images.githubusercontent.com/57150778/222348567-a01a0dd6-6605-4f13-8053-05eee54af0c7.png" width=400>

Upon sourcing this circuit and plotting the output versus input, the following plot is obtained:

<img src="https://user-images.githubusercontent.com/57150778/222349610-cc70e1c2-aa35-45ef-8abf-02756e781f89.png" width=400>

The characteristics are more centrally aligned. 


</p>
</details>

<details><summary><h2> :book: 3: Switching Threshold Vm </h2></summary><p>

Consider two scenarios: In the second one, pmos is bigger in size than nmos.
The shape of the waveform is the same in both characteristics. This indicates that cmos inverter is  very robust device. i.e., when Vin is 0, op is high and vice versa for all cmos devices. Hence cmos logic is widely used for logic gate design.

<b>Static behaviour evaluation : CMOS inverter robustness</b>

<h3> Switching Threshold, Vm</h3>

It is the point at which Vin = Vout. It is where the characteristic meets a 45' line. It is the value of Vin at which output switches value. Case 1- Vm is approximately 1V. Case2: Vm is approximately 1.2V
At this point both pmos and nmos are in saturation region, thus there is high possibility of current flowing directly from power to ground. 

<img src="https://user-images.githubusercontent.com/57150778/222351570-a81548bd-490a-4ee4-83ba-8e9fc0c1afde.png" width=600>

Both pmos and nmos are turned on because Vg > Vth. At switching threshold, VGS = VDS. 

Thus VGS>>VTH

At this point, the same current is flowing through pmos and nmos

Idsp = -Idsn

<img src="https://user-images.githubusercontent.com/57150778/222351729-e4f86557-321a-4da0-b970-86a00786827f.png" width=550>

</p>
</details>

<details><summary><h2> :book: 4: Static and dynamic simulation of cmos inverter </h2></summary><p>

We vary pmos width as integral multiples of nmos width and check the variation of switching threshold value to see robustness of cmos inverter:

![image](https://user-images.githubusercontent.com/57150778/222355763-b5bd313c-efab-4321-bd30-f220bc1a18e4.png)

We take the following spice ckt with equal cmos and nmos widths:

<img src="https://user-images.githubusercontent.com/57150778/222355957-c051a4de-91b2-49bd-bf95-719dc892a999.png" width=400>

We source the ckt file:
> source ckt_file
> run
> setplot
> dc1
> display

<img src="https://user-images.githubusercontent.com/57150778/222356021-30e99aa9-19f7-4391-8517-f7999f1e5384.png" width=400>

"plot out vs in"

<img src="https://user-images.githubusercontent.com/57150778/222356132-ac18589d-5b62-4543-9773-2d3afc303b69.png" width=300>

From this curve, Vm~0.98.

We also do a delay calculation - finding variation of rise and fall delay with varying switching threshold. We change the input to a pulse to do a dynamic simulation, and do a transient analysis. 

![image](https://user-images.githubusercontent.com/57150778/222356427-80a59998-8ccf-48a1-960f-19940067b818.png)

In ngspice we run:
> Source "ckt file"
> "run"
> "setplot"

<img src="https://user-images.githubusercontent.com/57150778/222356670-78600dd8-881c-4c41-ab30-9efc35aca905.png" width=450>

> "tran2"

 <img src="https://user-images.githubusercontent.com/57150778/222356804-d2cf7eec-0592-427e-aeba-5ede71e06976.png" width=450>

> "plot out vs time in"
	
<img src="https://user-images.githubusercontent.com/57150778/222356901-d0e3ec42-ac57-42f0-b9c0-801b89cf42c0.png" width=350>

We calculate rise delay by difference in 50% values of ip rise and op rise waveforms"

<img src="https://user-images.githubusercontent.com/57150778/222357670-00e28798-da53-4dc4-bfc6-6582c912b67e.png" width=400>

<img src="https://user-images.githubusercontent.com/57150778/222357101-80c0d133-bbb2-40be-b1dc-0cdbeb0546b7.png" width=300>


Similarly for op_fall delay…. 


</p>
</details>

<details><summary><h2> :computer: L5- Labs to git clone vsdstdcelldesign </h2></summary><p>

![image](https://user-images.githubusercontent.com/57150778/222360087-483cb309-32cd-489d-ab1f-d937350f2985.png)

![image](https://user-images.githubusercontent.com/57150778/222360114-e68f55e7-00db-4a20-83dd-335bdcc520a9.png)

![image](https://user-images.githubusercontent.com/57150778/222360139-3d1cd73f-e6b3-4921-b8a3-afcaa7e134ea.png)

The inverter cell is viewed in magic layout.

</p>
</details>

</p>
</details>

<details><summary><h1> D3_SK2- Inception of Layout and CMOS fabrication Process </h1></summary><p>

<details><summary><h2> :book: L1- Create Active Regions </h2></summary><p>

<h3> 16 Mask Process </h3>

1) Selecting a substrate

The complete design is fabricated on the substrate. There are many kinds of substrates available. But we go or the one used most commonly in mobile devices, chips, etc - p-type silicon substrate.
Some of its properties include high resistivity, doping level of 10^15 per cm cube and orientation of 100.
The doping level is to be maintained at a level below the well doping. 
		
<img src="https://user-images.githubusercontent.com/57150778/222362314-18263edd-596e-4208-9dfa-7a39e71e284f.png" width=550>
	
2) Create active region for transistors

We create pockets for pmos and nmos devices. First step is to create an isolation between all the pockets so that the transistors don't interfere with functioning of each other.  First a SiO2 layer is grown which acts as an insulator. Next we deposit an 80nm layer of (Silicon Nitride) Si3N4. 
Next to create the pockets, we deposit a 1um layer of photoresist. 
Next we make Mask1 (masks are nothing but layout geometries in fabrication terms). Masks are used to protect certain areas of the photoresist while the other areas remain exposed to UV light. Thus we chemically wash out in developing solution the photoresist from certain regions.

<img src="https://user-images.githubusercontent.com/57150778/222362487-b3dbf8f1-5963-45cc-8b00-46cfc0d10987.png" width=450>

The resultant is:

<img src="https://user-images.githubusercontent.com/57150778/222362575-84d20b9e-1f1d-4506-baab-a6ed00d802cc.png" width=400>
		
Next we remove the mask so that now if we do some deposition or some etching, the areas under the photoresist stay protected. 

<img src="https://user-images.githubusercontent.com/57150778/222362669-9f5c2991-85b8-4f2e-8550-ada0060d3a75.png" width=400>

Next we etch off the silicon Nitride:
		
<img src="https://user-images.githubusercontent.com/57150778/222362756-9086efaf-6179-4fdb-acb5-1ad11cfce3e7.png" width=400>
		
Finally we can remove the photoresist because the Si offers enough protection to SiO2 areas to grow the oxides on the other areas. When we put this into an oxidation furnace, the unprotected areas of SiO2 will grow. This creates isolation areas (divided by grown SiO2) and transistors can be isolated on either sides of it.
		
<img src="https://user-images.githubusercontent.com/57150778/222362863-025d2cbd-1732-403d-ade5-8ca212f93fe2.png" width=400>
	

This process is called "LOCOS" (Local Oxidation of Silicon)

Next, the Si3N4 is stripped out in hot phosphoric acid- 
		
<img src="https://user-images.githubusercontent.com/57150778/222362991-4fa51c77-4de2-4734-b768-680feec8e441.png" width=400>


</p>
</details>

<details><summary><h2> :book: L2- Formation of nwell and pwell </h2></summary><p>

![image](https://user-images.githubusercontent.com/57150778/222365712-be957318-4205-48d1-afde-c7eb5468de4a.png)

</p>
</details>

<details><summary><h2> :book: L3- Formation of gate terminal </h2></summary><p>

![image](https://user-images.githubusercontent.com/57150778/222366272-b6357446-4187-4658-a2a8-e903c48d7be3.png)

</p>
</details>

<details><summary><h2> :book: L4- Lightly Doped Drain (LDD) formation  </h2></summary><p>

![image](https://user-images.githubusercontent.com/57150778/222367058-c8e37eb2-49f8-40c5-b15b-24713366ec64.png)

</p>
</details>

<details><summary><h2> :book: L5- Source and Drain formation </h2></summary><p>

![image](https://user-images.githubusercontent.com/57150778/222367432-83df9376-8275-4d6c-adf7-5839221c9742.png)

</p>
</details>


<details><summary><h2> :book: L6- Local interconnection formation </h2></summary><p>

![image](https://user-images.githubusercontent.com/57150778/222367793-8c5f6451-18d6-44a2-b1e9-c4def08c41c6.png)

</p>
</details>

<details><summary><h2> :book: L7- Higher Level Metal Formation </h2></summary><p>

![image](https://user-images.githubusercontent.com/57150778/222368318-bc8f3036-5c4f-4c00-a322-4a60fe446ba2.png)

</p>
</details>

</p>
</details>

</p>
</details>

