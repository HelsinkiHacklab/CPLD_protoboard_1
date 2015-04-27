# CPLD_protoboard_1
Dual CPLD based prototyping and breadboarding support board

The purpose of this combined HW/SW project is to produce a configurable prototyping board for various digital tech labs and demos.
The main features of the board:

- 2 CPLDs (Xilinx XC9572XL), 5V input tolerant 72 macrocell devices in 44 pin QFP package. These control the input and output signaling and on-board input and output devices.
- a fast clock generator producing a 50 MHz tick for synchronous logic
- a variable frequency slow clock to provide a clock tick on the human scale for testing breadboard circuit logic.
  The frequency range is roughly < 1Hz to ~100 Hz, adjustable with a simple potentiometer.
- Power is supplied either via a mini-USB connector (power only) @ 5V or alternatively via a DC jack (2mm pin, 5.5mm barrel, pin positive) @ max 15V.
  On board regulators produce the required 5V and 3.3V supplies to the electronics.
  Output signal voltage levels are selected by a 3 pin header next to the power input jack.
- The CPLDs are connected into a JTAG chain for programming. A 6 pin (2x3) header is provided for connection. The signal numbering is according to Atmel ISP header convention.
  A custom JTAG cable is needed to adapt the Xilinx programmer for this board. Instructions will be provided in the project documentation.

Inputs:
- 8 tactile pushbuttons with buffered outputs brought to pin headers for connection into the user circuit. 
- 8 SPDT switches with buffered outputs to pin headers.
- 1 quadrature encoder with push switch (24 increments / rev), also these signals are buffered and brought to pin headers.

The buttons and switches are under CPLD control; the input goes to CPLD and from the CPLD to the output pin headers via a logic buffer circuit.
This allows the output pin behavior to be controlled by the program in the input CPLD.
To facilitate synchronous logic the slow and fast clock signals are brought to the input CPLD.

Outputs:
- 1 buffered bidirectional uncommitted port ( 8 signals ) - with the direction controlled by user or output CPLD.
  This port can be either an input or an output depending on the state of the port direction control signal.
  The port signals are buffered and brought to the output CPLD and are freely usable as inputs to, or outputs from the output CPLD.
- 1 unbuffered port connected to 8 + 8 indicator LEDs. The port signals are connected to the output CPLD as well as 2 banks of 8 LEDs.
  The LEDs are multiplexed by the output CPLD under program control. By default LED bank 1 is selected.
- 1 unbuffered port connected to a 2 digit 7-segment display. The digits are multiplexed using the same signaling as the LED banks.
- 3 unbuffered signals controlling a RGB LED. Also these signals are replicated to the output CPLD.

Unbuffered input signals are connected directly to the CPLD pins so care must be taken to avoid overvoltages and overloads. The CPLDs are 5V tolerant but no more than that.
However, all the various LEDs are driven from these signals using separate driver FETs so the LEDs do not load the user signals.
Note that the CPLD signals connected to LEDs and pin headers can be either inputs or outputs.
This means that the CPLD can either read the external signal controlling the LED or it can provide the LED control (and output signal at the same time).

A basic Verilog application will be provided for both CPLDs such that the buttons and switches are replicated to the outputs and LEDs and 7 segments are under user control.
Many other combinations are possible as well.
