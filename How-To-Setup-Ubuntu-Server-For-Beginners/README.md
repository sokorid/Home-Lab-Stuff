## YouTube Video Tutorial
[update me]

## Things you'll need:

- Secondary Computer: that you will be using for your Home Server
- Ethernet Cable Plugged Up To Your Router Or Switch
- USB Flash Drive (8GB or larger): Important: All data on the drive will be erased 
- during the installation process. Make sure to back up any important files!
- Ubuntu Server ISO File: Download the latest version directly from the official
- Ubuntu website: https://ubuntu.com/download/server#release-notes
- BalenaEtcher: This user-friendly tool will help you create the bootable USB drive. 
- Download it here: https://etcher.balena.io/#download-etcher

## Boot the installer

- Plug the USB stick into the system to be installed and start it.
- Most computers will automatically boot from USB or DVD, though in 
- some cases this is disabled to improve boot times. If you don’t see the 
- boot message and the “Welcome” screen which should appear after it, 
- you will need to set your computer to boot from the install media.
- There should be an on-screen message when the computer starts 
- telling you what key to press for settings or a boot menu. Depending on 
- the manufacturer, this could be Escape, F2, F10 or F12. Simply restart 
- your computer and hold down this key until the boot menu appears, 
- then select the drive with the Ubuntu install media.

## Install Guide For Ubuntu Server
most important things to take note of is
- when you get to the IP screen, pay attention to what your web interface is
- make sure you disable lvm
- make sure you enable SSH

Once It's Finished Installing

We Are Going To SSH Into It
- there's a batch file in the Repository either copy it or you can download it which will make it easier for us to SSH into the server
- https://github.com/sokorid/Home-Lab-Stuff/blob/main/How-To-Setup-Casaos-Through-Ubuntu-Server-For-Beginners/Open%20SSH.bat
- you need to replace the username with the username that you created in the install
- you need to replace the IP address with the IP address that is currently set up on the server

if you don't know what the IP address is this is how you can find it
```Command
ip a
```
