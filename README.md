# Introduction

This project contains instructions and Makefile for setting up a portable Astrophotography computer.
It can be used for setting different computers:  Raspberry Pi, Libre Computers Le Potato and a regular x86 PC.
Why use makefile as opposed to shell script? Because make stops execution in case of failures and can be invoked for the whole installation or for a part of it.

# Warning
This script is better suited for users, who have experience with Linux and want to build a system from scratch, utilizing official images from Ubuntu. 
In case if you don't have experience with Linux, it is suggested to install Astroberry image or buy StellarMate image.

# List of features:
1. Installs most commonly used Astrophotography software:
* INDI
* Kstars
* PHD2
* CCDCiel
* Skychart
* Astrometry with sextractor
* ASTAP plate solver, which is much faster than astrometry
2. Sets up Wireless Access Point. Default name is RPI and password is password but can be changed in the script. Once connected to WAP,  IP address of PI is 10.0.0.1
3. Sets up vnc to be started automatically
4. Configures screen to be 1920x1080 for headless operation
5. Miscellaneous software
* Joplin notes app (broken under 20.04)
* Syncthing for syncing images into processing PC
* Arduino IDE 
* Latest Libraw with Canon CR3 support. At this point, only CCDCiel is working with this library, Ekos crashes with it
6. Fully headless operation

# Installation for Raspberry Pi

1. Downlaod 32 bit image of 20.04 from page:

https://ubuntu.com/download/raspberry-pi

It looks like they use same image for RPi3 and RPi4, so same image can be used for RPi3

2. Unpack and burn image into SD Card.

3. Connect to Pi

Connect Ethernet cable, put the card into RPI and boot.

Once green networking LED starts blinking, you can try to find the RPI on the network using nmap.
Replace 192.168.1.0 by your network's subnet:

```
nmap -p 22 --open -sV 192.168.1.0/24
```

You can also find IP address of your RPi in your router admin page, look for DHCP leases.
Of course, you can also just add connect mouse, keyboard and screen and find the IP using command:
```
ip addr
```


Once you identify IP of your PI,  login into it using ssh, with user/password : ubuntu/ubuntu, e.g.:

```
ssh ubuntu@192.168.1.100
```

It will ask to change the password.

4. Install the software

Run the following commands.
At some point installer will ask to select Display Manager. You need to chose lightdm.

```
sudo apt update
sudo apt install -y git make
git clone https://github.com/avarakin/AstroPiMaker4.git
cd AstroPiMaker4
make
```
This will take an hour or so. It may ask some questions, so monitor the process.
You will need to reboot Pi after that.
Once Pi is up, you should be able to see it as RPI in the list of available Access Points. Password is "password" but can be changed in the script. Once connected to WAP,  IP address of PI is 10.0.0.1

# Steps after installation
1. Reboot the system.
2. Connect to it using VNC
3. Installer creates a startup script indi.sh in home directory which you need to edit to include your drivers
4. In case if you run Gnome, you can change the look and feel of desktop to be more conventional by opening Gnome Tweaks tool and enabling "Dash to Panel" and "System Monitor" extensions

# Installation for x86 PC
Install any flavor of Ubuntu you like and then install script and rest of software:

sudo apt update
sudo apt install -y git make
git clone https://github.com/avarakin/AstroPiMaker4.git
cd AstroPiMaker4
make pc

# Installation for Libre Computer Le Potato
Libre Computer Le Potato is a low cost ($35 as of January 2023) Raspberry Pi clone, which is widely available. It does have some shortcomings comparing to Pi 4: 
1. Lower speed, e.g. it takes 30s to plate solve an image vs 22s on RPi4
2. It only has USB2 and no USB3, but performance degradation was not noticeable even for 26MB IMX571 mono chip
3. Network is only 100MB and not 1GB, but performance degradation was not noticeable while using VNC
4. 
