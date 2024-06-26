# spoonfeeder
experimental idea for booting a Radio Shack Color Computer by a Pi Pico(W) by Spoonfeeding

Based on this Proto Board:
https://raw.githubusercontent.com/JayesonLS/TandyCircuitsAndLogic/master/CoCoProtoBoard/CoCoProtoBoard.png

## Working Spoonfeeder for 1/2-text-screen SemiGraphics Game on Aardvark

Plug Aardvark into coco2.

Load micropy/step5.py into Thonny.

Turn on the coco2.  The screen will be frozen.
That's because the Aardvark is holding HALT down,
and waiting until it sees a RESET.

So click the RESET on the back of the coco2.

You should see this in the Thonny console:

```
MPY: soft reboot
Step2: waiting for RESET.
got RESET.
debounced.
RESET gone.
SLEPT half a second.
Activated onreset prog.  Deactivating.

(0034ccff, 000002fd) Activated sm2. Deactivating.
(0060ccff, 000102fd) Activated sm2. Deactivating.
```

And the coco2, after spoonfeeding all the bytes,
should be playing Life on the upper half of the
screen.   There will just be dots on the lower half.

Why only half the screen?  I wanted this to work on
a 4K coco1, and I didn't have enough RAM for the
extra Life arrays in the code that I wrote,
if the screen was full-sized.

## Demo of Control Write & Data Read on Aardvark

`sunday/alfadown-ff7b.py` was not outputting stuff
correctly when I last touched it.

But thanks to Phoenix and Thomas, they fixed the
problem with Pin Directions.  I was using a PIO opcode
that doesn't work for Pin Directions.

A small BASIC program can be typed in to make
the demo work:

```
10 POKE &HFF7A, 22  : REM write any byte to Control Port
20 FOR I=1 TO 8
30 X = PEEK(&HFF7B)
40 PRINT X, CHR$(X)
50 NEXT I
```

When that is run, the POKE to the control port will
enable the Data Port machine to operate.  Then we can
read the "Hello world..." message.  That FOR loop
just reads the first 8 bytes of the message.

Also if you want the Pico W to power up into a more
sane state -- so it doesn't cause the coco to stall
(due the the HALT being low) -- load `sunday/aard_boot.py`
into the Pico's filesystem with the name `boot.py`.

## END
