# Print custom text on Hikvision camera video

With this shell command and automation it becomes possible to print custom text on the live video of the newer Hikvision cameras (models supporting ISAPI).
Note that the shell command is parametrized, so that only one is needed for all the cameras (if multiple are present of similar type). Camera IP address, message text and display enable/disable has to be specified in the service call.

Two examples included, one which updates outdoor temperatures, and one which displays a message on the video when somebody pushes the doorbell button.

![pic|600x122](https://community-assets.home-assistant.io/original/3X/4/3/43cecd69911e4619e2776ab45f0d3794e597ca55.png) 

Note that there can be even 4 text fields supported, just look in the OSD settings of the camera.
![pic2](https://community-assets.home-assistant.io/original/3X/3/0/30e23537e9d229b1e6d1d56bc243cfbd928f3366.png)

To access them, use the `camera_text_id` parameter (integers from 1 to 4). Each field can be positioned on screen, use `camera_text_pos_x` and `camera_text_pos_y` for that (coordinates originate from the bottom left corner). To remove text from screen switch it off with `camera_text_enabled` parameter set to `false` (important: can be `true` or `false`, and has to be lowercase and in quotes!)

Tested and working with DS-2CD2T47G1-L firmware v5.6.5. A field can have max 44 characters.

https://community.home-assistant.io/t/print-custom-text-on-hikvision-camera-video/276009
