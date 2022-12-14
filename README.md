# Tidal

**What is this?**

A vertical mouse with a sensor and report rate that doesn't suck.

<img src="https://user-images.githubusercontent.com/60482558/184889012-e802225c-90c1-4e23-8474-9f53ecea9dff.png" height="230"> <img src="https://user-images.githubusercontent.com/60482558/185208380-d320a7cb-e078-474e-af08-3384783d78df.png" height="230"> <img src="https://user-images.githubusercontent.com/60482558/185208451-0a71bb99-5001-4fe7-8753-b52eb4c2303a.png" height="230">

(to note, the bottom cover on this example is different than the newest version, the newest version supports more sensor breakout boards)

**Why again?**

I researched several vertical mice, and they all seem to be either geared towards working (they have mediocre sensors, and are limited to 125HZ report rates because they are intended to be used over bluetooth), or are bottom of the barrel "gaming" trash (they have trash tier hardware, and some crappy LEDs tacked on to them). This mouse has a PMW3360 sensor, and a 1000HZ report rate. It can have LEDs, but it doesnt need to to be awesome. It runs QMK, so it is extremely extensible.

**Where do I start?**

Download the STLs and print them. They are designed to be printed without supports for the most part. The main body will need support from the build plate, and the buttons will likely need skirts, and some trimming.

