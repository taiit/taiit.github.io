---
title: Build raspberry pi realtime kernel
author: taiit
date: 2022-09-03 17:00:00 +0700
categories: [Blogging, Tutorial]
tags: [linux, raspberry pi]
pin: true
---
## Common application
```bash
$ sudo apt-get update 
$ sudo apt-get install vim gedit tmux powerline \
    tzdata apt-utils lsb-release software-properties-common openssh-client \
    wget curl gzip git time meld net-tools openssh-server
$ sudo apt install tightvncserver
```

## Build raspberry pi realtime kernel

```bash

export UNAME_R=5.15.0-1013-raspi
export RT_PATCH
export KERNEL_VERSION=5.15.0
export UBUNTU_VERSION=jammy
export LTTNG_VERSION=2.13
export KERNEL_DIR=linux-raspi

export ARCH=arm64
export triple=aarch64-linux-gnu



sudo dpkg --add-architecture ${ARCH}

# setup arch
sudo apt-get update
sudo apt-get install -q -y gcc-${triple}
sudo dpkg --add-architecture ${ARCH}
sudo sed -i 's/deb h/deb [arch=amd64] h/g' /etc/apt/sources.list
sudo add-apt-repository -n -s "deb [arch=$ARCH] http://ports.ubuntu.com/ubuntu-ports/ $(lsb_release -s -c) main universe restricted"
sudo add-apt-repository -n -s "deb [arch=$ARCH] http://ports.ubuntu.com/ubuntu-ports $(lsb_release -s -c)-updates main universe restricted"
# sudo rm -rf /var/lib/apt/lists/* 
sudo apt-get update
sudo apt-get build-dep -q -y linux

```
### install build deps
``` bash

$ sudo apt-get install libncurses-dev flex bison openssl libssl-dev dkms \
    libelf-dev libudev-dev libpci-dev libiberty-dev autoconf fakeroot

if test -z $UNAME_R; then 
        curl -s http://ports.ubuntu.com/pool/main/l/linux-raspi/ | grep linux-buildinfo |  grep -o -P '(?<=<a href=").*(?=">l)' | grep ${ARCH} | grep ${KERNEL_VERSION} | sort | tail -n 1 | cut -d '-' -f 3-4
        UNAME_R=`curl -s http://ports.ubuntu.com/pool/main/l/linux-raspi/ | grep linux-buildinfo | grep -o -P '(?<=<a href=").*(?=">l)' | grep ${ARCH} | grep ${KERNEL_VERSION} | sort | tail -n 1 | cut -d '-' -f 3-4`-raspi
fi

mkdir -~/work/rpi${KERNEL_DIR}
cd ~/work/rpi/
git clone -b master --single-branch https://git.launchpad.net/~ubuntu-kernel/ubuntu/+source/linux-raspi/+git/${UBUNTU_VERSION} ${KERNEL_DIR}
cd ~/work/rpi/${KERNEL_DIR}

# checkout necessary tag
git fetch --tag
echo $UNAME_R > ~/work/rpi/uname_r.txt
git tag -l *`cat ~/work/rpi/uname_r.txt | cut -d '-' -f 2`* | sort -V | tail -1 > ~/work/rpi/tag.txt
git checkout `cat ~/work/rpi/tag.txt`

# install buildinfo to retieve `raspi` kernel config
cd ~/work/rpi

wget http://ports.ubuntu.com/pool/main/l/linux-raspi/linux-buildinfo-${KERNEL_VERSION}-`cat ~/work/rpi/uname_r.txt | cut -d '-' -f 2`-raspi_${KERNEL_VERSION}-`cat ~/work/rpi/tag.txt | cut -d '-' -f 4`_${ARCH}.deb

dpkg -X *.deb ~/work/rpi


# install lttng dependencies
# echo "deb http://cz.archive.ubuntu.com/ubuntu impish main" | sudo tee /etc/apt/sources.list.d/impish_main.list
sudo apt-get update
sudo apt-get install -y libuuid1 libpopt0 liburcu-dev libxml2 numactl

cd ~/work/rpi/${KERNEL_DIR}
/home/taihv/work/rpi/get_rt_patch.sh `make kernelversion` > ../rt_patch.txt
cd ~/work/rpi/
wget http://cdn.kernel.org/pub/linux/kernel/projects/rt/`echo ${KERNEL_VERSION} | cut -d '.' -f 1-2`/older/patch-`cat ~/work/rpi/rt_patch.txt`.patch.gz

gunzip patch-`cat ~/work/rpi/rt_patch.txt`.patch.gz


sudo apt-add-repository -s -y ppa:lttng/stable-${LTTNG_VERSION}
sudo apt-get update
apt-get source lttng-modules-dkms


# patch kernel, do not fail if some patches are skipped
cd ~/work/rpi/${KERNEL_DIR}
patch -p1 --forward < ~/work/rpi/patch-`cat ~/work/rpi/rt_patch.txt`.patch


# setup build environment
cd ~/work/rpi/${KERNEL_DIR}
export $(dpkg-architecture -a${ARCH})
export CROSS_COMPILE=${triple}-
# no need to run fakeroot debian/rules clean
# no need to run LANG=C fakeroot debian/rules printenv

cp /home/taihv/work/rpi/usr/lib/linux/`cat ~/work/rpi/uname_r.txt`/config  .config 
# make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- menuconfig
make menuconfig
make LOCALVERSION=-raspi -j `nproc` bindeb-pkg
make LOCALVERSION=-raspi -j20 bindeb-pkg

scp *.deb taihv@192.168.79.27:~/work
sudo dpkg -i linux-image-*.deb
sudo apt update
sudo apt upgrade


COPY ./.config-fragment /home/user/linux_build/.

# config RT kernel and merge config fragment

./scripts/kconfig/merge_config.sh .config ../.config-fragment

cd ~/work/rpi/${KERNEL_DIR}
    && fakeroot debian/rules clean

```


## Version 3


# make bcm2711_defconfig
``` bash
./scripts/config --disable CONFIG_VIRTUALIZATION
RUN ./scripts/config --enable CONFIG_PREEMPT_RT
RUN ./scripts/config --disable CONFIG_RCU_EXPERT
RUN ./scripts/config --enable CONFIG_RCU_BOOST
RUN ./scripts/config --set-val CONFIG_RCU_BOOST_DELAY 500
```
CONFIG_HIGH_RES_TIMERS=y
CONFIG_NO_HZ	=	y				 #	when	idle,	Eckles	to	save	energy	
CONFIG_HZ	=	100 #	Eck	every	10	ms

## Version 4
``` bash
export ARCH=arm64
export triple=aarch64-linux-gnu
export $(dpkg-architecture -a${ARCH})
export CROSS_COMPILE=${triple}-

make menuconfig
make LOCALVERSION=-raspi -j `nproc` bindeb-pkg
make LOCALVERSION=-raspi -j20 bindeb-pkg

```