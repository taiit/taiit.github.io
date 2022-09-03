
---
title: BuildRoot raspberry pi 4 boot note
author: taiit
date: 2021-02-02 10:00:00 +0700
categories: [Blogging, Tutorial]
tags: [linux, raspberry pi]
pin: true
---

Add new to config.txt
```
enable_uart=1
# disable bluetooth module and map pl011 UART on pins 14 and 15
dtoverlay=disable-bt
```
and cmdline.txt
```
console=serial0,115200
```

get running kernel config

cat /boot/config-$(uname -r)
