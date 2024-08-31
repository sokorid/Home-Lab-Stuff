## YouTube Video Tutorial
[Update me]

## Things you'll need:

- Secondary Computer: that you will be using for your Home Server
- Ethernet Cable Plugged Up To Your Router Or Switch
- USB Flash Drive (8GB or larger): Important: All data on the drive will be erased 
- during the installation process. Make sure to back up any important files!
- Debian Server ISO File: Download the latest version directly from the official
- Debian website: https://www.debian.org/download
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
- then select the drive with the Debian install media.

## Install Guide For Debian Server
my recommendations
- make sure you select Use entire disk
- not with LVM
- select all files in one partition
- make sure you unselect desktop enviroment and GNOME
- make sure you select SSH Server
- make sure you select Grub-pc "YES"

Once It's Finished Installing
