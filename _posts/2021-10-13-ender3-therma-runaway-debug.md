---
layout: post
title: "Solving Thermal Runaway on my 3D Printer "
comments: false
description: "Debugging and solving ender 3 thermal runaway"
keywords: "3dprinting, electronics, debug"
published: false
visible: 0
---

A month ago, finally got myself my first 3D printer, a modest Ender 3. Not the point of this post,
but a very very nice cheap machine, after a quick 30min assembly without any troubles, I had the
printer ready and working almost flawlessly, after one or two calibration print, very nice results.

# The problem
After a week of honeymoon, printing random stuff from thingiverse, and inserts for my board game
collection, one day I walk to my printer halted with a error on the screen:
**Thermal runaway PRINTER HALTED**. Ok, that's bad. This being my first 3d printer, I go and google
wtf is a thermal runaway, after a few minutes I have a superficial idea.

Basically, The printers is trying to heat, turning the heat element on, but it don't see a change of
temperature on the sensors, this can be that the heating element it's not working, or worse, the
sensor is faulty and the machine keep heating, causing a potential fire. As a safety feature, the
printer stops, but this problem can happen to the nozzle heater or the bed heater and the original
ender 3 firmware don't give me this info.

# Installing marlin
During the first week with the printer, I didn't thinker with it, it was working and I was having,
and being a cheap hobbyist machine, sooner or later I'll end up modifying it, well, the time came.

With the original firmware not giving me the info on what caused the thermal runaway, my next step
was installing Marlin, the process is pretty straight forward if you have some experience with
embedded systems, clone the repository, use the config files for the ender, build with platformio.

New firmware installed, I did some more prints, and catch the problem again, this time with a
valuable information: **E1 Heating failed**. Ok, now I know that the problem it's on the hotend.

# First round of debug
Another quick search, I gather on the internet the main causes of thermal runaway

* Thermistor too tight on the heating block
* Bad connections on the thermistor or heating element
* Bad thermistor
* Bad heating element
* Bad power supply
* Faulty control board

Verified all of the items, monitored the voltage during print, measured the resistance of the
heating element and thermistor, did all the connections again, the only issue that I could not
eliminate was the faulty main board, as this is my only printer.

Did some more prints, and the error happened again...

# Installing octoprint and gathering more data
Installed octoprint with docker on my laptop, and started monitoring my prints. Octoprint give us
a nice graph of temperatures, and was a invaluable tool to debug this.

Did a couple of prints, and started to notice a pattern, the prints normally start with the
temperatures pretty stable, like so:
![Start of print](/assets/images/ender3_thermal_runaway/bad_thermistor_start.png)

But after the first layers, when the printer stop doing the skin slowly and start moving pretty
erratic, the temperature oscillates a lot.
![MId of print](/assets/images/ender3_thermal_runaway/bad_thermistor_mid.png)

![Console log](/assets/images/ender3_thermal_runaway/thermal_runaway_console_log.png)
![Thermal catch](/assets/images/ender3_thermal_runaway/thermal_runaway_graph.png)
![Gambiarra](/assets/images/ender3_thermal_runaway/gambiarra.jpeg)
