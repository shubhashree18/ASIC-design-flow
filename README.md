# RTL to GDSII flow 

# Introduction to RTL to GDSII

The RTL to GDSII flow is a critical process in the design and fabrication of digital integrated circuits (ICs). It represents the transformation of a high-level Register Transfer Level (RTL) description of a circuit, typically written in hardware description languages like Verilog or VHDL, into the final GDSII format. This GDSII format is used to create photomasks for semiconductor fabrication.

![Screenshot (58)](https://github.com/user-attachments/assets/0898c72b-074f-4586-99cf-402df254dbf5)


## 1. RTL(Register Transfer Level) :
* Code involves describing the digital logic of a circuit in terms of data flows and the operations performed on data using an HDL(hardware description language) ex: Verilog, Systemverilog, VHDL.
* Example: A simple RTL Verilog Code of a Inverter shown Below:

   ![Screenshot (59)](https://github.com/user-attachments/assets/ed54d7ce-20dc-4ae7-91f5-a0de1aa03513)

## 2. Functional Verification :
* The primary goal of functional verification is to confirm that the design, described using an HDL like Verilog or VHDL, correctly implements the specified functionality.
* This involves checking that the design meets all the requirements and specifications without any logical errors using tools such as Modelsim ,QuestaSim, Xcelium,etc.
* For Functional Verification we require the Design files and the Testbench of the Design written in HDL like Verilog or VHDL Through which we can generate a Waveform to
verify the functionality of the Design is correctly implemented as written in the Code.

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

## 3. Floor planning 

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
