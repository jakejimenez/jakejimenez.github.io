---
layout: post
title: Hacking with a Raspberry Pi Zero (W)
---

## Headless Setup and P4wnP1 Tutorial

Now this is one of the better “hacking” source code repository tools I have found for the raspberry pi, rivaled largely by PoisonTap created by Samy Kamkar. Although, P4wnP1 (by MaMe82) establishes many more features when compared to PoisonTap, it’s ability to install backdoors, create SSH’able wifi hotspots for remote control, and ability to run ducky scripts on boot make it excellent for this tutorial.

---

Now this is one of the better “hacking” source code repository tools I have found for the raspberry pi, rivaled largely by PoisonTap created by Samy Kamkar. Although, P4wnP1 (by MaMe82) establishes many more features when compared to PoisonTap, it’s ability to install backdoors, create SSH’able wifi hotspots for remote control, and ability to run ducky scripts on boot make it excellent for this tutorial.

Equipment List:
1. Raspberry Pi Zero or Raspberry Pi Zero W
2. USB OTG Adapter
3. Windows or OSX Computer (With internet connection)
4. Ability to plug Micro SD Card into computer.

Software List:
1. Raspbian Jessie Lite [Link](http://downloads.raspberrypi.org/raspbian_lite/images/)
2. P4wnP1 by MaMe82 [Link](https://github.com/mame82/P4wnP1.git)
3. Etcher [Link](https://etcher.io/)

###Headless Setup


---

Now that you’ve got your requirements squared away, it’s time to turn to our Pi Zero setup. For this we are going to need to plug the USB OTG cable into the second MicroUSB port on the Pi, the one labeled “USB” and not “PWR IN”. Go ahead and plug your SD card into your computer and let’s get the Pi ready for P4wnP1.
First, go ahead and open Etcher. Select your Jessie Lite image and flash that to your SD card.



---

Once your image has been flashed, unplug the SD card and plug it back in. The SD Card should be named “boot”, go ahead and open that.

Now, in order to SSH into our Pi locally, we are going to need to do some initial setup. First open Notepad or TextEdit (or any other text editor) and create a file called “ssh” with no extension or content. Save that to your boot SD card. Next, you’re going to need to edit your “cmdline.txt” file. To make this easier I have provided everything you need to copy and paste, so go ahead and delete everything in “cmdline.txt” and copy and paste the code below.

```dwc_otg.lpm_enable=0 console=serial0,115200 console=tty1 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=deadline fsck.repair=yes rootwait modules-load=dwc2,g_ether quiet init=/usr/lib/raspi-config/init_resize.sh```

Alright, now we are going to get the last part done for our headless setup before we move on to booting up our Pi. This leads us to the “config.txt” file in our boot SD card, go ahead and open this file and scroll all the way to the bottom. When there, under where it says

```dtparam=audio-on```

add

```dtoverlay=dwc2```

So in the end the bottom of your file will look like this:

```dtparam=audio-on
dtoverlay=dwc2```

You’re done with the this step! Now for step 3.


---

Unplug your micro SD card and plug it into your Pi Zero, once done, plug the Pi into your computer using the OTG cable. Your computer should register it within 15 seconds or so, then give the pi around 30 sec — 1 min for the Pi to boot up.
Go ahead and open up PuTTy or Terminal (for Mac) and ssh into your Pi by typing in:
```raspberrypi.local```
or
```ssh pi@raspberrypi.local```
for Mac.

It should then ask you for a password, enter “raspberry” as the password. Boom! You should now see your Pi’s terminal window in front of you. This next section may include the most error messages and such. Trial and error may be needed.

Mac: Navigate to System Preferences > Sharing > Internet Sharing and Share your connection with the RNDIS/Ethernet adapter. Your Pi should now have internet.

Windows: Control Panel > Network > Change adapter settings and you should see something identified as a RNDIS/Ethernet adapter (In my case, its named Ethernet 2), and your main internet. Go to your main internet connection, right click and select properties. Select the sharing tab and and share your connection with whatever your RNDIS/Ethernet adapter is named. You can do all of this while SSH’ed into your Pi, so once done, try
ping google.com

If that works, you should be good to continue. If not, try restarting your Pi.

Now that internet has been obtained. It’s time for our next step.


---

* Now it’s time to get the software side of our Pi setup. Go ahead and run
```sudo apt-get update```
and wait for that to finish. We now need to get our repository that, but in order to do that we need git. In order to do that we need to run the following command.
```sudo apt-get install git```
Once git is installed just clone the P4wnP1 directory in the root directory of your P1.
```git clone https://github.com/mame82/P4wnP1.git```
We are almost done! All we need to do now is test this bad boy out. So go ahead and
```cd P4wnP1 && ./install.sh```
Give that some time to run, it will take a while so go outside or idk, walk your dog. Once it is done, unplug it and plug it back in, SSH back into it and continue from this point.

Okay! You’re back into your Pi after running the install, now we need to setup our first payload! Go ahead and type
```sudo nano setup.cfg```
You should see many different options and such, once you have read the documentation fully, you can go ahead and run through those options but for now lets focus on the bottom portion of the `setup.cfg` file.
At the bottom of the file, you should see something similar to this:

```PAYLOAD=network_only.txt
#PAYLOAD=hid_keyboard.txt```

Go ahead and comment out the `network_only.txt` payload and uncomment the `hid_keyboard.txt`
So that your end result will look something like this:

```#PAYLOAD=network_only.txt
PAYLOAD=hid_keyboard.txt```

You’re done!
Now that everything is done, all there is to do is shutdown the Pi safely and plug it into a computer. I’ve found that it’s easier to test on a Windows computer, so I’d suggest using that first.
```sudo shutdown now```
And go ahead and plug that bad boy into an unlocked Windows PC… and voila! It should open Notepad and enter something in.

Fin.
