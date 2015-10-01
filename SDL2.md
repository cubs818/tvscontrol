These are my notes on attempting to utilize the SDL2 library on the RaspberryPi.  This could be great for an ATEM which has multiple HDMI ports.  It could used as a more powerful media player.

# Installing SDL2 #
These steps are based on [Building BitFighter on RaspberryPi](http://bitfighter.org/wiki/index.php/Building_on_Raspberry_Pi)

Update RaspberryPi firmware. Type:
```
sudo rpi-update
```

Update apt-get too. Type:
```
sudo apt-get update
sudo apt-get dist-upgrade
```
This will likely take a while 30+ minutes.

Edit the RaspberryPi's configuration:
```
sudo nano /boot/config.txt
```
... and, add the following line to the bottom the file:
```
gpu_mem=128
```
Save and exit.

You will have to reboot for the above steps to take affect.
```
sudo reboot
```

Now, get the development tools. Type:
```
sudo apt-get install build-essential libsdl1.2-dev automake
```

You will need to add yourself to the video and input groups in order to let SDL work in Command Line. Type:
```
sudo usermod -aG video,input,audio pi
```
(If you are not using the default "pi" user, change the above commands appropriately.)

You should log out and log back in now.

Get the SDL2 dependencies:
```
sudo apt-get install libudev-dev libasound2-dev libdbus-1-dev
sudo apt-get install libraspberrypi0 libraspberrypi-bin libraspberrypi-dev
```

Since SDL2 on Raspberry Pi is still not in the main repo, so you have to build it yourself:
```
cd /home/pi
wget http://www.libsdl.org/release/SDL2-2.0.3.tar.gz && tar -xvf SDL2-2.0.3.tar.gz
```

Run the configure script:
```
cd SDL2-2.0.3
./configure && make
sudo make install
cd ..
```
This will take ~1 hour.

SDL2 has several supplementary library projects for using images, fonts, etc.  These need to be installed individually:
```
wget http://www.libsdl.org/projects/SDL_ttf/release/SDL2_ttf-2.0.12.tar.gz && tar -xvf SDL2_ttf-2.0.12.tar.gz
cd SDL2_ttf-2.0.12
./autogen.sh
./configure && make
sudo make install
```
and...
```
wget http://www.libsdl.org/projects/SDL_image/release/SDL2_image-2.0.0.tar.gz && tar -xvf SDL2_image-2.0.0.tar.gz
cd SDL2_image-2.0.0
./autogen.sh
./configure && make
sudo make install
```

# Testing the Install #
For the test code included with SDL to compile and link, the Makefile needs to be modified.
```
cd ~/SDL2-2.0.3/test
./configure
nano Makefile
```
On the line, near the top of the file, beginning with "LIBS = ", add:
```
-L/opt/vc/lib
```
Save and exit.

# Failure...? #
The installs and compiles complete without errors.  But, executing the test code located in ~/SDL2-2.0.3/test/ yields mixed results:
**testplatform succeeds** testgles succeeds
**testgles2 succeeds** testver succeeds
Most of the rest, fail with "Couldn't create window: Could not initialize OpenGL / GLES library"