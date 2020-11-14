# Control the white ColorVU LED on Hikvision cameras

With this command line switch it's possible to turn on and off the super-bright white LED on the ColorVU Hikvision cameras (toggles the checkbox corresponding to "System Settings > External Device > Enable Supplement Light" in the web UI of the camera). One would want to do this temporarily because the LED is so bright that it may be annoying to people spending some time on the area watched by the camera. There's an automation included which turns it back on after 2 hours, just in case someone forgets to do do this.

Tested and working with DS-2CD2T47G1-L firmare v5.6.5.

https://community.home-assistant.io/t/control-the-white-colorvu-led-on-hikvision-cameras/245092
