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

## Time To Start Setting Up The Server
login to root account
 - instead of typing in your username in the login screen log in using root and your password

now we need to install two useful packages

use this command
```command
apt install -y sudo curl
```
now let's make sure the system is up to date
```command
apt-get -y update
```
once it finishes run this command
```command
apt-get -y upgrade
```

once this finishes up we can switch over to SSH

if you don't know what the IP address is this is how you can find it
```Command
ip a
```

We Are Going To SSH Into It
- there's a batch file in the Repository either copy it or you can download it which will make it easier for us to SSH into the server
- https://github.com/sokorid/Home-Lab-Stuff/blob/main/How-To-Setup-Casaos-Through-Ubuntu-Server-For-Beginners/Open%20SSH.bat
- you need to replace the username with the username that you created in the install
- you need to replace the IP address with the IP address that is currently set up on the server

if you create a text file and enter this into it
```text
scho off
cls
title SSH Client
color f
cls
ssh UserName@IPAddress
```
and then do save as and then replace the .txt to .bat

you should get the same result

## SSH Open
if you did it right it should ask you to agree to a fingerprint just type "yes"

login to your account

now let's give your account permission to sudo
 - to do this we need to login as root user this is the command
```command
su root
```
now let's give our main account permission
 - just replace username with your username
```command
sudo usermod -aG sudo UserName
```
now let's log back into our account
 - just replace username with your username
```command
su UserName
```
now let's make sure we're fully back into the directory of our account
```command
cd
```

Now Let's Set Up A Static IP Address

let's make the file to house your static IP address
```Command
sudo nano /etc/network/interfaces
```
now you need to look for the primary network interface

and you need to change dhcp to static

now copy the code and paste it below static
```text
address IP
netmask 255.255.255.0
gateway IP
```
Keybinds For Nano
- Control + O = Save
- Press Enter To Confirm
- Control + X = Close

now we need to restart the configuration by using this command
```command
sudo systemctl restart networking.service
```
now let's check to see if it's back up
```command
sudo systemctl status networking.service
```
Control + C to back out

## everything below this is not necessary
but you can continue with the next steps if you want


### Too Helpful Little Tools
we're going to download and install NeoFetch and Htop

the command is
```command
sudo apt install -y neofetch htop
```
now let's make sure they're installed

this is a helpful tool to give you information on your PC specs
```Command
neofetch
```

if you are used to Windows this is an advanced version of the task manager
- Keybinds to Close htop is F10
```Command
htop
```

### Remove SSH Welcome
- this is not necessary to remove unless 
- you don't like seeing all the information every time 
- you SSH into your Server
```Command
sudo nano /etc/pam.d/sshd
```
Then find the following two lines:
- session    optional     pam_motd.so  motd=/run/motd.dynamic
- session    optional     pam_motd.so noupdate
Once you locate them, comment them down by placing # in front of each line

Keybinds For Nano
- Control + O = Save
- Press Enter To Confirm
- Control + X = Close

### Automatic Updates
Install the Package the command is
```command
sudo apt install unattended-upgrades
```
now let's verify the installation
```command
systemctl status unattended-upgrades
```
Open the Configuration File
```command
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
```
now let's open the auto-update file
```command
sudo nano /etc/apt/apt.conf.d/20auto-upgrades
```
copy and paste this into the file
```text
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
APT::Periodic::AutocleanInterval "7";
```
This file allows you to define how often the auto updates take place. The lines in the file are:
- Update-Package-Lists. Use 1 to enable auto-update.
- Unattended-Upgrade. Type 1 to enable auto-upgrade.
- AutocleanInterval. Enable auto-clean packages for a specific number of days
now let's add AutocleanInterval and set it to 7 days

means the system clears the download archive every seven days.

now let's apply the changes

Keybinds For Nano
- Control + O = Save
- Press Enter To Confirm
- Control + X = Close

we have one more configuration to do
```command
sudo dpkg-reconfigure -plow unattended-upgrades
```
it's going to bring up a message and do the following
 - select "YES"
 - Select Keep the local version currently installed

now to make all the configuration changes take effect
```command
sudo systemctl restart unattended-upgrades.service
```
now let's Testing Automatic Upgrades
```command
sudo unattended-upgrades --dry-run --debug
```

this is the end of the guide hopefully this helps someone out there peace out
