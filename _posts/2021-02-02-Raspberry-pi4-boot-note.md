
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


### installed tool
```bash
$ sudo apt install git vim gedit \
    net-tools openssh-server raspi-config
```


sudo apt -y install tigervnc-standalone-server
vncpasswd
tigervncserver -xstartup /usr/bin/gnome-session -geometry 1920x1080 -localhost no
tigervncserver -kill :1
##### On Ubuntu, instead of the gpio group, add your user to the dialout group to give yourself access to the GPIO pins.
sudo apt install rpi.gpio-common
sudo adduser "${USER}" dialout
sudo reboot

############################################
# [New] Install new raspberry pi
## boot config.txt (todo: confirm it works)
```bash
hdmi_force_hotplug=1 
hdmi_group=2	#hdmi_group=2 (DMT)
hdmi_mode=81 # 81   1366x768  60Hz
```
## common software
```bash
sudo apt install net-tools vim tightvncserver raspi-config
```

## gui
```bash
sudo apt install xubuntu-desktop
```
## Update some config

file: ~/.vnc/xstartup
```
# Start up the standard system desktop
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
/usr/bin/startxfce4
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
x-window-manager &

# run vncserver: vncserver -geometry 1366x768
# Ensure clipboard works
# /usr/bin/autocutsel -s CLIPBOARD -fork
```

## realtime kernel update

## linuxcnc


# Tips
Remove old kernels, as well as lingering software packages that are no longer required on your system. It is a good idea to occasionally run this command just to free up disk space
```
sudo apt autoremove --purge
```
To see a list of your Linux kernels on Ubuntu, execute the following dpkg command:
```
sudo dpkg --list | egrep 'linux-image|linux-headers'
```

