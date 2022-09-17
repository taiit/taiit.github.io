---
layout: post
title: Upgrade Cronos CNC Controller Board Firmware
date: 2022-09-17 19:24 +0700
---
Quick Reading
* TOC
{:toc}


In this post I will upgrade my Cronos CNC Controller Board Firmware

<img src="https://raw.githubusercontent.com/taiit/taiit.github.io/main/_posts/2022-09-17/2022-09-17_20h21_47.png" width="500" />

### 1. Check Cronos CNC board version
We can run [Candle](https://github.com/Denvi/Candle){:target="_blank"} then to check which version is, simple run command **I$**
e.g. my controller borad is runing version v1.1f

<img src="https://raw.githubusercontent.com/taiit/taiit.github.io/main/_posts/2022-09-17/2022-09-17_11h56_12.png" width="800" />

Then check latest release version at [grbl releases](https://github.com/gnea/grbl/releases){:target="_blank"}. In my case it is v1.1h so I would like to upgrade this version.

### 2. Backup hex file and flashing new one
##### 2.1. We will backup the current version first.
Run bellow command in Command Prompt to save pflash as a hex file:
```powershell
C:\work>"C:\Program Files (x86)\Arduino\hardware\tools\avr\bin\avrdude.exe" -C "C:\Program Files (x86)\Arduino\hardware\tools\avr\etc\avrdude.conf" -p atmega328p -c arduino -P com3 -U flash:r:dump_grbl_v1.1f.hex:i

avrdude.exe: AVR device initialized and ready to accept instructions

Reading | ################################################## | 100% 0.00s

avrdude.exe: Device signature = 0x1e950f (probably m328p)
avrdude.exe: reading flash memory:

Reading | ################################################## | 100% 4.02s

avrdude.exe: writing output file "dump_grbl_v1.1f.hex"

avrdude.exe: safemode: Fuses OK (E:00, H:00, L:00)

avrdude.exe done.  Thank you.
```

***Note:** 
* avrdude.exe is in Ardunio software package, you can go to [Ardunio Software Page](https://www.arduino.cc/en/software){:target="_blank"}, download & install it.
* ***-p com3** the current com port which connect to controller board.
* ***-U flash:r:dump_grbl_v1.1f.hex:i*** said that avrdude will read the hex file in controller board and save it to hard driver as name *dump_grbl_v1.1f.hex*.
* more information about avrdude is [here](https://www.nongnu.org/avrdude/user-manual/avrdude.html){:target="_blank"}.

##### 2.2. Then download the release hex file at [grbl releases page](https://github.com/gnea/grbl/releases){:target="_blank"}and write it to controller board
e.g: incase hex file name ***grbl_v1.1h.20190825.hex***

```powershell
C:\work>"C:\Program Files (x86)\Arduino\hardware\tools\avr\bin\avrdude.exe" -C "C:\Program Files (x86)\Arduino\hardware\tools\avr\etc\avrdude.conf" -p atmega328p -c arduino -P com3 -U flash:w:grbl_v1.1h.20190825.hex:i

avrdude.exe: AVR device initialized and ready to accept instructions

Reading | ################################################## | 100% 0.00s

avrdude.exe: Device signature = 0x1e950f (probably m328p)
avrdude.exe: NOTE: "flash" memory has been specified, an erase cycle will be performed
             To disable this feature, specify the -D option.
avrdude.exe: erasing chip
avrdude.exe: reading input file "grbl_v1.1h.20190825.hex"
avrdude.exe: writing flash (29920 bytes):

Writing | ################################################## | 100% 4.81s

avrdude.exe: 29920 bytes of flash written
avrdude.exe: verifying flash memory against grbl_v1.1h.20190825.hex:
avrdude.exe: load data flash data from input file grbl_v1.1h.20190825.hex:
avrdude.exe: input file grbl_v1.1h.20190825.hex contains 29920 bytes
avrdude.exe: reading on-chip flash data:

Reading | ################################################## | 100% 3.68s

avrdude.exe: verifying ...
avrdude.exe: 29920 bytes of flash verified

avrdude.exe: safemode: Fuses OK (E:00, H:00, L:00)

avrdude.exe done.  Thank you.

```
### Finally!!
recheck the new version is working.
<img src="https://raw.githubusercontent.com/taiit/taiit.github.io/main/_posts/2022-09-17/2022-09-17_12h00_53.png" width="800" />