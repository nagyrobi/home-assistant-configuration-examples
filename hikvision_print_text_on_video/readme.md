# Print custom text on Hikvision camera video

With this shell command and automation it becomes possible to print custom text on the live video of the newer Hikvision cameras (models supporting ISAPI).
Note that the shell command is parametrized, so that only one is needed for all the cameras (if multiple are present of similar type). Camera IP address, message text and display enable/disable has to be specified in the service call.

Two examples included, one which updates outdoor temperatures, and one which displays a message on the video when somebody pushes the doorbell button.

![pic|600x122](https://community-assets.home-assistant.io/original/3X/4/3/43cecd69911e4619e2776ab45f0d3794e597ca55.png) 

Tested and working with DS-2CD2T47G1-L firmware v5.6.5.

Note that there can be even 4 text fields supported, just look in the OSD settings of the camera. To access them, the shell command can be modified around the `<id>1</id>` xml tag, by replacing 1 with the number of the required field. Haven't tested this myself though.

https://community.home-assistant.io/t/print-custom-text-on-hikvision-camera-video/276009
