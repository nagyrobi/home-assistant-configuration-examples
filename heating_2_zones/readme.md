# Smart heating control for Home Assistant - 2 zones version

I've put together a heating control system for my house using Home Assistant. This completely replaces any hardware thermostat, all decisions are done in software by the system. Since it took quite a lot of work, I thought I'd share it for anyone trying to achieve something similar. It all started from  the solution on [this page](https://www.earth.li/~noodles/blog/2018/10/heating-automation.html) but I wanted a more flexible approach - however it was excellent to set the base to start from. The goal was also to have all configuration available in Lovelace GUI and no need to edit config files in order to change operational parameters.

![Lovelace UI](https://raw.githubusercontent.com/nagyrobi/home-assistant-configuration-examples/main/heating/heating_lovelace_small.jpg)

## Features
- 2 zones (composed of 1 heater main pump + 2 separate zone pumps/valves)
- split the days in 4 time segments, separately for workdays and free days (morning, daytime, evening, nighttime)
- schedule different temperature levels for each time segment, separately for each zone
- separate thermostat for each zone
- provide an override system for holiday/party mode (a predefined period until a different temperature level can be set, which when ends, returns to the schedule)
- automatic home/away mode switching based on presence detection
- automatic on/off switching based on outside temperature, weather forecast temperature, door/window opening sensors

## Configuration layout
I have my Home Assistant config set up using [splitting method](https://www.home-assistant.io/docs/configuration/splitting_configuration/) which I recommend anyone who wants to have a more flexible way of setting up things. Configuration items can be separated in sub-folders based on integration types, and they can be further split in multiple yaml files, which is great because if any config has to be excluded or temporarily taken out, all it takes is to rename the extension of the filename to something else and restart Home Assistant. I share my heating control solution in this layout - I've only left in the configuration items related to it. This way most of the things are self-explanatory.

## Requirements
- MQTT switches to control the heater pumps/valves (something like [Sonoff Mini](https://templates.blakadder.com/sonoff_mini.html) running [Tasmota](https://tasmota.github.io/docs/) firmware)
- MQTT temperature sensors in each room, and outdoors (for indoors I used [Geeklink IR Bridge](https://templates.blakadder.com/geeklink-GK01.html) with an extra [DS18B20](https://tasmota.github.io/docs/DS18x20/) soldered on an punched out on the side of it - extra benefit that it can also control AC devices in the room but that's a different story; for outdoors the same sensor on a [Sonoff Dual](https://templates.blakadder.com/sonoff_dual_R2.html) switching my garden lights; both running Tasmota)
- weather forecast service [set up in Home Assistant](https://www.home-assistant.io/integrations/openweathermap/) 
- a device tracker integration to monitor for [presence detection](https://www.home-assistant.io/integrations/snmp/#precense-detection)
- [average](https://github.com/Limych/ha-average) custom component installed
- optionally [simple thermostat card](https://github.com/nervetattoo/simple-thermostat) for the main thermostat in the frontend
- optionally some door/window opening sensors, configured in a group of binary sensors, to detect when any of them is open

## How it works from the user's perspective

Normally the user doesn't have to deal with the main thermostat because it's operation is fully automatic.

Each day is split into morning, daytime, evening, nighttime periods, the starting times for these can be set separately for workdays and free days. A [workday binary sensor](https://www.home-assistant.io/integrations/workday/) helps with this, because not only Saturdays and Sundays can be free, but also national holidays, vacations etc. configuring this properly makes the system aware of these. So all one has to do is to set the desired temperature levels for these periods and turn on the `Heating timer enable` switch. As time passes, the system will automatically adjust the thermostat's target temperature to the level corresponding to the time. With `Heating timer enable` turned off, this will not happen. Whenever the switch is turned on, the system immediately checks for the current time and adjusts the temperature level to the corresponding time period. If a temperature value is modified in the scheduler, the thermostat value will be updated accordingly.

Timer can be overridden with a separate `Heating override` switch. It can be specified `Until` when (with both date and time) to hold the override and return to the timer mode. This can be useful to have a fixed temperature level set regardless of the timing, and have it finish automatically after a deadline. Note that override applies to home/away and weather on/off automations too, so with this, away mode will not trigger, and the system will continue to operate regardless of the outdoor temperatures.

The system will automatically switch to away mode when the presence detection reports this. The climate configuration has a temperature value set for away mode, and the schedules don't run while away. 
Monitoring both outside temperature and also weather forecast temperature helps turning off the system when it's warm enough in the environment, thus saving precious energy.

## Lovelace config

```yaml
type: horizontal-stack
cards:
  - type: entities
    entities:
      - entity: input_number.heating_temp1_morning
      - entity: input_number.heating_temp1_day
      - entity: input_number.heating_temp1_evening
      - entity: input_number.heating_temp1_night
    title: Zone 1 temperatures
    show_header_toggle: false
  - type: entities
    entities:
      - entity: input_number.heating_temp2_morning
      - entity: input_number.heating_temp2_day
      - entity: input_number.heating_temp2_evening
      - entity: input_number.heating_temp2_night
    title: Zone 2 temperatures
    show_header_toggle: false
  - type: entities
    entities:
      - entity: input_boolean.heating_timer_switch
      - entity: input_boolean.heating_override_switch
      - entity: input_number.heating_temp1_override
      - entity: input_number.heating_temp2_override
      - entity: input_datetime.heating_override_until
    title: Override
    show_header_toggle: false
  - type: entities
    title: Workday timings
    entities:
      - entity: input_datetime.heating_weekday_morning_start
      - entity: input_datetime.heating_weekday_day_start
      - entity: input_datetime.heating_weekday_evening_start
      - entity: input_datetime.heating_weekday_night_start
    show_header_toggle: false
    state_color: false
  - type: entities
    title: Free day timings
    entities:
      - entity: input_datetime.heating_weekend_morning_start
      - entity: input_datetime.heating_weekend_day_start
      - entity: input_datetime.heating_weekend_evening_start
      - entity: input_datetime.heating_weekend_night_start
    show_header_toggle: false

```

Forum discussion: https://community.home-assistant.io/t/smart-heating-control-with-home-assistant/237966


Grab the source configs from here:
https://github.com/nagyrobi/home-assistant-configuration-examples/tree/main/heating
