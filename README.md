Bradwii for Hubsan H107L v2.3. 
=======

Add support of flysky procotol

As the X4 board use the same rf chip as the TH9X tx (A7105), it can used with the FlySky protocol.

The rx code is mainly based on midelic from rcgroups, see http://www.rcgroups.com/forums/showthread.php?t=1921870

8 channels can be used.

You may need to erase full ship as the transmitter id is stored into flash.

Support for TH9X bind (first time):
- Switch the hubsan on (blue leds will flash)
- Switch on the TH9X in binding mode (press the module button while turning on), red leds will flash (wait for first TX frame)
- Switch off the transmitter, then switch on again, the hubsan shoud start calibration

Next time you just have to switch on the hubsan and the transmitter.

Support for TxAdapter_gke autobinding in flysky mode (https://github.com/gke/TxAdapter_gke):
 - Just switch on the hubsan and the transmitter.

Channel order : ROLL, PITCH, THROTTLE, YAW

As transmitter id is stored into flash, you don't need to bind every times, if any id is stored, it will wait for
any frame from the transmitter before the calibration start.

Rebind : even if your hubsan is already know the transmiter id, it possible to bind it with an other transmiter. At startup it alternatively
wait for know TX frame (red leds blinking) and wait for bind frame (blue leds blinking).


#### The H107L uses the following hardware
 * Nuvoton MINI54ZDE ARM Cortex-M0
 * AMICCOM A7105 2.5GHz transceiver
 * mCube MC3210 3-Axis Accelerometer
 * InvenSense MPU-3050 3-Axis MEMS Gyroscope


Manual accelerometer calibration:
 * Quadcopter must be on level surface
 * Quadcopter must be in "not armed" state
 * Throttle stick at minimum
 * Move roll stick 3 times left and right
 * LEDs blink in circular pattern to indicate calibration process. When finished, results are stored in data flash.

#### Development issues:
When burning a firmware with new PID control parameters, checkboxconfig or anything else from the usersettings struct make sure to erase the data flash.

Otherwise the firmware will continue to use the old data.

If the quadcopter has the newer Mini54ZDE (labeled HBS002) instead of the Mini54ZAN (sometimes labeled HBS001) make sure that all unused bits of CONFIG0 are set to 1.
It says so in the datasheet and if they are set to 0 witing to flash or config registers does not work as expected.

Still having reset issues using OpenOCD and the UM232H based SWD adapter. Using "cortex_m reset_config vectreset" first seemed to work but later it didn't.

The OpenOCD Mini51 flash driver did not work for me, so I modified it.
Config file and modified source [here](https://gist.github.com/TheLastMutt/d1c1948acaace7444c1c).

#### The H107L uses the following hardware
 * Nuvoton MINI54ZAN (or MINI54ZDE in later models) ARM Cortex-M0
 * AMICCOM A7105 2.5GHz transceiver
 * mCube MC3210 3-Axis Accelerometer
 * InvenSense MPU-3050 3-Axis MEMS Gyroscope

Credits
======
 * RCGROUPS!
 * Bradwii was originally coded by Brad Quick for AVR: https://github.com/bradquick/bradwii
 * Trollcop forked and ported to ARM STM32, untested: https://github.com/trollcop/bradwii
 * The Mini54ZAN ARM port to V202/JD385 was done by Victor: https://github.com/victzh/bradwii
 * The Hubsan X4 H107L port was done by Goebish: https://github.com/goebish/bradwii-X4
 * Extra work on the H107L port by TheLastMutt: https://github.com/TheLastMutt/bradwii-x4-gcc
