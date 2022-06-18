# uPD8711 notes

_as largely extracted from the datasheets, reading disassembly, etc..._

## Boot Mode

Mode0,1 buth hi thru 4k7's

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
* Internal RAM    0xff00 to 0xffff

Where are the triggers/vel dacs?

## Execution Registers



## Interrupts

* IRQ0 - vec at 4 - External NMI
* IRQ1 - vec at 8 - Timers 0,1
* IRQ2 - vec at 16 - External interrupt 1,2
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
