# Kobra 2 Neo Klipper Config

These are the configuration files I'm using to run Klipper on Kobra 2 Neo.

---

## Warning

Before using this make sure you use `PROBE_CALIBRATE` and manually calibrate the z-offset.

Alternative you can check out the `Z_ENDSTOP_HIT` macro to get the offset. Read the comments first in the `macro.cfg`.

Then start to do you own bed mesh leveling!

In order to properly calibrate the z-offset and bed leveling you need set the temperature of the bed and extruder to the printing temperature you are using for the fillament you have. Make sure your noozle is wipe clean when you do this procedure.

---

## UART

You can also chose to use UART interface if you are fine with soldering two wires.

Check `printer.cfg` and the wiring images in the [ImagesMbWiring](ImagesMbWiring).