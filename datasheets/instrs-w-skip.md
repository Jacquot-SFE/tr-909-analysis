# Instructions With Conditional Skip of Next

_Because they sometimes lurk in unexpected places._

## Suffixes

Instruction suffixes often encode where the operand is, and there are similar families of instructions with different oper locn's.

**W doesn't mean Word**

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

## Instructions

* Adds (Subts) that check no carry (borrow) (NC = skip next if No Carry):
	* ADDNC
	* ADDNCW
	* ADDNCX
	* ADINC
	* DADDNC - Add 2regs to EA, check
	* DSUBNB
	* SUBNB  - effectively a GT test
	* SUBNBW -
	* SUBNBX
	* SUINB
	* SUINB
* Bit tests
	* BIT - skip next if bit is SET
	* DOFF -
	* DON -
	* OFFA -
	* OFFAW
	* OFFAX
	* OFFI
	* OFFIW
	* ONA
	* ONAW
	* ONAX
	* ONI
	* ONIW
* Ind/Dec-rements
	* DCR
	* DCRW
	* INR
	* INRW
* Equality, GT/LT
	* DEQ - 16-bit equality
	* DGT - skip if GT
	* DLT - skip if LT
	* DNE - skip if not equal
	* EQA - skip if equal
	* EQAW - skip if equal
	* EQAX - skip if equal (indirect)
	* EQI - skip if equal, immed
	* EQIW - skip if equal
	* GTA
	* GTAW
	* GTI
	* GTIW
	* LTA
	* LTA
	* LTAW
	* LTAX
	* LTI
	* LTIW
	* NEA
	* NEAW
	* NEAX
	* NEI
	* NEIW
* Explicit Skips
	* SK - skip on CY/CH/Z
	* SKIT - interrupt flag
	* SKN - no flags set
	* SKNIT - skip on no interrupt flag
* Shifts
	* SLLC - logical left, skip next on carry set
	* SLRC - logical right, skip next on carry set
