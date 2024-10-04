# RTL to GDSII flow 
   
# Introduction to RTL to GDSII

The RTL to GDSII flow is a critical process in the design and fabrication of digital integrated circuits (ICs). It represents the transformation of a high-level Register Transfer Level (RTL) description of a circuit, typically written in hardware description languages like Verilog or VHDL, into the final GDSII format. This GDSII format is used to create photomasks for semiconductor fabrication.

![Screenshot (58)](https://github.com/user-attachments/assets/0898c72b-074f-4586-99cf-402df254dbf5)

</details><details>
  <summary>RTL</summary>
   
## 1. RTL(Register Transfer Level) :
* Code involves describing the digital logic of a circuit in terms of data flows and the operations performed on data using an HDL(hardware description language) ex: Verilog, Systemverilog, VHDL.
* Example: A simple RTL Verilog Code of a Inverter shown Below:

   ![Screenshot (59)](https://github.com/user-attachments/assets/ed54d7ce-20dc-4ae7-91f5-a0de1aa03513)

</details><details>
  <summary>Functional Verification</summary>
   
## 2. Functional Verification :
* The primary goal of functional verification is to confirm that the design, described using an HDL like Verilog or VHDL, correctly implements the specified functionality.
* This involves checking that the design meets all the requirements and specifications without any logical errors using tools such as Modelsim ,QuestaSim, Xcelium,etc.
* For Functional Verification we require the Design files and the Testbench of the Design written in HDL like Verilog or VHDL Through which we can generate a Waveform to
verify the functionality of the Design is correctly implemented as written in the Code.

#### Output:

![Screenshot (64)](https://github.com/user-attachments/assets/270acce3-4864-4092-aa52-fe97f675fb12)

</details><details>
  <summary>Synthesis</summary>
   
## 3. Synthesis :
* Synthesis is a physical design Process where a high-level Hardware Description Language (HDL) description (such as Verilog or VHDL) is transformed into a lower-level representation that can be implemented on hardware.
* This process is essential for converting abstract designs into concrete implementations on FPGAs (Field-Programmable Gate Arrays) or ASICs (Application-Specific Integrated Circuits).
#### Goals of Doing Synthesis: 
To get a gate level Netlist, Logic optimization, Logic Equivalence between RTL and Netlist, Inserting clock gates if needed, Meet Timing .

#### Logic Synthesis:
The process by which RTL is converted to an equivalent circuit as interconnection of logic gates.

#### RTL to Netlist Flow:

![Screenshot (60)](https://github.com/user-attachments/assets/e5110d07-bd43-4001-b145-37132b581305)

A netlist is a detailed description of the components in an electronic circuit and the connections between them.

### Constraints:
Constraints are specific requirements of a design that needs to be honored or
attempted to be honored by the CAD tools. They play a critical role in guiding the
design flow, from initial synthesis and optimization stages to final physical
implementation and verification, ensuring that the designed circuit meets its intended
functionality and performance goals. 

Something that the EDA tool cannot determine on
its own such as -
1) Time at which input arrives at the Die/Chip boundary.
2) Load that has to be driven by the output.
3) Transition time for the inputs.
4) Clock frequency, false paths, exceptions, etc

### Clock Skew:
The instantaneous difference between the arrival of a clock signal at any two Flipflops
is called ‘skew’. Skew should be minimum or zero ideally. Skew is of two types
Positive Skew and Negative Skew.
* Positive Skew : It captures clock coming later than launch clock.
* Negative skew : It captures clock coming earlier than launch clock.
* Local Skew : The difference in arrival of clock at pins of two consecutive sequential
elements.
* Global skew : The difference between shortest clock path delay and longest clock path
delay reaching two sequential elements for a clock domain.
* Zero Skew : When all flops get the clock edge with the same delay (ideal) relative to
each other.

### Clock Sources :

Two types of clock sources-
* Primary clock sources : waveform independent of other clock sources in that design.
* Derived clock sources : waveform depends on other clock sources.

Master clock : clock from which we derive another clock is known as the master clock
of the derived clock . CS1 is the master clock source of CS2.

