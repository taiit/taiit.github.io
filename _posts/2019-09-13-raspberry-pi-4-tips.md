---
title: "Raspberry Pi 4 Tips"
categories:
  - Linux 
  - RaspberryPI
tags:
  - tips
---

I wrote down all tips for raspberry pi 4 here.

# Table of contents.

# 0. Bluetooth Connection

Run command bluetoothctl tp control bluetooth devices.

``` bash
  $ bluetoothctl
  [bluetooth]# scan on
  Discovery started
  [CHG] Controller DC:A6:32:18:78:94 Discovering: yes
  [NEW] Device 6A:03:46:9B:FC:80 6A-03-46-9B-FC-80
  [NEW] Device ED:8E:0E:1A:5F:27 RAPOO BleMouse
  ...
  [bluetooth]# connect 6C:5D:63:76:5F:16
  Attempting to connect to 6C:5D:63:76:5F:16
  [CHG] Device 6C:5D:63:76:5F:16 Connected: yes
  [CHG] Device 6C:5D:63:76:5F:16 Modalias: usb:v0A5Cp0001d0129
  [CHG] Device 6C:5D:63:76:5F:16 UUIDs: 00001124-0000-1000-8000-00805f9b34fb
  [CHG] Device 6C:5D:63:76:5F:16 UUIDs: 00001200-0000-1000-8000-00805f9b34fb
  [CHG] Device 6C:5D:63:76:5F:16 ServicesResolved: yes
  Connection successful
  ...
  [bluetooth]# help
  
```

Useful commands

- discoverable on
- scan on
- connect <device>
- help

