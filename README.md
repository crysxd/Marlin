# What does this fork do?

![Nightly](https://github.com/crysxd/Marlin/workflows/Nightly/badge.svg)

This fork of the Marlin project provides nightly firmware builds for the BigTreeTech SKR mini E3 v1.2 by taking the daily changes of the [Marlin project](https://github.com/MarlinFirmware/Marlin) and building thermal with BigTreeTech's default configuration found [here](https://github.com/bigtreetech/BIGTREETECH-SKR-mini-E3).

Daily build [firmware.bin](https://github.com/crysxd/Marlin/releases) can be found here.

Jjust download the latest firmware.bin and copy it to your SD card. The update is flashed with the next reboot of the printer. It's adviced to save the FIRMWARE.CUR file before flashing. You can restore the previous firmware by renaming this file to firmware.bin and flashing it again.

Two kind of releases are provided:
 - 2.0.x builds are less frequently updated and can be considered "more mature"
 - bugfix-2.0.x are changing almost daily and contain the latest fixes and optimizations

Personally, I update to the latest bugifx-2.0.x every couple weeks.

Please be aware that the firmware.bin files and guidance provided here are created automatically and are usually not tested (besides the one I download myself and use on my printer). This means I can give no guarantee about the quality or functionality.

## BLTouch builds (Coming soon)

**READ THIS BEFORE DOING ANYTHING!**

The BLTouch builds are configured for following wiring:
![img](https://camo.githubusercontent.com/56b380ce77969d6bf7e4d627094676b2625dc876/68747470733a2f2f692e696d6775722e636f6d2f42524a516e65392e6a7067)

This means that you replace your Z-Endstop with the BLTouch. The BLTouch is then used for
Z axis homing instead of the endstop. This has many advantages, e.g. you do not need to
adjust your Z-Endstop height when adding a glass build plate. Please remove the Z-endstop from your printer to prevent it from colliding with the Z axis.

After flashing the firmware, you need to tell your printer where the BLTouch is relative to your
nozzle. This will be different depending on your fan duct and or BLTouch holder. The firmware is
configured for X0 Y0 Z6. This means after homing, your printer will assume it needs to move the nozzle
up 6mm to print on your surface (it will print mid-air). This is intentionally to prevent damage to your printer in case you miss the next step ;)

To adjust the nozzle offset, use the M851 gcode command. You can find the X and Y values usually
where you got your BLTouch mount from. The Z value must be calibrated. To set everything up you need to use
a software like Pronterface or Octoprint to send gcode commands to your printer. If you don't have this or don't want to do this, you can also create a text file, add the commands to it (one per line) and save it as
`something.gcode` and then "print" it. Perform following steps:

- Copy the firmware.bin to your SD card, insert it and restart the printer
- The BLTouch should deploy a couple of times when the printer is booting. A little blue LED and a big red one should be lit up afterwards. **If this is not the case (might be different for some clones), double check your wiring and proceed with high caution when homing!**
- Move the Z axis high up so you have time to react when it homes the Z axis
- Home all axis
- When the Z axis moves down, the BLTouch probe should deploy
- Hold your finger under the probe pin and let it touch your hand. It should retract.
- The axis will move up, the BLTouch will deploy again. Let it touch your finger again
- **If the BLTouch does not deploy or the axis does not stop moving down, immediately cut the power to the printer!**
- If everything worked, home the printer again
- Open Pronterface or Octoprint and run `M851 X? Y? Z6` and replace the ? with your X and Y values, e.g `M851 X-2.4 Y11.4 Z6`
- Go to "Main Menu > Motion > Move Axis > Soft Endstops" to turn them off
- Place a piece of paper under the nozzle
- Go to "Main Menu > Motion > Move Axis > Move Z > 0.025mm" and move the nozzle down until you feel a little resistance on the paper while moving it
- Note down the number on the screen.
- Subtract the number from 6, e.g. if your screen read -8.54 then you need to do 6-8.54=**-2.54**
- Open Pronterface or Octoprint and run `M851 Z?` and replace the ? with your calculated value.
- Open Pronterface or Octoprint and run `M500` to save your settings to EEPROM and make the persist restarts.
- Home your printer again
- Go to "Main Menu > Motion > Move Axis > Move Z > 0.025mm" and move it down to 0. The nozzle should now just hover over the bed.
- Open Pronterface or Octoprint and run `M48` to run a test of the BLTouch. The terminal will show the results on how precise it works.
- Open Pronterface or Octoprint and run `G29` to run a bed leveling procedure.
- Add `G29` to the end of your start gcode in your slicer to run the bed leveling procedure before every print. Alternatively, you can open Pronterface or Octoprint and run `M500` to save the results of your `G29`. You do not need to run the bed leveling routine now before every print, but if your bed changes a little the nozzle will be off.






# Marlin 3D Printer Firmware

![GitHub](https://img.shields.io/github/license/marlinfirmware/marlin.svg)
![GitHub contributors](https://img.shields.io/github/contributors/marlinfirmware/marlin.svg)
![GitHub Release Date](https://img.shields.io/github/release-date/marlinfirmware/marlin.svg)
[![Build Status](https://github.com/MarlinFirmware/Marlin/workflows/CI/badge.svg?branch=bugfix-2.0.x)](https://github.com/MarlinFirmware/Marlin/actions)

<img align="right" width=175 src="buildroot/share/pixmaps/logo/marlin-250.png" />

Additional documentation can be found at the [Marlin Home Page](https://marlinfw.org/).
Please test this firmware and let us know if it misbehaves in any way. Volunteers are standing by!

## Marlin 2.0

Marlin 2.0 takes this popular RepRap firmware to the next level by adding support for much faster 32-bit and ARM-based boards while improving support for 8-bit AVR boards. Read about Marlin's decision to use a "Hardware Abstraction Layer" below.

Download earlier versions of Marlin on the [Releases page](https://github.com/MarlinFirmware/Marlin/releases).

## Building Marlin 2.0

To build Marlin 2.0 you'll need [Arduino IDE 1.8.8 or newer](https://www.arduino.cc/en/main/software) or [PlatformIO](http://docs.platformio.org/en/latest/ide.html#platformio-ide). Detailed build and install instructions are posted at:

  - [Installing Marlin (Arduino)](http://marlinfw.org/docs/basics/install_arduino.html)
  - [Installing Marlin (VSCode)](http://marlinfw.org/docs/basics/install_platformio_vscode.html).

### Supported Platforms

  Platform|MCU|Example Boards
  --------|---|-------
  [Arduino AVR](https://www.arduino.cc/)|ATmega|RAMPS, Melzi, RAMBo
  [Teensy++ 2.0](http://www.microchip.com/wwwproducts/en/AT90USB1286)|AT90USB1286|Printrboard
  [Arduino Due](https://www.arduino.cc/en/Guide/ArduinoDue)|SAM3X8E|RAMPS-FD, RADDS, RAMPS4DUE
  [LPC1768](http://www.nxp.com/products/microcontrollers-and-processors/arm-based-processors-and-mcus/lpc-cortex-m-mcus/lpc1700-cortex-m3/512kb-flash-64kb-sram-ethernet-usb-lqfp100-package:LPC1768FBD100)|ARM® Cortex-M3|MKS SBASE, Re-ARM, Selena Compact
  [LPC1769](https://www.nxp.com/products/processors-and-microcontrollers/arm-microcontrollers/general-purpose-mcus/lpc1700-cortex-m3/512kb-flash-64kb-sram-ethernet-usb-lqfp100-package:LPC1769FBD100)|ARM® Cortex-M3|Smoothieboard, Azteeg X5 mini, TH3D EZBoard
  [STM32F103](https://www.st.com/en/microcontrollers-microprocessors/stm32f103.html)|ARM® Cortex-M3|Malyan M200, GTM32 Pro, MKS Robin, BTT SKR Mini
  [STM32F401](https://www.st.com/en/microcontrollers-microprocessors/stm32f401.html)|ARM® Cortex-M4|ARMED, Rumba32, SKR Pro, Lerdge, FYSETC S6
  [STM32F7x6](https://www.st.com/en/microcontrollers-microprocessors/stm32f7x6.html)|ARM® Cortex-M7|The Borg, RemRam V1
  [SAMD51P20A](https://www.adafruit.com/product/4064)|ARM® Cortex-M4|Adafruit Grand Central M4
  [Teensy 3.5](https://www.pjrc.com/store/teensy35.html)|ARM® Cortex-M4|
  [Teensy 3.6](https://www.pjrc.com/store/teensy36.html)|ARM® Cortex-M4|
  [Teensy 4.0](https://www.pjrc.com/store/teensy40.html)|ARM® Cortex-M7|
  [Teensy 4.1](https://www.pjrc.com/store/teensy41.html)|ARM® Cortex-M7|

## Submitting Changes

- Submit **Bug Fixes** as Pull Requests to the ([bugfix-2.0.x](https://github.com/MarlinFirmware/Marlin/tree/bugfix-2.0.x)) branch.
- Follow the [Coding Standards](http://marlinfw.org/docs/development/coding_standards.html) to gain points with the maintainers.
- Please submit your questions and concerns to the [Issue Queue](https://github.com/MarlinFirmware/Marlin/issues).

## Marlin Support

For best results getting help with configuration and troubleshooting, please use the following resources:

- [Marlin Documentation](http://marlinfw.org) - Official Marlin documentation
- [Marlin Discord](https://discord.gg/n5NJ59y) - Discuss issues with Marlin users and developers
- Facebook Group ["Marlin Firmware"](https://www.facebook.com/groups/1049718498464482/)
- RepRap.org [Marlin Forum](http://forums.reprap.org/list.php?415)
- [Tom's 3D Forums](https://forum.toms3d.org/)
- Facebook Group ["Marlin Firmware for 3D Printers"](https://www.facebook.com/groups/3Dtechtalk/)
- [Marlin Configuration](https://www.youtube.com/results?search_query=marlin+configuration) on YouTube

## Credits

The current Marlin dev team consists of:

 - Scott Lahteine [[@thinkyhead](https://github.com/thinkyhead)] - USA &nbsp; [Donate](http://www.thinkyhead.com/donate-to-marlin)
 - Roxanne Neufeld [[@Roxy-3D](https://github.com/Roxy-3D)] - USA
 - Chris Pepper [[@p3p](https://github.com/p3p)] - UK
 - Bob Kuhn [[@Bob-the-Kuhn](https://github.com/Bob-the-Kuhn)] - USA
 - Erik van der Zalm [[@ErikZalm](https://github.com/ErikZalm)] - Netherlands &nbsp; [![Flattr Erik](https://api.flattr.com/button/flattr-badge-large.png)](https://flattr.com/submit/auto?user_id=ErikZalm&url=https://github.com/MarlinFirmware/Marlin&title=Marlin&language=&tags=github&category=software)

## License

Marlin is published under the [GPL license](/LICENSE) because we believe in open development. The GPL comes with both rights and obligations. Whether you use Marlin firmware as the driver for your open or closed-source product, you must keep Marlin open, and you must provide your compatible Marlin source code to end users upon request. The most straightforward way to comply with the Marlin license is to make a fork of Marlin on Github, perform your modifications, and direct users to your modified fork.

While we can't prevent the use of this code in products (3D printers, CNC, etc.) that are closed source or crippled by a patent, we would prefer that you choose another firmware or, better yet, make your own.
