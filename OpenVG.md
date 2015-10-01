# Installing OpenVG #

Update RaspberryPi firmware. Type:
```
sudo apt-get install rpi-update
sudo rpi-update
```

Update apt-get too. Type:
```
sudo apt-get update
sudo apt-get dist-upgrade
```
This will likely take a while 30+ minutes.

The GPU needs to have more [more memory](http://elinux.org/R-Pi_Troubleshooting#Choosing_the_right_ARM.2FGPU_memory_split) than is dynamically allocated to it by default.  Edit the RaspberryPi's configuration:
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

[Example code](https://github.com/raspberrypi/firmware/tree/master/opt/vc/src/hello_pi) should be available in /opt/vc/src/hello\_pi/.
```
ls /opt/vc/src/hello_pi
```

# [OpenVG LibShapes](http://mindchunk.blogspot.com/2012/09/openvg-on-raspberry-pi.html) #

Install necessary dependencies:
```
sudo apt-get install libjpeg8-dev libfreetype6-dev git indent
```

```
git clone git://github.com/czechtech/openvg
cd openvg
make library
sudo make install
cd ..
```

Test the library:
```
cd openvg/client
make test
```
At the end of the demo, it will display the website of the library.  Press the enter key to terminate.
```
cd ../..
```

# LibQProPresentText #


# OMXPlayer #

https://github.com/popcornmix/omxplayer


# Details #

Add your content here.  Format your content with:
  * Text in **bold** or _italic_
  * Headings, paragraphs, and lists
  * Automatic links to other wiki pages