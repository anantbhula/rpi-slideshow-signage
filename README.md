# Digital signage using Raspberry Pi

All I wanted was a simple slideshow that would run on raspberry pi boot. Since I found the solutions which was bits and pieces from different source I figure i compile them in one document for my reference as well as others. All of the solution provided here are what worked for me and does not necessary means it will work for you,however I am always willing to improve so feel free to send me a message if you have better solution.

Material used: RPi 3, 16GB microSD, 5v 2A microUSB power supply, HDMI cable and diplay.
(You are browsing on github meaning you already know how you can make the RPi connection with display and ethernet cable.

## Raspbian installation
1. Download raspbian image from [here](https://www.raspberrypi.org/downloads/raspbian/)
2. Use [Etcher](https://www.balena.io/etcher/) or similar program to flash os to microSD card.
3. Plug ethernet cable, HDMI cable, microSD, keyboard/mouse and power supply to boot rpi.

## System update and upgrade (Optional)
If installed the latest version of the Raspbian OS above, then skip ahead. Otherwise, update Raspbian to the latest version.
```
sudo apt-get update
sudo apt-get dist-upgrade
```

## Screensave and Image viewer
Now install screensaver and image viewer tool using
```
sudo apt-get install feh
sudo apt-get install xscreensaver
```

## Disable Screensaver Blanking

check the consoleblank value it must be 0
```
cat /sys/module/kernel/parameters/consoleblank
```

If the value return other than 0 then edit the file `/boot/cmdline.txt` and add `consoleblank=0` to turn screen blanking off completely

## Store Image
We will be running a slideshow which will pickup image files from `/home/pi/Pictures/Slides'.

First create `Slides` directory
```
sudo mkdir /home/pi/Pictures/Slides
```
Now stores any images that you want to display as part of the slideshow within `Slides` directory

## Slideshow script
In the home directory `/home/pi`, create a file slideshow.sh
```
sudo nano /home/pi/slideshow.sh
```

Include this three lines:
```
#!/bin/sh
cd /home/pi/Pictures/Slides
feh -Z -z -F -D 10 --hide-pointer --auto-rotate
```
where 10 is in second interval, changes images every 10 seconds and hides the mouse pointer among other things.

Change the mode of `slideshow.sh` to 755.
```
sudo chmod 755 ~/slideshow.sh
```

## Make slideshow.sh script to run on boot
```
sudo nano /etc/xdg/lxsession/LXDE-pi/autostart
```
Now add following line at the end of the script
```
@/bin/sh /home/pi/slideshow.sh
```

##(Optional) Change Orientation
Now we'll make a couple of system configuration changes
```
sudo nano /boot/config.txt
```
and add

```
#Display orientation. Landscape = 0, Portrait = 1
display_rotate=1

# Use 24 bit colors
framebuffer_depth=24
```

## Reboot
Now reboot rpi and make sure the slideshow script runs on boot
```
sudo reboot
```
