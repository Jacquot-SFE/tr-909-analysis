# Instructions With Conditional Skip of Next

_Because they sometimes lurk in unexpected places._

## Suffixes

Instruction suffixes often encode where the operand is, and there are similar families of instructions with different oper locn's.

**W doesn't mean Word.  It means indexed off VV.s**

* W = Working Register = offset from VV
* A = Register A
* I = Immediate
* X = indirect, addr by reg pair

## Status Reg

Conditions reflected in the status register

|bit|abbrev|function|
|---|------|--------|
|0  |CY    |Carry/Borrow (bits 7 or 15) |
|1  |x     |unused  |
|2  |L0    |Overlay 0 |
|3  |L1    |Overlay 1 |
|4  |HC    |Half Carry - bit 3 to 4 |
|5  |SK    |Skip condition active |
|6  |Z     |Zero |
|7  |x     |unused |

Overlay 0 & 1 are complicated, something to do with skipping sequences of MVI L (L0), LXI H (L0), MVI A (L1),  

## Marking these in the disassembly

The comment at column 38 has been `//`.
I'll follow instructions that check a condition with a `/c`
And the optionally skipped one with '/s'
It looks like there are cases with series of conditionals, so marking them `/cs`
And I'll put an `x` in the table below when the disassembly has no instances of the instruction, or a number indicating how may there are.
## Instructions

### The common ones

* BIT - test a single VV:xx bit, skip next if set.
* OFFI - and a reg against immed bit pattern, skip next if result is 0
* OFFIW - and a VV against an immed bit pattern, skip next if result is 0
* ONIW - and a VV against an immed bit pattern, skip next if result is nonzero
* DCR - decrement reg, skip next if it borrows
* DCRW - decrement VV reg, skip next if it borrows
* INR - increment reg, skip next if carry
* INRW - increment VV reg, skip next if carry
* DEQ - 16-bit, skip next if equal
* DGT - 16-bit, skip if GT
* DLT - 16-bit, skip if LT

### The complete list



* Adds (Subts) that check no carry (borrow) (NC = skip next if No Carry):
	* ADDNC x
	* ADDNCW x
	* ADDNCX x
	* ADINC 4
	* DADDNC x - double Add: 2regs to EA, check carry
	* DSUBNB 4 - double subt: 2 regd from EA, no borrow
	* SUBNB 1 - effectively a GT test
	* SUBNBW 1 -
	* SUBNBX x
	* SUINB 2
* Bit tests
	* BIT 168 - skip next if bit is SET
	* DOFF x -
	* DON x -
	* OFFA x -
	* OFFAW x
	* OFFAX x
	* OFFI 30
	* OFFIW 48
	* ONA 2
	* ONAW x
	* ONAX x
	* ONI x
	* ONIW 17
* Ind/Dec-rements
	* DCR 23
	* DCRW 20
	* INR 26
	* INRW 13
* Equality, GT/LT
	* DEQ 5 - 16-bit equality
	* DGT 9 - 16-bit skip if GT
	* DLT 14 - 16-bit skip if LT
	* DNE 2 - 16-bit skip if not equal
	* EQA 3 - skip if equal
	* EQAW 4 - skip if equal
	* EQAX 2 - skip if equal (indirect)
	* EQI 25 - skip if equal, immed
	* EQIW 17 - skip if equal
	* GTA 3 - skip if LH GT RH
	* GTAW 4 -
	* GTI 13
	* GTIW 1 -
	* LTA x
	* LTAW 4
	* LTAX x
	* LTI 24
	* LTIW 3
	* NEA 4
	* NEAW x
	* NEAX 1
	* NEI x
	* NEIW 16
* Explicit Skips
	* SK 3 - skip on CY/CH/Z
	* SKIT 10 - interrupt flag
	* SKN 5 - no flags set
	* SKNIT 4 - skip on no interrupt flag
* Shifts
	* SLLC - logical left, skip next on carry set
	* SLRC - logical right, skip next on carry set