### Clock Latency :
The Total time a clock signal takes to propagate from clock source to a specific register
clock pin.Clock latency comprises two components - clock source latency(off-chip) and
clock network latency(on-chip).

Clock latency = Source latency + Network latency (insertion delay)

### Virtual Clocks :
A virtual clock is a clock that exists but is not associated with any pin or port of the
design. It is used as a reference in STA analysis to specify input and output delays
relative to a clock.

### Clock Transition:
Clock transition, also known as clock edge rate or clock slew, refers to the time it takes
for a clock signal to transition from one logic level to another. Specifically, it measures
the time required for the clock signal to rise from a low logic level to a high logic level
(rise time) or to fall from a high logic level to a low logic level (fall time).

### Clock Jitters :
Clock jitter refers to the variations or deviations in the timing of a clock signal's edges
from their ideal positions. It is the variation of the clock period from edge to edge. It
can vary +/- jitter
value. Clock jitter can be caused by a variety of factors, including power supply noise,
temperature variations, electromagnetic interference, and imperfections in the clock
generation circuitry.

### Clock Uncertainty :
The time difference between the arrivals of clock signals at registers in one clock
domain or between domains. It allows us to specify the expected clock setup or hold
uncertainty associated with jitter, skew, and a guard band(margin).

### Load Specifications :
"Load specifications" are used to define the capacitive load that a particular signal or
port must drive. This is important for timing analysis, power analysis, and ensuring the
design meets its performance targets.

### False Paths:
By setting up false paths, you help the synthesis and timing analysis tools to ignore
non-critical paths that do not impact the functional timing of the circuit, thereby
streamlining the design process and focusing optimization efforts on the truly critical
paths.

### Group Paths:
By grouping paths, you can apply specific constraints, analyze them more effectively,
and simplify the overall timing analysis process. There are certain application to specific
path groups -
* Input to Register (I2R): groups all paths that start from any input port and end
at any register. These paths are critical for analyzing the timing of data entering
the design and being captured by registers.
* Register to Output (R2O): groups all paths that start from any register and end
at any output port. These paths are essential for timing analysis of data being
output from the design after being processed.
* Register to Register (R2R): groups all paths that start from any register and
end at any other register. These paths are critical for internal timing analysis,
ensuring that data moves correctly between registers within the design.
* Input to Output (I2O): groups all paths that start from any input port and end
at any output port.
These paths might represent combinational paths that do not involve registers.

### I/O Constraints:
Set_input_delay and set_output_delay commands are used to specify the delay
at input port and delay from the output port to the next off-chip flop with
respect to launch clock.

#### Output netlist:

