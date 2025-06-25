---
title: "Upgrading and Klipper-izing the Ender-3 S1 for Quality 3D Prints"
date: 2025-06-01
last_modified_at: 2025-06-01
tags:
  - 3D Printing
header:
   image: /assets/images/headers/2025-06-01-upgrading-ender-3-s1-header.jpg
   teaser: /assets/images/headers/2025-06-01-upgrading-ender-3-s1-header.jpg
---

Since [obtaining an Ender-3 S1](https://www.codyhou.com/getting-into-3d-printing/) for 3D printing, I've learned a lot about the printer and made many modifications to improve print quality. 

## Basic 3D Printer Tuning

Before going through anything else I would highly recommend going through [Ellis' Print Tuning Guide](https://ellis3dp.com/Print-Tuning-Guide/) first. Some notes:

* Squaring the printer: After making sure that the gantry is securely and evenly screwed in to the base, I do this by putting two glue sticks or objects of equal size at the bottom of the Z axes, then loosening the grub screws for the Z axes and bringing the gantry down until it rests equally on both glue sticks. Then I re-tighten the grub screws.
* Extruder calibration: I found that I didn't need to change E-steps on my Ender-3 S1 but your mileage may vary.
* First layer squish: Within my slicer I changed my line width on the first layer to 120% (for a 0.40mm nozzle, 0.48mm).
* Pressure advance: Not available on default or ["Professional"](https://github.com/mriscoc/Ender3V2S1) Ender-3 S1 firmware. See [section on Klipper](#getting-more-features-with-klipper) below. 

## Drying Filament

I would argue this is an essential purchase for 3D printing and would have saved me so much headache if I got one earlier. Plastic filaments are slightly hygroscopic (and some types much more so than others), so for the best print quality and consistency a proper filament dryer will go a long way. I purchased the [Polymaker PolyDryer](https://us.polymaker.com/products/polydryer) and it has made worlds of a difference particularly for PETG. Compared to other models the PolyDryer is fairly quiet and also more properly ventilated.

## Zero Y Offset Mod for Consistent First Layers

As detailed in my previous post, I printed a [zero Y offset bracket](https://www.printables.com/model/438224-x45-sprite-cr-touch-probe-zero-y-offset-mount-v11-) for the Ender-3 S1 to mitigate any potential issues with bed leveling. The exact reasons for this are detailed in more depth on the mod's Printables page linked earlier. Just remember to adjust the probe offset.

## Solving Inadequate Cooling

The default fan on the Ender-3 S1 is woefully inadequate [as I previously encountered](http://localhost:4000/getting-into-3d-printing/#my-first-print). There are many cooling fan solutions out there, including the [Zuff ModulR](https://cults3d.com/en/3d-model/tool/modulr-duct-5015-fan-ender-3-s1-s1-pro-sprite-extruder-accelerometer-led-strip) and [Heartbreaker V5](https://www.printables.com/model/558083-heartbreaker-mk5-a-dual-5015-fan-cooling-system-fo). I went with the [Taurus V5](). All these designs employ 5015 fans which greatly improves cooling.

Currently I'm using some Winsinn fans that I found on Amazon for cheap but I'm seeing reviews and comments on Reddit that the GDSTime-branded fans are much better, with official Sunon 5015s being the best. The Winsinn fans exhibit fan rattle and whine and don't push out as much air, so I find I have to push the fan speed to 20% for PETG and 50% for PLA prints. Too high though and PLA will warp from the bed and bridging will not be good as the molten plastic will bend unevenly. 

## Improving Bed Adhesion

I really like the results of textured plates on prints, but I found that PLA doesn't adhere as well to textured plates. Also, using PETG and PLA on the same plate is a no-go, as the PLA won't stick at all. I went out and got a smooth PEI plate and that solved all my bed adhesion problems. Also don't be like me and leave the plastic protective layer on the PEI sheet; I was getting layer shifts for no reason because of this.

## Being a Good Neighbor

The Ender-3 S1's stepper motors are already fairly quiet, but it's probably a good idea to isolate the noise and vibration from printer head movements, particularly when living in an apartment complex. An easy mod is to buy a cheap concrete and rubber paver and set the printer on top of them. CNC Kitchen made a great [article and video](https://www.cnckitchen.com/blog/reduce-your-3d-printing-noise-with-a-concrete-paver) on this topic.

## Getting More Features With Klipper

Alongside cooling, this is one of the biggest upgrades available for the Ender-3 S1, though I wouldn't recommend getting started with Klipper without a good understanding of 3D printing and completing the previous steps. While Klipper is associated with very fast 3D printers, the biggest upgrades that Klipper offers from my experience compared to stock or "Professional" firmware are the following:

* Adaptive bed mesh: Instead of probing the entire bed, you can probe the specific area being printed and also further customize the number of points probed. This saves a ton of time as I like to calibrate the mesh before every print.
* Pressure advance: This feature allows the Ender-3 S1 to print nice, straight 90-degree corners. I noticed the lack of this feature immediately when my XYZ cube ended up with bulging corners on default firmware.
* Input shaping: Another flaw that may show itself when printing the XYZ cube is resonance, which can manifest as shadowy lines around text features. Input shaping measures the resonant frequencies of the printer and compensates for this.
* Wireless printing: Easy quality of life upgrade. I can export my prints directly from Prusa Slicer instead of messing around with an SD card.

### Installation

Many folks will go out and buy a cheap Raspberry Pi and buck converter so they can run it off of the PSU, but I use an even cheaper method: an old laptop, which can be scrounged for free. Given that Klipper can run off of a Raspberry Pi, it doesn't require a lot of processing power, so an old laptop is usually going to be overkill. I am using a Lenovo ThinkPad X220T which is a touchscreen tablet laptop sporting an Intel Core i5-2520M processor, and during prints CPU utilization never goes above 5%. The laptop can be plugged into the printer's front USB-C port. Any USB-A to USB-C cable with data transmission will do.

Klipper documentation is comprehensive and general installation instructions can be found [here](https://www.klipper3d.org/Installation.html). Given that the OS commonly used for Klipper with Raspberry Pi devices, Raspberry Pi OS, is based off of Debian, I chose to [install Debian](https://www.debian.org/) without a desktop environment. Then, by using the KIAUH helper script, I installed Klipper accompanied with Mainsail (web UI to control Klipper) and Moonraker (API that connects Mainsail to Klipper). Since I have a touchscreen laptop, I also installed KlipperScreen, a GUI touch interface for Klipper. I flashed the firmware to the Ender-3 S1 by following the instructions in the [printer configuration file](https://github.com/Klipper3d/klipper/blob/master/config/printer-creality-ender3-s1-2021.cfg), noting that my Ender-3 S1 motherboard microcontroller was a STM32F401. This can be checked by opening up the bottom cover.

### Basic Configuration and Macros

I used the default Ender-3 S1 configuration file as a starting point. As I'm using the [zero Y offset mod](#zero-y-offset-mod-for-consistent-first-layers), I needed to adjust some parameters as noted below:

~~~
[bltouch]
x_offset: -45.0

[bed_mesh]
mesh_max: 200, 220 # Mesh x-position at 200 is absolute position of nozzle at 245

[safe_z_home]
home_xy_position: 155, 110

[bed_screws]
screw1: 66, 192
screw2: 240, 192
screw3: 240, 22
screw4: 66, 22
~~~

After this, I added a bed screw adjust macro that I found somewhere on the Internet:

~~~
[screws_tilt_adjust]
screw1: 66, 192
screw2: 240, 192
screw3: 240, 22
screw4: 66, 22
screw1_name: Back Left
screw2_name: Back Right
screw3_name: Front Right
screw4_name: Front Left
speed: 200
horizontal_move_z: 5
screw_thread: CW-M4

[gcode_macro SCREW_TILT]
description: Macro to tune bed leveling
gcode:
    G28
    SCREWS_TILT_CALCULATE
~~~

I also added some macros for starting and ending the print. These are based on the default profiles for the Ender-3 S1 in Prusa Slicer.

{% raw %}
~~~
[gcode_macro START_PRINT]
description: Routine to be executed at the start of a print
gcode:
    {% set BED_TEMP = params.BED_TEMP|default(60)|float %}
    {% set EXTRUDER_TEMP = params.EXTRUDER_TEMP|default(190)|float %}
    # Wait for bed to reach temperature before probing
    M190 S{BED_TEMP}
    # Use absolute coordinates
    G90
    # Reset the G-Code Z offset (adjust Z offset if needed)
    SET_GCODE_OFFSET Z=0.0
    
    # Home the printer
    G28
    # Perform adaptive bed mesh
    BED_MESH_CALIBRATE ADAPTIVE=1

    # Move nozzle
    G1 Z50 F240
    G1 X2.0 Y10 F3000

    # Set and wait for nozzle to reach temperature
    M109 S{EXTRUDER_TEMP}

    # Prime the nozzle
    G1 Z0.28 F240
    G92 E0
    G1 X2.0 Y140 E10 F1500
    G1 X2.3 Y140 F5000
    G92 E0 
    G1 X2.3 Y10 E10 F1200
    G92 E0
~~~
{% endraw %}

{% raw %}
~~~
[gcode_macro END_PRINT]
description: Routine to be executed at the end of a print
# Adopts end G-code settings from PrusaSlicer Ender-3 S1 preset
gcode:
    # Turn off bed, extruder, and fan
    M140 S0
    M104 S0
    M106 S0
    # Move nozzle away from print while retracting
    G91
    G1 X-2 Y-2 E-3 F300
    G90

    # Get printer variables
    {% set z_max= printer.configfile.settings.stepper_z.position_max|float %}
    {% set y_max = printer.configfile.settings.stepper_y.position_max|float %}
    {% set z_offset = printer.configfile.settings.bltouch.z_offset|float %}

    # Move print head up
    {% if params.MAX_LAYER_Z|float < z_max %}
        G1 Z{z_offset + ((params.MAX_LAYER_Z|float + 2), z_max)|min} F600
    {% endif %}

    # Present print and move print head out of the way
    G1 X5 Y{y_max * 0.85} F600

    # Move print head further up
    {% if params.MAX_LAYER_Z|float < (z_max - 10) %}
        G1 Z{z_offset + ((params.MAX_LAYER_Z|float + 70), (z_max - 10))|min} F600
    {% endif %}

    # Move print head further up
    {% if params.MAX_LAYER_Z|float < (z_max * 0.6) %}
        G1 Z{z_max * 0.6} F600
    {% endif %}

    G90
    # Disable steppers
    M84
~~~
{% endraw %}

Regardless of whether you use KlipperScreen, their [documentation](https://klipperscreen.readthedocs.io/en/latest/macros/#recommended) actually has some pretty good default macros which I also use. There is also a [Reddit post with some helpful macros](https://www.reddit.com/r/klippers/comments/vgfp6p/klipper_macros_i_use_them_on_an_ender_3_s1_ill/) for the Ender-3 S1 that could be better but I haven't compared them yet.

### Adaptive Layer Mesh

I don't feel the official Klipper documentation on this is very clear. To enable adaptive layer mesh, we need to enable object exclusion, which allows you to stop printing certain objects on the bed if they fail to save prints. [This guide from Obico](https://www.obico.io/blog/klipper-exclude-object/) details it, but the gist is as follows. At the top of the main `printer.cfg` file, add `[exclude_object]`. Then, in `moonraker.conf`, add the following section:

~~~
[file_manager]
enable_object_processing: True
~~~

Lastly, enable labeling objects in the slicer. In my `START_PRINT` macro I have already enabled adaptive mesh labeling with the `BED_MESH_CALIBRATE ADAPTIVE=1` command. You can configure the settings for adaptive mesh under the `[bed_mesh]` header.

After re-tramming the bed with the `SCREW_TILT` macro, I tune the PID for the nozzle and print bed, and then adjust the z-offset using [square patches](https://ellis3dp.com/Print-Tuning-Guide/articles/first_layer_squish.html).

### Tuning Pressure Advance

The [Pattern Method](https://ellis3dp.com/Print-Tuning-Guide/articles/pressure_linear_advance/pattern_method.html) on Ellis' Print Tuning Guide does a great job of determining the right pressure advance value for Klipper. Mine ended up being 0.45 for my Ender-3 S1. 

### Input Shaping

I use the [Klipper USB Accelerometer (KUSBA)](https://github.com/xbst/KUSBA) which I mount to the printer sing [this mount](https://www.printables.com/model/1056583-self-tapping-kusba-mount-for-ender-3-s1) that I designed. Then I follow the instructions from the GitHub repo on configuring the accelerometer, and run my tests with the `SHAPER_CALIBRATE` command (see [this page from the official Klipper documentation](https://www.klipper3d.org/Measuring_Resonances.html#input-shaper-auto-calibration)).

Remember to uncomment `[include adxlmcu.cfg]` when printing. Also, The KUSBA doesn't need to be connected to the Klipper host or be on the printer while printing, but I leave it on there anyway.

## Dealing With Fan Noise

Compared to a Prusa i3 MK3S+, the fan noise on the Ender-3 S1 is pretty annoying. On my sample in particular I noticed the hotend fan being louder than the PSU, which was contrary to what others online noted about the PSU and mainboard fans being louder. I added some GPL 105 oil (I don't have machine oil, which would probably be better suited for this purpose) to the bearing of the hotend fan and that decreased noise significantly.

In the future it might be a good idea to replace the PSU and mainboard fans as well, keeping in mind that all the fans on this printer are wired for 24V which limits the selection of replacement fans. The PSU and mainboard fans are apparently 60mm diameter and 15mm thick, so a pair of [Noctua NF-A6x15](https://noctua.at/en/nf-a6x15-flx) fans with a buck converter could do the trick.

## One Remaining Issue: Inconsistent Extrusion...Or a Lesson On Setting Expectations

This is my biggest gripe with the Ender-3 S1, or so I thought. Lines are always going to be present in FDM prints. I really should have read [this page](https://ellis3dp.com/Print-Tuning-Guide/articles/setting_expectations.html) about the limits of how good FDM printing can get, even after extensive tuning.

There was a video on YouTube which has now been removed which claimed that changing one of the extruder gears to MR93ZZ bearings could improve inconsistent extrusion. I tried this mod and did not see any improvement, however.

The real solution is to buy some matte filament or use fuzzy skin.

## Final Notes

All in all, the optimizations outlined here have really improved the quality and consistency of prints I get; in fact, my prints are better quality than that of a Prusa i3 MK3S+! It's kind of amazing how far a $70 printer can go.
