# Digital-VLSI-SoC-design-and-planning
## DAY 01: Inception of open-source EDA, OpenLANE and sky130 PDK
Day 1 introduces the fundamental concepts of digital VLSI design by explaining how software interacts with hardware and how a chip is built from abstraction to fabrication using the RTL-to-GDSII flow. It begins with understanding a real system like an Arduino board, where the chip (microprocessor) communicates with external components through pads, while all logic is implemented inside the core and the entire physical chip area is called the die. The concept of SoC is introduced using a RISC-V based system containing IP blocks such as SRAM, ADC, DAC, and SPI, emphasizing that these are reusable foundry-provided components. RISC-V is presented as an open-source Instruction Set Architecture (ISA) that allows flexible and cost-free hardware design. The flow of execution from software to hardware is also explained, where application programs pass through system software layers—operating system, compiler, and assembler—to generate binary instructions that act as an interface between high-level languages and hardware execution.

The session then shifts to SoC design using open-source tools, highlighting that three essential components are required for digital ASIC design: RTL design (behavioral hardware description), EDA tools (for design and verification), and PDK data (which links design with fabrication through rules, libraries, and models). The simplified RTL-to-GDSII flow is explained in detail, including synthesis (RTL to gate-level netlist), floorplanning and power planning (defining chip area and power network), placement (arranging cells efficiently), clock tree synthesis (distributing clock signals with minimal skew), routing (creating physical interconnections), and sign-off (verification using DRC and static timing analysis). OpenLANE is introduced as a fully automated open-source flow integrating tools like Yosys, OpenROAD, Magic, and Netgen to generate a clean GDSII with no DRC, LVS, or timing violations, supporting both autonomous and interactive modes. It is optimized for the Sky130 PDK, an open 130nm process released by Google and SkyWater, which enables real chip fabrication without proprietary restrictions.

Finally, Day 1 provides practical exposure to OpenLANE by explaining its directory structure, basic Linux commands, and design preparation steps. A sample design (picorv32a) is used to demonstrate how configuration files define parameters like clock period, and how the design is prepared using the prep command before running synthesis. After synthesis, reports such as flip-flop count and total cell count are analyzed to evaluate design metrics like flop ratio. The workflow also shows how intermediate files like merged LEF and run directories are generated, helping in understanding how theoretical ASIC design concepts are implemented in a real open-source environment. Overall, Day 1 builds a strong foundation in both conceptual understanding and practical flow of open-source VLSI design

![day1](Images/day1(1).png)
![day1](Images/day1(2).png)
![day1](Images/day1(3).png)
![day1](Images/day1(4).png)
![day1](Images/day1(5).png)

```math
Flop\ Ratio = \frac{Number\ of\ D\ Flip\ Flops}{Total\ Number\ of\ Cells}
```
```math
Flop\ Ratio = \frac{1613}{14876} = 0.108429685
```

<br>
<br>

```math
Percentage\ of\ DFF's = Flop\ Ratio * 100
```
```math
Percentage\ of\ DFF's = 0.108429685 ∗ 100 = 10.8429685%
```
## DAY 02: Good floorplan vs bad floorplan and introduction to library cells
Day 2 focuses on the physical design stage of VLSI, particularly floorplanning, placement, and basic cell design concepts. It begins with understanding chip floorplanning, where the dimensions of the core and die are determined based on the netlist. The concept of utilization factor (ratio of area occupied by cells to total core area) and aspect ratio (height-to-width ratio of the chip) is introduced, explaining how these parameters affect chip shape and efficiency. A utilization factor of 1 indicates full usage of core area, while lower values leave space for routing and additional cells. The importance of pre-placed cells (IP blocks like memory, clock modules, etc.) is discussed, which are fixed before placement and cannot be moved during later stages. To ensure stable operation, decoupling capacitors are placed near these blocks to supply instantaneous current and reduce voltage drops during switching.