The really only tricky thing to print are the buttons, they should be oriented on the build plate with the mousewheel cutout facing upwards. This will lay the button on a relatively thin piece. If you are printing in something other than PLA (I've printed this in PLA, PETG, and ABS) then its advisable to use a skirt, but hey, I'm not a cop, do what you think is right. They should also be printed slowly, and with a little extra fan than usual (I hope you have an enclosure if you print these in ABS). In my experiments, I found that that limited warping.

<img src="https://user-images.githubusercontent.com/60482558/185725709-8130b86a-ec81-4335-8262-8b37a3b8504a.png" height="230">

You will need to be suitably comfortable with soldering thin wire to 0.1" pitch pads, in relatively tight spaces. A clamp big enough to secure the mouse body while working on it is recommended.

**Whats next?**

**You need some hardware:**

1. at least 9 heat set inserts, https://www.aliexpress.com/item/4000232858343.html in M2 X D4.0 X L3.0
2. at least 9 screws, https://www.aliexpress.com/item/4000282581926.html, in M2 X 8

**And some electronics:**

1. Buy a logitech g403, or g703 from ebay, they are cheap, and have the sensor you need (if you have desoldering tools), as well as the scroll wheel module. You can harvest the switches, but I recommend against it. They arent great.
2. you will need switches, I recommend these: https://www.aliexpress.com/item/4000470902035.html, but you do you. Be sure to get the ones with the bigger mounting holes. M2 hardware needs to fit.
3. a pro micro, with USB C: https://www.aliexpress.com/item/1005003622414316.html
4. you will need a breakout board for the sensor. Here you have some options
    1. If you are hardcore: desolder the sensor from the mouse you bought, then get one of these and assemble: https://github.com/Ariamelon/Ogen
    2. If you are looking for an easier version of #1: https://www.tindie.com/products/jkicklighter/pmw3360-motion-sensor/
    3. If you are cheap like me: https://keymouse.com/keymouse-components/keymouse-mouse-sensor-module-3-0t If you go with this one, do be advised that the silkscreen on the board may be reversed, ie: pin 1 is pin 8, pin 2 is pin 7, etc. That was annoying to figure out. You also need to desolder the header, it wont fit.
5. RGBLEDS! I used these: https://www.adafruit.com/product/4368, and needed 25 leds out of the string.-
6. mouse feet. I use feet for a G102 Gpro: https://www.amazon.com/gp/product/B072K1F7C1 You can really use whatever fits. The bottom of the mouse is flat.
    
**And some tools:**

1. soldering iron
2. solder
3. thin wire, I like to use this stuff: https://www.adafruit.com/product/3169. It is also available from digikey (and canadian digikey!)
4. tweezers, nice fine ones
5. 3m vhb tape, thin, in half inch width: https://www.amazon.ca/gp/product/B06Y315PXN
6. a good set of small screwdrivers
7. a soldering vise or clamp of some sort makes this job way easier, especially soldering the wires to the pro micro while its in the body of the mouse.

**Alright, now, to get started assembling!**

1. do all of the heatset inserts. There are 9. 3 in the bottom, 4 in the button mounts, and 2 in the switch mounts.
2. disassemble the mouse you bought. There are youtube guides for this. You will need to keep both small mouse wheel pcbs, the wheel itself, and the cradle that holds the boards and locates the wheel shaft. The shield thing that goes above the wheel can go. Keep all of the screws, you will need them to mount the scroll wheel cradle and pcbs.
3. if you are hardcore, desolder the sensor from the mouse, and build the board you had made for it.
4. mount the pro micro with the vhb tape, such that the USB port sticks into the front hole.
5. solder wires to the normally open leads of the switches, as well as all 5 pads on the scroll wheel modules 2 pcbs. Make these 5 or 6 inches long.
6. mount the switches to the mouse body.
7. mount the scroll wheel cradle to the mouse body.
8. mount the scroll wheel boards to the cradle. This is a bit finicky given the tight space and the small screws.
9. leave all of the wires out of the channel that runs through the mouse, you will trim and solder these one by one.
    1. one wire from each switch and the middle click board (3 wires total) get joined together, and runs to pad 2 on the pro micro (this is the "row" pad in QMK, this mouse essentially thinks its a 3 key keyboard with one row and an encoder)
    2. the remaining left click wire goes to pad 3
    3. the remaining right click wire goes to pad 4
    4. the remaining middle click wire goes to pag 5
    5. the wire from the leftmost pad (or topmost, if the board is mounted in the mouse) on the scroll wheel encoder board goes to ground
    6. the other two wires from the scroll wheel encoder go to pads 6 and 7 on the pro micro. If the scroll wheel scrolls backwards, flip these two wires.
10. now wire the sensor. (remember the silkscreen might be backwards)
    1. run 3 inch wires off of all of the pads on the sensor except motion (its not needed).
    2. ground to ground
    3. clk to pad 15
    4. mosi to 16
    5. miso to 14
    6. cs to 10
    7. vcc to vcc (this ensures that the sensor is protected by the pro micros protection circuitry.
11. if you are not mounting LEDs, you are done wiring! You can skip this step!
    1. solder short wires to all 3 pads on the led strip, and then insert the strip carefully into the channel (this is tricky, be careful and go slowly. It is also possible to do this step first and then you dont have to deal with other wires being in the way). Do not peel the backing paper off of the leds, this will prevent them sliding in. The will stay where they are just fine.
    2. gnd to gnd
    3. signal to A3
    4. vcc to raw (this ensures that the LEDs get enough power, by bypassing the protection of the micro. Heres hoping your PC doesnt do strange things with the 5v USB rail!)
12. now you can fit the bottom cover. The sensor board should be located by the lens fitting into the bottom cover, and can be held in with some vhb tape between the board and the top surface of the bottom cover. After that you can do up the screws on the bottom.
13. fit the mouse buttons. These may need some tweaking and can be adjusted by sanding down the mount bosses on the buttons, or the boss that actually presses the switch. Take your time here and get it right, this really affects the feel of the mouse.
14. fit some mouse feet.
15. load the qmk firmware. This can be found here: https://github.com/patrickmcquay/qmk_firmware, under keyboards/pmcquay/tidal. A description on how to use qmk msys can be found here: https://msys.qmk.fm/guide.html. You will have to leave the bottom cover off to short the reset pad to ground with a tool. The tweezers work well for this. No I will not answer questions on how to deal with qmk msys, that is not software I wrote.
16. test. Make sure the buttons work and the scroll wheel scrolls the right way. The sensor can also be installed backwards. I've had an issue where when I first power it on, the mouse will lose one step when the scroll wheel changes directions, I'd imagine this is a qmk bug, as I've had it happen the same way on two of these now that had different encoders. I'm not sure what else it could be really.
17. optionally, you may want to purchase or make a flexible lightweight usb type c cable. Its not terribly difficult to make them, I used the same 30 guage silicone wire, as well as some paracord sheath, 1/2 inch 4:1 shrink tube, and these: https://www.aliexpress.com/item/1005001899869913.html. Find a pinout for the type A connector and then just join up the pads. If you got this far this will be trivial.

Enjoy, This is by far the most responsive vertical mouse that I've used. Let me know if this document can be improved or if you have any issues.    

Shield: [![CC BY-NC-SA 4.0][cc-by-nc-sa-shield]][cc-by-nc-sa]

This work is licensed under a
[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License][cc-by-nc-sa].

[![CC BY-NC-SA 4.0][cc-by-nc-sa-image]][cc-by-nc-sa]

[cc-by-nc-sa]: http://creativecommons.org/licenses/by-nc-sa/4.0/
[cc-by-nc-sa-image]: https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png
[cc-by-nc-sa-shield]: https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg
