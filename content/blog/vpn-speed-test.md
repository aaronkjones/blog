+++
date = "2019-01-13"
title = "VPN Speed Test"
description = "A guide on how to find the fastest VPN your provider offers"
images = []
series = ["Networking", "VPN"]
+++

This script will automate selecting the best VPN server from a given list of servers your provider offers.

# Requirements

* OpenVPN Client
* speedtest-cli
* Linux/POSIX/Bash for Windows

I am using usenetserver.com. In your case, these settings may differ.

```bash
#!/bin/bash

# List of hosts to test
HOSTS_FILE=inventory

# Your VPN provider's base config
BASE_CONF=uns.conf

# A temp config that will have a new host for each run
TEMP_CONF=temp.conf

# Results
RESULTS_FILE=speedresults

# Read hosts from file
while read -r HOST;
do

# Delete old results file if it exists
if [ -a $RESULTS_FILE ]; then
    rm "$RESULTS_FILE"
fi

# Change config to current host
sed "s|^remote\\ .*|remote $HOST|" $BASE_CONF > $TEMP_CONF

# Connect OpenVPN
sudo openvpn --config "$TEMP_CONF" --daemon

# Run speedtest and store result
{ echo "$HOST, "; speedtest --simple; printf " \\n"; } >> $RESULTS_FILE

# Disconnect OpenVPN
sudo kill -TERM "$(ps aux | awk '/[o]penvpn/ {print $2}')"

# Wait for process to die
sleep 5

# Display results
cat $RESULTS_FILE

done < "$HOSTS_FILE"
rm "$TEMP_CONF"
```
What it does:  

* Read a server from a list of servers (inventory)
* Modify the OpenVPN config to use that server
* Connect OpenVPN with new config * Run a speedtest using speedtest-cli 
* Display the results
