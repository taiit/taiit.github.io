---
title: "The very first thing I do on new raspberry pi 4."
categories:
  - Linux 
  - RaspberryPI
tags:
  - setup
  - config
---

When I got new raspberry PI version 4, I do a lot of things on this and I noted it here.

# Table of contents.

0. Install OS.

1. Setup on first boot.

2. Install & configure some softwares.

mosquitto mqtt broker

port: 1883
  
Test

  mosquitto_sub  -d -t testTopic
  mosquitto_pub -d -t testTopic -m "Hello world!"
  
Test thingsboard

  taihv@raspberry:~ $ mosquitto_pub -u nBw1IZFH2JySTt4ogkRh -h 34.69.172.61 -p 1884 -t v1/devices/me/telemetry -m '{"key1": 10}'

Test PI
  
  taihv@raspberry:~ $ mosquitto_sub -u nBw1IZFH2JySTt4ogkRh -h 127.0.0.1 -p 1884 -t v1/devices/me/telemetry
  
  taihv@raspberry:~ $ mosquitto_pub -u nBw1IZFH2JySTt4ogkRh -h 34.69.172.61 -p 1884 -t v1/devices/me/telemetry -m '{"key1": 10}'
  
