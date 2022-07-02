# Notes Specific to the TR909 CPU board

_And related software implementation stuff that doesn't really fit in the "chip" analysis._

## MIDI

### Drums

Defaults to transmit on channel 11, receive on channel 10?

* 35,36 (0x43, 0x44) = kick
* 37 = rim shot
* 38,40 = snare
* 39 = hand clap
* 41,43 = Low tom
* 42,44 = Closed hat
* 45,47 = Mid tom
* 46 = Open hat
* 48,50 = Hi tom
* 49 = Crash
* 51 = Ride

Velocity is mapped in the 64-96 range?  

### External Sequence

??

## Scan Matrix

### LED Scanner

Rows selected by PA 0:7
Columns selected by PB 0:4

|Col|Func1 |2|3|4|5|6|7|8|
|---|------|-|-|-|-|-|-|-|
|1  |step1 |2      |3    |4   |5  |6    |7    |8      |
|2  |step9 |10     |11   |12  |13 |14   |15   |16     |
|3  |      |Tk1    |Tk2  |Tk3 |Tk4|Pt1  |Pt2  |Pt3    |
|4  |entr  |ExtInst|Tempo|Back|Fwd|Cycle|Avail|Tapesnc|
|5  |Scale |Scale  |Scale|Scale|  |     |     |       |

### Switch Scanner

Rows selected by PA 0..4

Steps are fully diode isolated
Others are isolated by row

|PAbit|1|2|3|4|5|6|7|8|
|-----|-|-|-|-|-|-|-|-|
|0|step1      |2      |3    |4    |5    |6   |7    |8    |
|1|Step9      |10     |11   |12   |13   |14  |15   |16   |
|2|shift      |scale  |     |inst |shuf |last|stop |start|
|3|Clr        |t1     |t2   |t3   |t4   |pat1|pat2 |pat3|
|4|ent/accent |tapesyn|cycle|avail|fwd  |back|tempo|extinst|
