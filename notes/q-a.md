# Questions and Answers

Trying to organize my approach...top down or bottom up?

* How are voices triggered?
	* I'd assume writes to the amplitude latches, then a set/clear of the trigger latches (schem mentions 2 msec trigger pulses)
	* Need to revisit decoding of that region...0x40xx?
* Which low level hardware gets configured?
	* Serial gets set for 31250, 8,n,1
	* ADC?  
		* Channel 0 = tempo
		* Channel 3 = global accent
		* Why 0 and 3? (other pinfuncs?  Converter does groups of 4)
	* Clocks?
	* DIN sync?  
		* In vs Out
		* INT on DIN CLK in
		* HW or SW driving DIN clk out?
* Which interrupts get enabled?
	* Ser RX only in MIDI modes?
	* Vectors meaningfully populated?
		* 0x00 - yes - Reset...jumps to 0x02cd
		* 0x04 - no - NMI , external...enable interrupts, return
		* 0x08 - maybe - interval counter TM0, TM1.  CALT to 0x2b?
		* 0x10 - yes? - external int, rising/falling - stores ctxt, jumps to c5 or bc (based on vv:63 bit 1).
		* 0x18 - Yes - ETM0 ETM1 timers - check vv:67, jmp to e6
		* 0x20 - Yes - ADC and timer/evt counter - looks like it's counting something
		* 0x28 - Yes - Serial empty of full - jump to x162F
		* 0x60 - No - SWI ...it's in the middle of other tabular funcs...
	* And accesses to MKH/MKL:
		* MKH: (only 3 bits def'd: 0x07: ser tx, ser rx, AD)
			* (01a9) | 0x01 -
			* (02c8) & 0xfe -
			* (0342, 1dbc) = 04  
			* (0471, 0b53, 0b9b, 1a1b, 1a6a) = 00
			* (0d0e) & fb
			* (1326) check of 04
			* (15df) | 04
			* (1638, 18ba, 1b8f) check 04
			* (16f1) & fb
			* (1b96) = ff
			* (1c10, 1da0) = fe
			* (1ff2) subtract 0x68 ???
		* MKL: (EIN, E1, E0, Int2, int1, T1, T0, undefd)
			* (0345) | 0xbf - clear E1 mask
			* (0c4e, 1c5a) = 0xbf - clear E1 mask
			* (1486) & f7 - clear int1 mask
			* (148d) | 08 - set int1 mask
			* (14be) = ef - clear int2 mask
			* (1b93) = ff - set all masks
			* (1dc5) = bf or b7 (cond)  


* Something reads the buttons at startup to decide to show FW version
* How are patterns stored/represented?
