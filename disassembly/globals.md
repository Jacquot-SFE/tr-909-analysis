# Globals referenced in the source

It looks like the global data is kept in the bottom end of on-chip RAM (0xff00 to 0xffff).

The stack is at the top of it, and grows downward.  There don't seem to be a lot of push/pop operations.

So globals are referenced using the V register.  V is loaded at init as 0xff (as is it's counterpart, V'), and only touched one other place (cleared at 0x9c).  So we can assume that operands using "VV:" addressing are accessing globals.

What globals are there?  How do they appear to be used?

|Offset from VV:|Where ref'd|Type|Possible usage|
|-|-|-|-|
|02|0020, |Word|Timer 0,1 ISR|
|08|2be|bits?||
|09|2c1|||
|10|0152||passed as HL Ptr to func?|
...inc from 10...
|16|0308||Init to 0x10|
|17|0146||Scanner? Last observed states?|
|18|0185||passed as HL Ptr to func?|
...inc from 18...
|1e|030b||Init to 0x18|
|1f|014d||Scanner state?|
|24|14ae|bits| important state??|
|25|0ba6||init to 0f|
|28|0ba8||init to 0f|
|2b|0baa||init to 0f|
|31|0319||init to 0|
|33|0a14|bits||
|34|0a3e|||
|35|0140|||
|3c|0302||Init to 0x0a|
|40|0017 |Word, counter | timer interrupt |
|41|02ff||Init to 0x09|
|4b|15bd|txbyte|staging transmit status byte?|
|4C|0313||init to 0|
|4D|0315||init to 0|
|4e|0317||init to 0|
|4f|031b||init to 0xA0|
|50|031e||init to 0xa0|
|59|0b28|||
|5a|0020, |Counter|IRQ4 - AD complete|
|60|0d9d|||
|61|0da0|||
|62|19e3|Counter?|Seems to init to 4?  Or x17|
|63|0012, 00e8, |bits|IRQ2,3...|
|64|013f|bits|scanner related?|
|67|0018, |Bits?|IRQ3 Interval counter|
|69|1a03|bits|?|
|6a|030e||init to 0xb3|
|6e|0d55|bits||
|73|18e3|||
|74|0305||init to 0x01|
|D0|15b2|txbyte|staging transmit data byte?|

Stack is growing downward from 0xffff.

That D0 is puzzling, in that regard.  Does it never use more than a few bytes of stack?


# Expectations

Things I'd expect to see represented

* Current song/pattern#
* Int/ext memory
* Sync Mode: MIDI, int, DIN
* Shuffle/swing setting
* Current beat/step/24 ppq in some form.
* Scanner state (row/column)
* Matrix of LEDs to light (how is blink handled?)
* Playing state
* Global accent reading
* Tempo for local clock
* Tape stuff?  Yipe.

# Patterns

How are patterns represented?
An array of steps?
With norm & accent per voice?
And a "store tempo in pattern" flag
a
