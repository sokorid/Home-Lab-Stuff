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

## Time To Start Setting Up The Server
login to your account

now let's make sure the system is up to date
   
```Command
sudo apt update
```
once it finishes run this command
```Command
sudo apt upgrade
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

let's install are text editors
```command
sudo apt install -y nano neovim
```

Now Let's Set Up A Static IP Address

let's make the file to house your static IP address
```Command
sudo nano /etc/netplan/01-network-manager-all.yaml
```
now copy the code and paste it in
```text
network:
 version: 2
 ethernets:
   enp0s0:
     dhcp4: false
     addresses: [IP/24]
     gateway4: IP
```
we need to change three different things
 - enp0s0 to whatever our Network interface is
 - addresses: [IP/24] to [changeme/24] whatever you want your static IP to be
 - gateway4: IP to the IP address of your gateway

Keybinds For Nano

 - Control + O = Save
 - Press Enter To Confirm
 - Control + X = Close

to find out the information you need use this command
```command
ip a
```
you're looking for altname
- it should look like this "enp0s0"
- or pretty similar just make sure you copy yours
- so now that you know what you're
- Network interface is
- go back to 
```Command
sudo nano /etc/netplan/01-network-manager-all.yaml
```
and replace where it says enp0s0

with whatever yours is

if you want to make the changes take effect now type this command
```command
reboot
```
your system will reboot and you may need to reconfigure your SSH to the new IP address that you set it to

if you didn't change it to a new one you don't have to worry about that

## everything below this is not necessary
but you can continue with the next steps if you want

### Quick Little Tip

if you need to gain access to the root user and you don't want to have to log out and log back in just type this command in
```command
sudo -i
```
and to log back into your account use this command

just replace UserName to your username
```command
su UserName
```
and to go back to the user directory type this command
```command
cd
```

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

this command will make it take effect
```Command
sudo systemctl restart ssh
```

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
This file allows you to define how often the auto updates take place. The lines in the file are:
- Update-Package-Lists. Use 1 to enable auto-update.
- Unattended-Upgrade. Type 1 to enable auto-upgrade.
- AutocleanInterval. Enable auto-clean packages for a specific number of days
now let's add AutocleanInterval and set it to 7 days

means the system clears the download archive every seven days.
```text
APT::Periodic::AutocleanInterval "7";
```
now let's apply the changes
```command
sudo systemctl restart unattended-upgrades.service
```
now let's Testing Automatic Upgrades
```command
sudo unattended-upgrades --dry-run --debug
```

this is the end of the guide hopefully this helps someone out there peace out
