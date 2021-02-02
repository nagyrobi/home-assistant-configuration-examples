# Control the white ColorVU LED on Hikvision cameras

With this command line switch it's possible to turn on and off the super-bright white LED on the ColorVU Hikvision cameras (toggles the checkbox corresponding to "System Settings > External Device > Enable Supplement Light" in the web UI of the camera). One would want to do this temporarily because the LED is so bright that it may be annoying to people spending some time on the area watched by the camera. There's an automation included which turns it back on after 2 hours, just in case someone forgets to do do this.

Tested and working with DS-2CD2T47G1-L firmware v5.6.5.

I’ve added to the example a shell_command and an automation to update every day the LED operation schedule on the camera, for those not satisfied by the automatic mode.

It turned out in my case that the LED wouldn’t turn on by itself at night on certain cameras. It seems the built-in automation is disturbed by the LEDs of the others. But thankfully there is a different way to use it, scheduled. With this, mode is switched in the camera from “Auto” to “Scheduled” and it turns ON automatically 1 hour after sunset, and OFF 30mins before sunrise. The ON/OFF times in the camera are updated every day by a template in Home Assistant.
Note that the ON/OFF swicth is more complicated in schedule mode, as the camera requires the whole XML (including the schedule time) to toggle the state of the checkbox... and since the command_line switch doesn't accept templates, the shell_command has to be run again after switching to re-update the dummy values set by the switch. Quite dirty but usable.

https://community.home-assistant.io/t/control-the-white-colorvu-led-on-hikvision-cameras/245092
