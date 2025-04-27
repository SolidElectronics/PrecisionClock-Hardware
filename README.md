# PrecisionClock-Hardware
Alternate board designs for [mitxela/PrecisionClockMkII](https://github.com/mitxela/PrecisionClockMkII)

[0.56 inch x 18](/0.56%20inch%20x%2018) is a fully compatible version of the original design, but using the more readily available 0.56" displays.

[two row tenths v2](/two%20row%20tenths%20v2/) is a new design that requires modified firmware which you can get from [SolidElectronics/PrecisionClockMkII](https://github.com/SolidElectronics/PrecisionClockMkII).  That firmware supports both the original and new board versions (select using the #define statements).  The major changes included in this board are:

- Stacked date above time.  Date uses 0.56" LEDs and time uses 0.8" LEDs with 3mm LEDs for colons.

- USB-C power input, with footprints for mounting the connector on the side or the bottom.

- Dedicated holes for antenna mount, and I included STLs for the common ceramic antenna included with the GPS modules, and an SMA-mount if you'd prefer to use that.  I also added optional covers that can go over the colon indicators to make it look a bit more finished.

- Use surface-mount components wherever possible (not the AVR or MAX7219s because I already had the DIP versions)

- Switched non-segment indicators (colons and GMT/BST indicators) to use dedicated output pins on the MCU.  I found that driving anything other than the segments from the MAX7219 caused ghosting on the associated digits, and there wasn't a way to reduce the LED brightness that worked.

- Implemented digital brightness control.  The analog brightness control was great in theory but I couldn't get it to drive the LEDs at a level I was happy with, it would either be too bright or they'd go out entirely in a dark environment.  This probably could have been resolved by picking the correct transistors, LDR, and resistor values, but it was too much of a pain to re-solder them every time.

  Some concessions had to be made here due to the hardware limitations of the ATTiny2313, there is no ADC and only one available PWM output.  The LDR now uses the analog comparator to switch beteen high and low brightness, not a smooth gradient.  The non-segment indicators are PWM-driven through a 74AHCT125 so their brightness can be set independently from the main display, and the main display now uses the built-in dimming function from the MAX7219.  All of the parameters are tunable in the firmware, and there are potentiometers on the back to set the dimming switchover point and the overall date and time brightness levels.

It took a lot more work than I expected to get it to this point, and it was probably the most trace-dense board I've made, but overall I'm happy with how it turned out and it's now a permanent fixture in my office.