The session then explains power planning, highlighting issues like voltage drop and ground bounce that occur when many circuits switch simultaneously. To overcome this, a power mesh structure is used, allowing multiple paths for current flow and ensuring stable voltage across the chip. Pin placement is another key aspect, where input/output pins are arranged around the core, and clock pins are made larger to reduce resistance and delay. Logical cell placement blockage is also applied near pins to avoid routing congestion. The practical implementation of floorplanning using OpenLANE is demonstrated, including configuration parameters like core utilization and aspect ratio, and how to view layouts using DEF files and the Magic tool.

Further, the concept of library building and placement is introduced, where logical gates are mapped to physical standard cells with defined dimensions and timing characteristics. Placement is performed in two stages: global placement (rough arrangement) and detailed placement (legalized and optimized placement). Optimization techniques such as inserting buffers (repeaters) are used to maintain signal integrity over long distances. The discussion also covers congestion-aware placement using tools like RePlAce, ensuring efficient routing and minimal wire length.

Finally, Day 2 introduces cell design and characterization flow, where standard cells are designed using inputs like PDK data, design rules, and SPICE models. The design process includes circuit design, layout generation, and characterization to extract timing, power, and noise parameters. Concepts like Euler paths, stick diagrams, and layout generation are explained for transistor-level design. The characterization process involves simulation steps to generate timing models, followed by understanding timing parameters such as propagation delay and transition time using defined threshold levels. Overall, Day 2 builds a strong understanding of how logical designs are converted into optimized physical layouts while ensuring performance, power efficiency, and reliability.

![day2](Images/day2(1).png)
![day2](Images/day2(2).png)
![day2](Images/day2(3).png)

```math
1000\ Unit\ Distance = 1\ Micron
```
```math
Die\ width\ in\ unit\ distance = 660685 - 0 = 660685
```
```math
Die\ height\ in\ unit\ distance = 671405 - 0 = 671405
```
```math
Distance\ in\ microns = \frac{Value\ in\ Unit\ Distance}{1000}
```
```math
Die\ width\ in\ microns = \frac{660685}{1000} = 660.685\ Microns
```
```math
Die\ height\ in\ microns = \frac{671405}{1000} = 671.405\ Microns
```
```math
Area\ of\ die\ in\ microns = 660.685 * 671.405 = 443587.212425\ Square\ Microns
```

![day2](Images/day2(4).png)
![day2](Images/day2(5).png)
![day2](Images/day2(6).png)
![day2](Images/day2(7).png)
![day2](Images/day2(8).png)
![day2](Images/day2(9).png)
![day2](Images/day2(11).png)
![day2](Images/day2(13).png)
![day2](Images/day2(15).png)

## DAY 03: Design library cell using Magic Layout and ngspice characterization
Day 3 focuses on transistor-level design, simulation, layout creation, and characterization of a CMOS inverter using Magic and ngspice tools. It begins with revisiting IO placement, where pin configurations can be modified using OpenLANE parameters like `FP_IO_MODE`, allowing control over pin distribution across the chip. The core of this day is the design and simulation of a CMOS inverter using a SPICE deck. A SPICE deck is created by defining component connectivity (PMOS and NMOS connections including drain, gate, source, and substrate), assigning component values (transistor sizes), identifying and naming nodes (Vin, Vout, Vdd, Vss), and including additional elements like load capacitance, input voltage source, and supply voltage. Simulation commands such as DC sweep are used to obtain the Voltage Transfer Characteristics (VTC), where Vin is varied and Vout is observed. By modifying transistor sizing (e.g., making PMOS width three times NMOS), the switching threshold (Vm) shifts, demonstrating how transistor sizing affects inverter behavior. The switching threshold is a critical point where Vin equals Vout, and both NMOS and PMOS conduct simultaneously, potentially causing leakage. Both static (DC) and dynamic (transient) simulations are performed to analyze rise time, fall time, and delay characteristics.

