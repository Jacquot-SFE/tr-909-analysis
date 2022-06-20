# Globals referenced in the source

It looks like the global data is kept in the bottom end of on-chip RAM (0xff00 to 0xffff).

The stack is at the top of it, and grows downward.  There don't seem to be a lot of push/pop operations.

So globals are referenced using the V register.  V is loaded at init as 0xff (as is it's counterpart, V'), and only touched one other place (cleared at 0x9c).  So we can assume that operands using "VV:" addressing are accessing globals.

What globals are there?  How do they appear to be used?

|Offset from VV:|Where ref'd|Type|Possible usage|
|-|-|-|-|
|02|0020, |Word|Timer 0,1 ISR|
|16|0308||Init to 0x10|
|1e|030b||Init to 0x18|
|31|0319||init to 0|
|3c|0302||Init to 0x0a|
|40|0017 |Word, counter | timer interrupt |
|41|02ff||Init to 0x09|
|4C|0313||init to 0|
|4D|0315||init to 0|
|4e|0317||init to 0|
|4f|031b||init to 0xA0|
|50|031e||init to 0xa0|
|5a|0020, |Counter|IRQ4 - AD complete|
|60|0d9d|||
|61|0da0|||
|63|0012, 00e8, |bits|IRQ2,3...|
|67|0018, |Bits?|IRQ3 Interval counter|
|6a|030e||init to 0xb3|
|74|0305||init to 0x01|
