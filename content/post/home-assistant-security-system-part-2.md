---
title: Home Assistant Security System (Part 2)
tags: ["Home Assistant", "Raspberry Pi"]
date: 2019-04-11
draft: false
---

This is part two of the [Home Assistant Security System](https://aaronkjones.com/blog/home-assistant-security-system) guide. In this part, I show how I configured a dashboard display to complete my HA security system, to give visual feedback and, optionally, be used to manually arm the system. 

This alerts us if any door or window is opened while the family is not home and we can quickly glance at the display to see if anything is open before heading out the door.

This guide assumes you followed [part one](https://aaronkjones.com/blog/home-assistant-security-system).

<!--more-->

## Overview

- [Materials Used](#materials-used)
- [Display Setup](#display-setup)
- [Hassbian GUI (optional)](#hassbian-gui--optional-)
- [Enable VNC](#enable-vnc)
- [Chromium Kiosk Configuration](#chromium-kiosk-configuration)
  * [Install unclutter](#install-unclutter)
  * [Create the script](#create-the-script)
  * [Create the service](#create-the-service)
  * [Enable the service](#enable-the-service)
  * [Start the service](#start-the-service)
- [Configure Home Assistant](#configure-home-assistant)
- [Mounting the Display](#mounting-the-display)
- [Conclusion](#conclusion)

## Materials Used

* [Raspberry Pi LCD - 7" Touchscreen](https://amzn.to/2uqyCB1) - 64.95
* [Raspberry Pi 3 B+](https://amzn.to/2FAVQe7) - 39.95
* [SanDisk 32GB microSD Card](https://amzn.to/2HGCKp2) - 7.30

Total cost: ~ $110

## Display Setup

Connecting the display to a RPi is very simple. Use the included documentation. There is also a [video](https://www.youtube.com/watch?v=uXUjwk2-qx4) that can guide you through the process.


## Hassbian GUI (optional)

I wanted to use the same Raspberry Pi for Home Assistant and the dashboard display. To do so, I decided to use Hassbian OS. Since Hassbian does not include a GUI, I had to add one.

```bash
sudo apt-get update && apt-get -y upgrade
sudo apt-get -y install --no-install-recommends xserver-xorg xinit raspberrypi-ui-mods lxterminal gvfs
sudo apt-get -y install lightdm
sudo reboot
```

## Enable VNC

It's helpful to be able to VPN into the dashboard when you need to, such as initial log in.

- `sudo raspi-config`
- select `Interfacing Options > VNC` and enable

## Chromium Kiosk Configuration

Next, we want to have the dashboard load on boot.

### Install unclutter

```bash
sudo apt-get -y install unclutter
```

> Unclutter hides the mouse cursor when idle.

### Create the script

```bash
mkdir -p /home/pi/.bin/
vim /home/pi/.bin/hassdash.sh
```

```bash
xset s off
xset s noblank
xset -dpms
@unclutter -idle 0
sed -i 's/"exited_cleanly":false/"exited_cleanly":true/' /home/pi/.config/chromium/Default/Preferences
sed -i 's/"exit_type":"Crashed"/"exit_type":"Normal"/' /home/pi/.config/chromium/Default/Preferences
chromium-browser --disable-infobars --kiosk --force-device-scale-factor=0.90 https://www.home-assistant.io
```

### Create the service

```bash
vim /etc/systemd/system/hassdash.service
```

Insert this and save:

```bash
[Unit]
Description=Hass Dash
Wants=graphical.target
After=graphical.target

[Service]
Type=simple
User=pi
Environment=DISPLAY=:0
Environment=XAUTHORITY=/home/pi/.Xauthority
ExecStart=/bin/bash /home/pi/.bin/hassdash.sh
Restart=on-failure

[Install]
WantedBy=graphical.target
```

### Enable the service

```bash
systemctl enable hassdash
```

### Start the service

```bash
systemctl start hassdash
```

> You can check if the service started succesfully with `systemctl status hassdash`

## Configure Home Assistant

I configured Lovelace to display each Z-wave sensor status.

![](https://i.imgur.com/vZz6pWj.png)

```bash
views:
  - cards:
      - type: entities
        entities:
          - entity: binary_sensor.ecolink_door_window_sensor_sensor
          - entity: binary_sensor.ecolink_door_window_sensor_sensor_11
          - entity: binary_sensor.ecolink_door_window_sensor_sensor_3
          - entity: binary_sensor.ecolink_door_window_sensor_sensor_4
          - entity: binary_sensor.ecolink_door_window_sensor_sensor_5
          - entity: binary_sensor.ecolink_door_window_sensor_sensor_6
          - entity: binary_sensor.ecolink_door_window_sensor_sensor_2
    panel: false
    title: Sensors
    badges: []
  - cards:
      - type: alarm-panel
        states:
          - arm_home
          - arm_away
        entity: alarm_control_panel.home_alarm
    title: Alarm
    badges: []
```

You could also configure it to act as a manual alarm panel, but I could not get it to fit on a 7" display.

![](https://i.imgur.com/m46OYFI.jpg)

I plan on trying to use [lovelace-card-modder](https://github.com/thomasloven/lovelace-card-modder) and [Kiosk mode](https://gist.github.com/ciotlosm/1f09b330aa5bd5ea87b59f33609cc931) to try to get that looking better.

## Mounting the Display

![](https://i.imgur.com/iABCVUX.jpg)

This part is tricky since each person's situation is different. In my case, there was an existing alarm panel that was installed when the house was built (also the reason why the paint is mismatched in the picture above). I had to make the hole larger and I will share a couple takeaways from my experience.

- **Watch out for electrical lines**. Use a stud finder with wire detection and consulting an electrician.

- Using a level may not be the best way to go. If I had cut the hole perfectly level, it would have looked unlevel compared to the edge above it. This is hard to explain, but if you have another edge close by such as a shelf, make sure they line up evenly.

- Making a hole slightly smaller than the display back will allow you to *pop* the display snuggly in place. It is almost difficult to remove, so I don't have to worry about it falling out. However, drywall is not very durable around the edges, so time will tell if inserting and removing the display will wear the hole larger.

- The seal on the back of the display is not perfect. Drywall dust got in behind the glass, but I was able to blow it out with compressed air. I may seal the display with silicone in the future.

## Conclusion

Let me know if you have questions and also check out these wonderful sources for more information.

- [Home Assistant Documentation](https://www.home-assistant.io/docs/)
- [/r/homeassistant](http://reddit.com/r/homeassistant/)

<div style="text-align: left">
<a href="https://www.buymeacoffee.com/gj3bSy8IT" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Me A Coffee" style="height: auto !important;width: auto !important;" ></a>
</div>