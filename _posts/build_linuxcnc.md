sudo apt install git vim gedit apt-file rt-tests \
        automake build-essential pkg-config     \
        libglib2.0-dev libudev-dev libmodbus-dev libusb-1.0-0-dev libgtk-3-dev \
        libgtk2.0-dev *yapps* intltool libboost-all-dev tk8.6-dev bwidget tclx \
        libreadline-dev python3-tk mesa-common-dev libglu1-mesa-dev libxmu-dev \
        python3-pyqt5 python3-pyqt5.* # I am lazy

sudo apt install python3-xlib pyqt5-dev-tools python3-opengl python3-numpy python3-opencv

sudo apt --no-install-recommends install asciidoc -y

git clone https://github.com/LinuxCNC/linuxcnc.git


cd linuxcnc/src
./autogen.sh
./configure --enable-non-distributable=yes
make -j4
sudo make setuid
source ../scripts/rip-environment
../scripts/linuxcnc


# Install ubuntu-desktop
sudo apt-get install ubuntu-desktop


# remove ubuntu-desktop if any
sudo apt purge ubuntu-desktop -y && sudo apt autoremove -y && sudo apt autoclean

# then install kubuntu-desktop
sudo apt-get install kubuntu-desktop
sudo apt purge kubuntu-desktop -y && sudo apt autoremove -y && sudo apt autoclean