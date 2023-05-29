---
title: Linux CNC in Raspberry PI
author: taiit
date: 2022-09-03 18:10:00 +0700
categories: [Blogging, Tutorial]
tags: [linux, raspberry pi]
pin: true
---

Quick Reading
* TOC
{:toc}

## Run HAL (Hardware Abstraction Layer)
```
halcmd: loadrt hal_pi_gpio
halcmd: show pin
halcmd: show comp
halcmd: show thread
halcmd: show func
halcmd: show param
halcmd: loadusr halshow 

```

start halcmd by command: halrun
#1. Hello word, run siggen
```bash
halcmd: loadrt siggen
halcmd: loadrt threads name1=test-thread period1=1000000

halcmd: show thread
Realtime Threads:
     Period  FP     Name               (     Time, Max-Time )
    1000000  YES           test-thread (        0,        0 )

halcmd: show funct
Exported Functions:
Owner   CodeAddr  Arg       FP   Users  Name
 00004  ffffacf30d34  ffffac110120  YES      0   siggen.0.update

halcmd: addf siggen.0.update test-thread

halcmd: show thread
Realtime Threads:
     Period  FP     Name               (     Time, Max-Time )
    1000000  YES           test-thread (        0,        0 )
                  1 siggen.0.update
# Using GPIO26 as output
halcmd: loadrt hal_pi_gpio dir=16777216 exclude=5033164
halcmd: addf hal_pi_gpio.read test-thread
halcmd: addf hal_pi_gpio.write test-thread
halcmd: net LED <= hal_pi_gpio.pin-37-out
halcmd: net LED => siggen.0.clock
or 
net LED => siggen.0.clock <= hal_pi_gpio.pin-37-out

halcmd: show pin
Component Pins:
Owner   Type  Dir         Value  Name
    29  bit   IN           TRUE  hal_pi_gpio.pin-37-out <== LED
....
    21  bit   OUT          TRUE  siggen.0.clock ==> LED
....
halcmd: start

```
