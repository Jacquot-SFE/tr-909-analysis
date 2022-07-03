# uPD8711 notes

_as largely extracted from the datasheets, reading disassembly, etc..._

If you want the background that gets here, there are 3 CPU documents to track down:

* upd7810/11 manual
* upd87c10/11 manual
* upd8710/11, 7810H/87811H, 78c10/c11/c14 Microcomputers ()

The first two are very similar, and light on details.  The third is 189 pages, and has those missing details.

## Boot Mode

Mode0,1 buth hi thru 4k7's =

## Instructions with skip conditions

* MVI
* LXI
*

## Memory Map

From Datasheet:
* Internal ROM 0x0000 to 0x0fff
* "External Memory" 0x1000 to 0xfeff
* Internal RAM: 0xff00 to 0xffff

My understanding:
* External EEPROM 0x0000 to 0x1fff
* External RAM    0x2000 to 0x3fff
* Analog interface outputs 0x4000
   * Read from 4000 means a keyboard row (as sel by PA)
   * Write to 400X voice outputs?
* Cartridge 0x6000?
* ? 0x7000
* Internal RAM    0xff00 to 0xffff

Where are the triggers/vel dacs?

* Decode of A[0,1,2]
* !WR
* A[13,14,15] = 010 (0x4xxx)

Where are the switch inputs?  Mapped to data bus.

* At address?
* !RD
* A[13,14,15] = (0x4xxx?)

## Execution Registers

One each of the following

* PSW - 8b - Program Status Word
   * b0 - Carry
   * b1 - RFU
   * b2 - L0 - Overlay Zero
   * b3 - L1 - Overlay One
   * b4 - HC - Carry from bit 3 to 4
   * b5 - SK - Skip
   * b6 - Z - Operation result was zero
   * b7 - RFU
* PC - 16b - Program Counter - next instr to be fetched
* SP - 16b - Stack Pointer - grown downward

Two sets of the following (IE A and A'), some with swap instructions to trade contents.

* EA - 16b - Arith & logic operations
* A - 8b - Arith & logic operations
* V - 8b - Vector - MSB's of a page indexing reg (LSBs will be immed)
* B, C - Dual 8b, gang to 16b
* D, E - Dual 8b, gang to 16b - with indexing, inc/dec
* H, L - Dual 8b, gang to 16b - with indexing, inc/dec


## Interrupts

* IRQ0 - vec at 4 - External NMI
* IRQ1 - vec at 8 - Timers 0,1
* IRQ2 - vec at 16 - External interrupt 1,2
   * EI 1 is DIN Tempo
* IRQ3 - vec at 24 - Timer/event counter 0,1
* IRQ4 - vec at 32 - , AD complete
* IRQ5 - vec at 40



## Periph Registers

Apparently named registers (sim to Accum, index?), not memory mapped slots?

### Parallel Ports

* PA - port A - Scanner rows out
* PB - port B - LED scan columns Anode side w/ NPN pulls.</br>
 Button Scan read is memory mapped - IC610
* PC - port C - General IO
	* PC0 - TXD - MIDI out
	* PC1 - RXI -  MIDI In
	* PC2 - Footswitch
	* PC3 - Timer input - tape in
	* PC4 - Timer output - tape sync out
	* PC5 - Counter Input - DIN sync start
	* PC6	- Counter out 0 - DIN Continue
	* PC7 - Counter out 1 - unused.
* PD - port D - Multiplexed Address/Data Bus
* (No Port E?)
* PF - port F - Address bus MSBs

### Analog inputs

Pins AN[0..7], mostly unused

* AN0 - Tempo
* AN3 - Voice board master accent signal.

Registers:

# Instruction Notes

*Really need to understand the conditional stuff.*
*Really need to understand some of the addressing....*

* Jumps: simply modify the PC (and some status flags) - nothing on stack
   * JR: Jump Relative, 6 bits tot (+32, -31)
   * JRE: Jmp relative, extended, 9 bits tot (+255, -255)
   * JMP: Jump, full 16-bit operand
   * JB: Jump to addr in BC (1 byte instr)
   * JEA: Jump to addr in EA (2 byte instr)

## Calling

Calling sets up a return address to be popped by a ret.

* CALL: call routine with 16 bits immed data.  3 byte, 16 state
* CALB: (unused here) call to address in BC.  2 byte, 17 state
* CALT: call routine thru table: 32 16bit addresses loc'd at 0x0080..0x9f.  **Shortest**  1 byte, 16 state
* CALF: call routine at 0x0800 | (0x03ff & operand) **Fastest** call: 2 byte, 13 state**
* SOFTI
