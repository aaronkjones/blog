+++
date = "2019-03-25"
title = "Home Assistant Security System (Part 2)"
description = "Part two of the Home Assistant Security System guide"
images = []
series = ["Home Assistant", "Raspberry Pi"]
+++

## Introduction

This is part two of the [Home Assistant Security System](https://aaronkjones.com/blog/home-assistant-security-system) guide. In this part, I show how I configured a dashboard display to complete my HA security system, to give visual feedback and, optionally, be used to manually arm the system. This guide assumes you followed [part one](https://aaronkjones.com/blog/home-assistant-security-system).

## Overview

- Materials Used
- Display Setup
- Chromium Kiosk Configuration
- Home Assistant Configuration
- Pin Pad Configuration

In my case I wanted to use the same Raspberry Pi for Home Assistant and the display. To do so I decided to use Hassbian. Since Hassbian does not include a GUI, I had to install one. If you decide to follow suit, you will need to do the same. 
I followed [this guide](https://community.home-assistant.io/t/add-desktop-environment-to-hassbian/33047) in order to get that working.
## Materials Used

* [Raspberry Pi LCD - 7" Touchscreen](https://amzn.to/2uqyCB1) - 64.95
* [Raspberry Pi 3 B+](https://amzn.to/2FAVQe7) - 39.95
* [SanDisk 32GB microSD Card](https://amzn.to/2HGCKp2) - 7.30

## Display Setup

Connecting the display to a RPi is very simple. Use the included documentation. There is also a [video](https://www.youtube.com/watch?v=uXUjwk2-qx4) that will guide you through the process.

## Chromium Kiosk Configuration

```
sudo apt-get install --no-install-recommends chromium-browser
```

## Home Assistant Configuration


## Pin Pad Configuration