![Screenshot (65)](https://github.com/user-attachments/assets/28755a60-465d-417d-9255-ab7ea88b263f)

 </details><details>
  <summary>Floor planning</summary>
    
## 4. Floor planning 

Floor planning is the initial stage of chip design, determining the chip's width, height,
and overall area. It involves defining the core and die areas, placing macros, and
establishing blockages and halos.

### Objectives of Floor Plan
* Minimize the area.
* Minimize the timing.
* Reduce the wire length.
* Making routing easy.

### Key Aspects of Floor Planning
#### 1. Defining Core Area
  * Determine the shape and size of the block/partition.
  *  Create voltage areas for low-power or multi-voltage designs.
  *   Establish IO pad/pin areas and placement.
#### 2. Macro Placement
  * acros are generally placed near the core boundary to minimize routing and
power complexity and improve signal integrity.
  *  aintain proper spacing between macros for routing channels.
#### 3. Blockages and Halos
  * Placement Blockages:
      * Prevent placement tools from placing cells in specific regions.
      * Hard Blockage: No cells allowed.
      * Soft Blockage: Only buffers are permitted.
      * Partial Blockage: Percentage-based cell allowance.
  * Routing Blockages:
    * Prevent routing in specific regions.
    * Signal Blockage: Block data and clock signals, but allow power nets.
  * Halo (Keep-Out Region):
    * Surround macros to prevent other cells from being placed nearby.
    * Halos can overlap and move with macros.
#### 4. Adding Pre-Placed Cells
  * Place critical cells (like PLL, voltage regulators, and clock circuits) before actual
placement and routing.
  * Surround critical cells with decoupling capacitors to improve reliability and
efficiency

#### Output:

![Screenshot (63)](https://github.com/user-attachments/assets/0208cb2a-7a61-491c-94d8-61996730e19a)


</details><details>
  <summary>Power planning</summary>
   
## 5. Power planning

Power planning is a critical stage in the physical design flow of integrated circuits (ICs),
creating an efficient and reliable power distribution throughout the chip

### Objectives
* Reliable power delivery
* Thermal management

#### Power pad placement
Power pads are strategically placed on the chip to facilitate external power
connections. These pads must be located to minimize the distance between power
sources and areas of high power consumption. Proper placement is crucial to reduce
the resistance and inductance in the power delivery path, thereby ensuring a stable
power supply.

#### Power Ring Creation
Power rings are implemented around critical areas, such as the core logic blocks, to
provide a stable and reliable power supply. These rings typically include VDD (power)
and VSS (ground) connections and help in distributing power efficiently within the
core regions. Power rings also play a role in minimizing voltage drops and enhancing
the overall robustness of the power network.

#### Power Rail Routing
Within the standard cells and macroblocks, power rails are routed to ensure that power
is delivered to all individual components. This routing needs to be optimized to reduce
resistance and inductance, ensuring minimal power loss and efficient power
distribution across the chip. reduce and give points

#### Rings
Circulate power around the chip, ensuring uniform distribution to different sections.

#### Stripes
Transports power from the rings across the chip, maintaining consistent voltage levels
and connectivity between different power domains.

#### Rails
Provide direct connections between the chip's VDD and VSS and the standard cells
VDD and VSS, facilitating stable power delivery and signal integrity.

#### Trunks
Links the pads directly to the power rings, enabling efficient power transfer from
external sources to internal power distribution networks within the chip.

#### Output:

![Screenshot (61)](https://github.com/user-attachments/assets/5a116da5-6cac-472e-81bb-4f28b46256e9)

![Screenshot (62)](https://github.com/user-attachments/assets/55b6198e-96bf-46aa-9513-ad8ed7ea4b9f)

</details><details>
  <summary>Placement</summary>

## 6. Placement
Placement is the process of placing all the standard cells from the Netlist into the core
area.

### Goals of placement
* Timing, area and power optimization
* Minimize congestion and congestion hotspots
* Minimum cell density, pin density
* No timing DRV’s (Design Rule Violations)

### Placement Procedure Overview
#### 1. Pre-Placement Checks
   * Netlist should be clean.
   * Floorplan DEF should be good
      * Proper Pin placement
      * Macros and pre-placed cells should be in fix.
      * Power routes should be free of DRCs
#### 2. Place various Physical Only Cells like end-cap cells, well-tap cells, IO buffers, antenna diodes, and spare cells.
#### 3. Global Placement
All the cells are placed arbitrarily in the ASIC core, but they are not legally placed
(fixed location not assigned) within standard cell row.
#### 4. Legalization
Ensures that final placement is legal and there is no Placement constraint violation.
#### 5. High Fanout Net Synthesis
Some of the nets will have very high number of fanouts like Reset, Scan Enable
etc... But, there is a restriction for maximum fanout in timing constraints.
#### 6. Scan Chain Reordering
Creating a fresh scan chain routes by connecting the flops which are near to each
other. There is no requirement to check whether the flops are functionally talking
or not talking for scan chains.
#### 7. Timing/Power Optimization

#### Congestion 
* When the number of routing tracks available for routing in a given location is less than
the number necessary, the area is considered congested.
* Congestion = Available routing tracks – Required routing tracks

#### Slack
Slack is the difference between a path's required time and arrival time. For timing path
slack determines if the design is working at the specified speed or frequency.
Zero slack means that the design is critically working at the desired frequency.
Slack has to be positive always and negative slack indicates a violation in timing.

#### Skew
Skew is the difference of arrival times of clock edge at the clock pins of adjacent flops.
Skew affects the both setup and hold times.

#### Slew
Slew is transition time of the signal to change its state from logic 1 to logic 0. It is
associated with the rise time and fall time of signal.

#### output

![place](https://github.com/user-attachments/assets/bd5e07a4-53f2-4663-abe8-af177cd3a2e7)

![Screenshot (66)](https://github.com/user-attachments/assets/7bfcf956-6503-45c7-b06a-f15090bf8c47)

#### DRV Violations are observed:

![Screenshot (67)](https://github.com/user-attachments/assets/7359653c-e922-46be-8e24-97115929b280)

#### Design Optimization to remove DRV Violations
* ECO > Optimize Design
* In the Optimization Window, select the following
* Choose Design Stage - Pre CTS
* Optimization type - Setup
* Select all three Design Rule Violation.

![Screenshot (68)](https://github.com/user-attachments/assets/0685a727-7030-4f3f-a461-42b27a7c45c7)

#### Analyzing Congestion
* Route > NanoRoute > Analyze Congestion

 #### When Congestion Effort is Low

   ![Screenshot (71)](https://github.com/user-attachments/assets/7373d668-f430-4cf1-ae3c-a96b07aa0bb3)
   
   Routing Overflow: 0.03% H and 0.00% V

  #### When Congestion Effort is Medium

   ![Screenshot (72)](https://github.com/user-attachments/assets/1ae84819-55f5-4575-b7bb-db235b169590)
   
   Routing Overflow: 0.02% H and 0.00% V

  #### When Congestion Effort is high
  Routing Overflow: 0.00% H and 0.00% V
   No Overflow Observed.

</details><details>
  <summary>Clock Tree Synthesis</summary>
   
## 7. Clock Tree Synthesis
CTS is the process of connecting the clock from clock port to the clock pin of
sequential cells in the design by maintaining minimum insertion delay and balancing
the skew between the cells using clock inverters and clock buffers.

### CTS Goals
* Minimum Skew
*  Minimum Insertion delay
*  Complete the clock tree with no DRV (Tran, cap and fanout) violations.
*  No timing violations (Setup and Hold)

### CTS Procedure Overview

#### Pre CTS Checks
* Placement – Completed
* Power ground nets – Pre Routed
* Estimated Congestion – acceptable
* Estimated Timing – acceptable (~ 0ns slack)
* imated Max Tran/Cap – No violations
* High Fanout Nets
* Logical / physical library should have special clock cells (clkBuf or clkInv)

#### Clustering:
Depending on the geometry locations, the skew groups are being created as per
the description in SPEC file.
#### DRV Fixing:
At this stage, DRVs (max_tran, max_cap, max_length, max_fanout) are fixed.
#### Insertion Delay Reduction:
At this stage, insertion delay is minimized as much as possible, which is one of our
main goals for the Clock Tree Synthesis.
#### Balancing:
The main balancing happens at this stage with the help of different clock buffers
and inverters.
#### Routing of clock tree:
During this step, tool will route all the clock tree nets using a Nanoroute engine.
#### Post Conditioning:
At this stage, again DRVs will be checked and if required then it will be fixed.

### Clock
A signal with constant rise and fall with ideally equal width (50% rise and 50% fall of the
signal width) helps to control data propagation through the clock elements like Flip
Flop, Latches etc. The clock source mostly present in the top-level design and from
there propagation happens. PLL, Oscillator like constant sources are being used
normally in designs to get the clock.

### Types of Clock Tree Structures
Different structures are available to build clock tree to maintain minimum insertion
delay and balance the skew.
A few clock tree structures are:
* H – Tree structure
* X – Tree structure
* Geometric Matching Algorithm (GMA)
* Pi Tree structure
* Fishbone

#### Output:

run in script mode
      
      source cts.tcl

   ![Screenshot (73)](https://github.com/user-attachments/assets/3762f38f-4521-455f-a5f5-1f7bfea7e5b9)

No Setup Violations, because both WNS and TNS are Positive.

   ![Screenshot (74)](https://github.com/user-attachments/assets/d97db92b-d253-4d46-a0c3-e6e337c809a0)

If there are DRV Violations, follow ECO method as we did in placement

</details><details>
  <summary>ROUTING</summary>
   
## 8. ROUTING
Routing is the process of creating physical connections between or among the signal
pins by following the DRC rules and also after routing timing (setup and hold) have to
meet.

### Types of Routing:
* Pre routing – also known as power routing which comes under power planning.
* Clock routing – it can be done while building the clock tree in CTS stage.
* Signal routing – it is the stage after CTS.

### Goals of Routing:
* Minimize the total interconnect or wire length and vias.
* Complete the routing within the area of the design.
* No DRC violations.
* Meeting the timing.
* No LVS errors

### Tasks performed by signal routing:
* Global routing.
* Track assignment.
* Detailed routing.
* Search and repair.

#### Output

![Screenshot (75)](https://github.com/user-attachments/assets/2ae8e009-077f-49bb-b43b-fe060000fcf5)

#### Post Routing timing optimization
Timing >> Report Timing

setup violations:

![Screenshot (76)](https://github.com/user-attachments/assets/cdcfd728-60d5-4e6d-bedc-313bfe1f7e60)

hold violations:

![Screenshot (77)](https://github.com/user-attachments/assets/a454b21c-ee76-4a09-bde7-f258c1c651a9)

#### Filler cells in empty core area

In innovus window click Place >> Physical cell >> Add Filler

![Screenshot (78)](https://github.com/user-attachments/assets/90778ca8-7804-4885-b3f7-c6d4b3b519b9)

Add all the cells to the Selectable Cell List

</details><details>
  <summary>SIGNOFF</summary>

## 9. SIGNOFF
Signoff is the process of verifying the design at the final stage before going to the tape
out. Clean signoff reports are the green signal to the fabrication because clean reports
ensure the design satisfies all the required specifications and constraints at the final
stage.

### The major checks of signoff include:
### STA (Static Timing Analysis):
STA's main agenda is to make sure that signals propagate through the design
within specified time constraints. During STA the tool divides the entire design
circuit into 4 sets of timing paths, which are in2reg, reg2reg, reg2out, and in2out.
Then analyses the delay of signal paths in the circuit. By considering these delays,
the analysis determines the worst-case and best-case timings for various paths.
This information is important to make sure that the design functions properly
and meets all performance requirements (like clock frequency and setup/ hold
timings).
Tool: Tempus.

### IR Drop Analysis:
IR drop is also known as voltage drop. Due to the internal resistance of metal of
the power delivery network there could be a drop in voltage. This is becoming
more critical as design complexity increases. An IR drop can lead to variations in
supply voltage levels in the design, which causes performance degradation,
functionality errors and even complete failures. Excessive IR drop can result in
slower circuit operation, reduced noise margins and timing violations.
Tool: Voltus.

### EM Analysis:
EM means electromigration. It refers to the phenomenon where the movement
of metal atoms within the conductor is induced by the flow of high current
densities. This causes the metal atoms to break or short with nearby metal. This
will create issues like increased resistance, altered signal propagation, and
eventually, circuit malfunction or complete failure. So, designers take care of this
issue and they follow certain strategies to fix this effect.
Tool: Voltus.

### Physical Verification:
Physical verification ensures the correctness of the layout before manufacturing.
During this stage, tool analyses the design layout data against the set of
predefined design rules, which can cover various aspects such as minimum
feature sizes, metal spacing, metal density, LVS, and more. By performing this
stage, designers can identify the critical issues early in the design, reducing the
risk of costly manufacturing errors and post fabrication failures.
Tool: Calibre.

#### Output:

![Screenshot (79)](https://github.com/user-attachments/assets/beec253e-9e3d-4d51-9831-48084607ae20)

![Screenshot (80)](https://github.com/user-attachments/assets/7a5061ea-1535-42d0-819c-dd1b05c8864e)

![Screenshot (82)](https://github.com/user-attachments/assets/2d6e959e-6692-4d3c-9f57-a121c6d0f5dd)

![Screenshot (83)](https://github.com/user-attachments/assets/5aadd0f0-e309-4cd6-9ede-9b377e74446e)

![Screenshot (85)](https://github.com/user-attachments/assets/b6eac1c2-eee3-4f0b-80a4-0727b3e335b6)

![Screenshot (86)](https://github.com/user-attachments/assets/ea6641bf-0da7-402c-a5ff-d7df8102694f)

![Screenshot (87)](https://github.com/user-attachments/assets/d2decee0-e922-4513-aa7e-93acc4c61189)
