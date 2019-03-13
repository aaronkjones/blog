+++
title = "Home Assistant Security System"
date = "2019-03-13T18:30:40-07:00"
draft = true

+++

## Introduction

This is a high level guide that will get you up and running with Home Assistant as a basic door, window, and garage door security system.

### Topics Covered

- Installing Home Assistant
- Configuring Z-Wave
- Adding Z-Wave Sensors
- Configuring Notifications
- Creating a Notification
- Test Notification

## Materials Used

- [Raspberry Pi 3 B+](https://www.amazon.com/ELEMENT-Element14-Raspberry-Pi-Motherboard/dp/B07BDR5PDW/ref=sr_1_4?keywords=raspberry+pi&qid=1552499073&s=gateway&sr=8-4) $40
- [GoControl HUSBZB-1 Z-Wave USB Antenna](https://www.amazon.com/GoControl-CECOMINOD016164-HUSBZB-1-USB-Hub/dp/B01GJ826F8/ref=sr_1_5?keywords=z-wave+usb&qid=1552499004&s=gateway&sr=8-5) $40
- [Ecolink Z-Wave Door/Window Sensor](https://www.amazon.com/Z-Wave-Plated-Reliability-Garage-TILT-ZWAVE2-5-ECO/dp/B01MRZB0NT/ref=sr_1_fkmrnull_3?keywords=ecolink+garage+door+tilt&qid=1552499121&s=gateway&sr=8-3-fkmrnull) x 6 $35/ea
- [Ecolink Z-Wave Garage Door Sensor](https://www.amazon.com/Z-Wave-Plated-Reliability-Garage-TILT-ZWAVE2-5-ECO/dp/B01MRZB0NT/ref=pd_lpo_vtph_60_bs_lp_t_1?_encoding=UTF8&psc=1&refRID=SYE35K9HVQ98S8FGM56F) $30

Total cost: $320

## Installing Home Assistant

To install HA, see the [Getting Started](https://www.home-assistant.io/getting-started/) guide. 

Also, take a look at the [alternative installation methods](https://www.home-assistant.io/docs/installation/) of installing HA. 

Personally I chose Hassbian because I wanted a more traditional Linux environment to work in. Hass.io makes installation very simple.

## Configuring Z-Wave

[HA Z-Wave Documentation](https://www.home-assistant.io/docs/z-wave/)

Firstly, we will need to configure HA to enable Z-Wave devices. Configuration of HA is handled in the file, `/home/homeassistant/.homeassistant/configuration.yaml` on the Raspberry Pi. Components are enabled by including them in this file.

To enable Z-Wave add:

```
zwave:
  usb_path: /dev/ttyACM0
  device_config: !include zwave_device_config.yaml
``` 
> If /dev/ttyACM0 is not the path to your Z-Wave USB device, see [Platform specific instructions](https://www.home-assistant.io/docs/z-wave/installation/#platform-specific-instructions)

Once you add a component you must restart HA to load it.

`sudo systemctl restart home-assistant@homeassistant.service`

Once HA has restarted you should see "Z-Wave" under Configuration.

> If you don't see Z-Wave under Configuration see [My component does not show up](https://www.home-assistant.io/docs/configuration/troubleshooting/#my-component-does-not-show-up) for troubleshooting.

## Adding Z-Wave Sensors

[Adding devices documentation](https://www.home-assistant.io/docs/z-wave/adding/)

The basic workflow goes like this:

1. Go to Z-Wave in Home Assistant
2. With the plastic tab still inserted in the sensor or the battery removed, click the `Add Node` button
3. Bring the device close to the Raspberry Pi/Z-Wave USB Antenna
4. Pull the plastic tab on the sensor to put it in inclusion mode.
4. Install the sensor on a door/window
5. Click `heal network` in the Z-Wave configuration panel

Your device should now be visible in HA

## Configuring Notifications

There are MANY [notification](https://www.home-assistant.io/components/#search/notification) components available in HA. Personally, I am only using the [iOS notification](https://www.home-assistant.io/docs/ecosystem/ios/)

To enable a notification component, add it to the configuration file (`/home/homeassistant/.homeassistant/configuration.yaml).

For example, to enable iOS notifications, simply add:

`ios:`

## Creating a Notification

You can get creative on notification automations, but starting out I wanted to be notified if any sensor was tripped. To do so, we will need to create an automation. Automations are easily configured in the HA configuration panel. You can also edit the `/home/homeassistant/.homeassistant/automations.yaml` file directly.

I created an automation that notifies my phone when any sensor is opened (state changed from off to on).

```
- id: '1552497003983'
  alias: Door/Window Opened
  trigger:
  - entity_id: binary_sensor.ecolink_door_window_sensor_sensor
    from: 'off'
    platform: state
    to: 'on'
  - entity_id: binary_sensor.ecolink_door_window_sensor_sensor_2
    from: 'off'
    platform: state
    to: 'on'
  - entity_id: binary_sensor.ecolink_door_window_sensor_sensor_3
    from: 'off'
    platform: state
    to: 'on'
  - entity_id: binary_sensor.ecolink_door_window_sensor_sensor_4
    from: 'off'
    platform: state
    to: 'on'
  - entity_id: binary_sensor.ecolink_door_window_sensor_sensor_5
    from: 'off'
    platform: state
    to: 'on'
  - entity_id: binary_sensor.ecolink_door_window_sensor_sensor_6
    from: 'off'
    platform: state
    to: 'on'
  - entity_id: binary_sensor.ecolink_garage_door_tilt_sensor_sensor
    from: 'off'
    platform: state
    to: 'on'
  condition: []
  action:
  - data:
      message: A door or window was opened
      title: Alert
    service: notify.ios_chosin
```

Alternatively, you can go to the `Automations` section of the configuration and select each sensor under trigger, set their state from off to on. For action select `notifiy.<your device>` and insert a JSON formatted payload.

## Test Notification

In order to make sure the alert will function properly, we can invoke a test scenario. To do so, go to the `Services` configuration of HA. 

![Services](https://i.imgur.com/8RD5NXE.png)

Once there, in the Service field start typing `notify` and you will see the different notification options. If you don't see any, then the notifiy component did not load correctly (did you restart HA after loading it?).

In the `Service Data` field, enter:

```
{
"title":"Test",
"message":"Test message"
}
```

When you press `Call Service` you should receive a notification on your iPhone or however you configured to be notified.

Now in order to make sure the automation tigger will notify you, we can do another test. 

- Go to the `States` section.

![States](https://i.imgur.com/oP0CbUw.png)

- Select a sensor to use for testing
- Set state to off

Once you click `Set State`, the automation should trigger, and you should receive a notification

![](https://i.imgur.com/aArax1Bl.jpg)