The workflow then moves to practical layout design using Magic. The vsdstdcelldesign repository is cloned, and the CMOS inverter layout is viewed using the Sky130 technology file. Different layers such as polysilicon, diffusion, metal layers, and wells are identified visually. The layout is verified, and parasitic extraction is performed using commands like `extract all` and `ext2spice` to generate a SPICE netlist from the layout. This extracted netlist is then used in ngspice along with Sky130 model libraries to perform accurate simulations. A complete SPICE deck is created including `.include` statements for NMOS and PMOS models, power supply definitions, pulse inputs, and transient analysis commands. Simulation results are used to calculate key timing parameters such as rise time (20%–80%), fall time (80%–20%), propagation delay (difference between 50% input and output), and cell fall delay. These parameters are essential for characterizing the standard cell before integrating it into larger designs.

Further, the day explains the CMOS fabrication process in detail, starting from substrate selection (p-type silicon) and active region formation using oxidation and masking techniques. N-well and P-well formation is done through ion implantation and annealing. Gate formation involves oxide growth and polysilicon deposition, followed by lightly doped drain (LDD) formation to reduce short-channel and hot-electron effects. Source and drain regions are created using heavy doping, followed by annealing. Local interconnects are formed using titanium and titanium nitride layers, while higher metal layers are added using processes like chemical mechanical polishing (CMP), tungsten plug formation, and aluminum deposition. This step-by-step fabrication process connects layout design to real silicon manufacturing.

Finally, the day includes detailed labs on Sky130 technology files and design rule checking (DRC). Magic tool is used to load layouts, inspect layers, and verify rules using commands like `drc check` and `drc why`. Users learn to identify and fix DRC errors (e.g., poly.9 rule violations) by modifying the Sky130 technology file. Additional exercises involve correcting spacing rules, understanding geometric rule definitions, and verifying layouts against design rules. The day concludes with advanced debugging of layout issues and ensuring that the design adheres to fabrication constraints. Overall, Day 3 provides a complete understanding of how a standard cell is designed, simulated, laid out, verified, and prepared for integration into a full chip design flow. 

![day3](Images/day3(1).png)
![day3](Images/day3(2).png)
![day3](Images/day3(3).png)
![day3](Images/day3(4).png)
![day3](Images/day3(5).png)
![day3](Images/day3(6).png)
![day3](Images/day3(7).png)
![day3](Images/day3(8).png)
![day3](Images/day3(9).png)

```math
20\%\ of\ output = 660\ mV
```
```math
80\%\ of\ output = 2.64\ V
```
```math
Rise\ transition\ time = Time\ taken\ for\ output\ to\ rise\ to\ 80\% - Time\ taken\ for\ output\ to\ rise\ to\ 20\%
```
```math
Fall\ transition\ time = Time\ taken\ for\ output\ to\ fall\ to\ 20\% - Time\ taken\ for\ output\ to\ fall\ to\ 80\%
```

<br>

```math
Rise\ Cell\ Delay = Time\ taken\ for\ output\ to\ rise\ to\ 50\% - Time\ taken\ for\ input\ to\ fall\ to\ 50\%
```
```math
Fall\ Cell\ Delay = Time\ taken\ for\ output\ to\ fall\ to\ 50\% - Time\ taken\ for\ input\ to\ rise\ to\ 50\%
```
```math
50\%\ of\ 3.3\ V = 1.65\ V
```

![day3](Images/day3(11).png)

```math
Rise\ transition\ time = 2.246777ns - 2.18242ns
```
```math
Rise\ transition\ time = 64.357ps
```

![day3](Images/day3(12).png)

```math
Fall\ transition\ time = 4.095486ns - 4.053121ns
```
```math
Fall\ transition\ time = 42.365ps
```

![day3](Images/day3(13).png)

```math
Rise\ Cell\ Delay = 2.211528ns - 2.150000ns
```
```math
Rise\ Cell\ Delay = 61.528ps
```

![day3](Images/day3(14).png)

```math
Fall\ Cell\ Delay = 4.078042ns - 4.050000ns
```
```math
Fall\ Cell\ Delay = 28.042ps
```

![day3](Images/day3(15).png)
![day3](Images/day3(16).png)
