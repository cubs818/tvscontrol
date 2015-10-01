# ATEM Setup #
ATEM Television Studio needs to have firmware 4.1.
[Mac](http://www.blackmagicdesign.com/support/archive?sid=3962&pid=3973&dlSeries=&os=mac&custom=true)
[Windows](http://www.blackmagicdesign.com/support/archive?sid=3962&pid=3973&dlSeries=&os=win&custom=true)

# XKeys Setup #
TVSControl is written for XKeys device model [XK-24](http://www.piengineering.com/xkeys/xk24.php).

# Raspberry Pi Setup #
The majority of the setup is done on the [raspberrypi](http://www.raspberrypi.org/faqs) model B.  This is the brains of the control.

SD card for Raspberry Pi needs to be formatted and Raspbian Wheezy 2013-09-25 installed.

[HOWTO](http://elinux.org/RPi_Easy_SD_Card_Setup#Flashing_the_SD_card_using_Mac_OSX)
[Download](http://downloads.raspberrypi.org/raspbian/images/raspbian-2013-09-27/2013-09-25-wheezy-raspbian.zip)

## Remote Connection ##
[RX, TX, Gnd, 5v](https://learn.adafruit.com/system/assets/assets/000/003/130/medium800/learn_raspberry_pi_gpio_closeup.jpg)
```
sudo screen /dev/tty.usbserial 115200
```

## Development Tools ##
```
sudo apt-get update
sudo apt-get install qt4-dev-tools
sudo apt-get install libusb-1.0-0-dev
sudo apt-get install cmake
```

## PI Engineering SDK ##
```
wget http://www.piengineering.com/developer/SDKs/pihid32-1.0.0.tar.gz
tar xvf pihid32-1.0.0.tar.gz
cd pihid32-1.0.0
./configure -D CMAKE_INSTALL_PREFIX=/usr
make
sudo make install
sudo cp udev/90-xkeys.rules /etc/udev/rules.d/
cd ..
```
There's a minor typo that needs to be fixed `sudo nano /etc/udev/rules.d/90-xkeys.rules` and delete "S" from "ATTRS".

## QATEMConnection ##
This is a fork of [libqatemcontrol](https://github.com/petersimonsson/libqatemcontrol) before Blackmagic Design moved the ATEM switchers to firmware 4.2.
```
wget https://github.com/czechtech/libqatemcontrol41/archive/master.zip
unzip master.zip
rm master.zip
cd libqatemcontrol41-master
qmake && make
sudo make install
cd ..
```

## QXKey24 ##
```
wget https://github.com/czechtech/libqxkey24/archive/master.zip
unzip master.zip
rm master.zip
cd libqxkey24-master
qmake && make
sudo make install
cd ..
```

## TVSControl ##
```
wget https://tvscontrol.googlecode.com/svn/trunk
cd tvscontrol
qmake && make
sudo make install
```
To have tvscontrol start automatically upon powering on:
```
sudo cp tvscontrol.sh /etc/init.d/
sudo chmod 755 /etc/init.d/tvscontrol.sh
sudo update-rc.d tvscontrol.sh defaults
cd ..
```

## Reboot Raspberry Pi ##
```
sudo poweroff
```
Wait 10 seconds, then unplug and replug in power to reboot.