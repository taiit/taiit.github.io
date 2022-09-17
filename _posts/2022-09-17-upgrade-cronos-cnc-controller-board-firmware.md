---
layout: post
title: Upgrade Cronos CNC Controller Board Firmware
date: 2022-09-17 19:24 +0700
---

In this post I will upgrade my Cronos CNC Controller Board Firmware

<img src="https://raw.githubusercontent.com/taiit/taiit.github.io/main/_posts/2022-09-17/2022-09-17_20h21_47.png" width="500" />

### 1. Check Cronos CNC board version
We can run [Candle](https://github.com/Denvi/Candle) then to check which version is, simple run command **I$**
e.g. my controller borad is runing version v1.1f

<img src="https://raw.githubusercontent.com/taiit/taiit.github.io/main/_posts/2022-09-17/2022-09-17_11h56_12.png" width="800" />

Then check latest release version at [grbl releases](https://github.com/gnea/grbl/releases). In my case it is v1.1h so I would like to upgrade this version.

### 2. Backup hex file an flashing new one
##### 2.1. We will backup the current version first.
Run bellow command in Command Prompt to save pflash as a hex file:
```
"C:\Program Files (x86)\Arduino\hardware\tools\avr\bin\avrdude.exe" -C "C:\Program Files (x86)\Arduino\hardware\tools\avr\etc\avrdude.conf" -p atmega328p -c arduino -P com3 -U flash:r:dump_grbl_v1.1f.hex:i
```

***Note:** 
* avrdude.exe is in Ardunio software package, you can go to [Ardunio Software Page](https://www.arduino.cc/en/software), download & install it.
* ***-p com3** the current com port which connect to controller board.
* ***-U flash:r:dump_grbl_v1.1f.hex:i*** said that avrdude will read the hex file in controller board and save it to hard driver as name *dump_grbl_v1.1f.hex*.
* more information about avrdude is [here](https://www.nongnu.org/avrdude/user-manual/avrdude.html).

##### 2.2. Then download the release hex file at [grbl releases](https://github.com/gnea/grbl/releases) and write it to controller board
e.g: with hex file name ***grbl_v1.1h.20190825.hex***

```
"C:\Program Files (x86)\Arduino\hardware\tools\avr\bin\avrdude.exe" -C "C:\Program Files (x86)\Arduino\hardware\tools\avr\etc\avrdude.conf" -p atmega328p -c arduino -P com3 -U flash:w:grbl_v1.1h.20190825.hex:i
```
### Finally!! new version is working
<img src="https://raw.githubusercontent.com/taiit/taiit.github.io/main/_posts/2022-09-17/2022-09-17_12h00_53.png" width="800" />