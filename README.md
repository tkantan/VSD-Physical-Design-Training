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


</p>
</details>

</p>
</details>

</p>
</details>
