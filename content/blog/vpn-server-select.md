---
author: Aaron Jones
date: 2019-02-10
title: VPN Server Select
best: true
---

This post explains how to set a VPN connection on a home router. I am using a Netgear R6300V2 with DD-WRT.

For DD-WRT, you can check if your router is supported in the Router Database.

# Enable the CLI

The CLI is disabled by default. It must be enabled to continue.

Tested on DD-WRT version 24+

* Load the DD-WRT WebUI (typically at http://192.168.1.1)
* Go to the Services tab
* Enable SSHd in the Secure Shell section
* Paste your public key (echo ~/id_rsa.pub) in the authorized key of the SSHD section that expanded
* Alternatively, enable password login
* Apply settings*

# Enable OpenVPN Client

* Still in Services, select the VPN tab
* Enable “Start OpenVPN Client”
* Apply settings

# Test Your Connection

`ssh root@192.168.1.1`

# Change settings manually

Once connected you can now change settings on the router without having to do so manually through the GUI.

> Note, most routers store settings in NVRAM. Changes to these settings should persist after a reboot.

# Set host server

`nvram set openvpncl_remoteip=<enter host here> && nvram commit`

# Start OpenVPN Service

`openvpn --config /tmp/openvpncl/openvpn.conf --route-up /tmp/openvpncl/route-up.sh --route-pre-down /tmp/openvpncl/route-up.sh --daemon`

# Kill OpenVPN Service

`ps | grep openvpn # will show you the process id`

`kill -TERM <the process id> # will terminate OpenVPN gracefully`

# Using a script

I wrote a script to simplify the process

```bash
#!/bin/bash

ROUTER_USER=root
ROUTER_ADDRESS=192.168.1.1
ROUTER_PORT=22
VPN_SERVER=$1

# Kill OpenVPN Service
ssh $ROUTER_USER@$ROUTER_ADDRESS -p $ROUTER_PORT "kill -TERM \$(ps | awk '/[o]penvpn/ {print \$1}')"

# Wait for service to terminate
sleep 5

# Set new server
ssh $ROUTER_USER@$ROUTER_ADDRESS -p $ROUTER_PORT "nvram set openvpncl_remoteip=$VPN_SERVER && nvram commit"
ssh $ROUTER_USER@$ROUTER_ADDRESS -p $ROUTER_PORT "sed -i 's/^remote\\ .*/remote $1 1194/' /tmp/openvpncl/openvpn.conf"

# Start OpenVPN Service
ssh $ROUTER_USER@$ROUTER_ADDRESS -p $ROUTER_PORT "openvpn --config /tmp/openvpncl/openvpn.conf --route-up /tmp/openvpncl/route-up.sh --route-pre-down /tmp/openvpncl/route-up.sh --daemon"
```

Supply the script with a VPN server to connect to:
`VPNServerSelect.sh server1.yourvpn.com`
