# Print custom text on Hikvision camera video

With this shell command and automation it becomes possible to print custom text on the live video of the newer Hikvision cameras (models supporting ISAPI integration).
Note that the shell command is parametrized, so that only one is needed for all the cameras (if multiple are present of similar type). Camera IP address, message text and display enable/disable has to be specified in the service call.

Two examples included, one which updates outdoor temperatures, and one which displays a message on the video when somebody pushes the doorbell button.

Tested and working with DS-2CD2T47G1-L firmware v5.6.5.

https://community.home-assistant.io/t/print-custom-text-on-hikvision-camera-video/276009
