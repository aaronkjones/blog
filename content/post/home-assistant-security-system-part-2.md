---
title: "Home Assistant Security System (Part 2)"
date: 2019-03-25T12:48:33+01:00
draft: true
---

## Introduction

This is part two of the [Home Assistant Security System](https://aaronkjones.com/blog/home-assistant-security-system) guide. In this part, I show how I configured a dashboard display to complete my HA security system, to give visual feedback and, optionally, be used to manually arm the system. This guide assumes you followed [part one](https://aaronkjones.com/blog/home-assistant-security-system).

## Overview



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

```
sudo raspi-config
```
Select `Interfacing Options > VNC`

## Chromium Kiosk Configuration

Next, we want to have the dashboard load on boot.

### Install unclutter (hides mouse cursor when idle)

```bash
sudo apt-get -y install unclutter
```

### Create Hassdash script

```
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

## Home Assistant Configuration

## Mounting the Display

This part is tricky since each person's situation is different. In my case, there was an existing alarm panel that was installed when the house was built. I had to make the hole larger. I will share a couple takeaways from my experience.

**Watch out for electrical lines**. While I was making the hole larger I noticed there were power lines for a light switch near by. Since there was an existing hole I could see inside. If you are cutting a new hole, I recommend using a stud finder with wire detection and consulting an electrician.

Using a level may not be the best way to go. There is an edge above where my panel is and if I had used a level when cutting a hole, it would not be perfectly parallel to that edge. Instead, I used a measuring tape from that edge to where I wanted the panel.

Making a hole slightly smaller than the display back will allow you to *pop* the display snuggly in place. It is almost difficult to remove, so I don't have to worry about it falling out.

The seal on the back of the display is not perfect. Drywall dust go in behind the glass, but I was able to blow it out with compressed air. I may seal the display with silicone in the future.