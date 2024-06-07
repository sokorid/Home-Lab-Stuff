## YouTube Video Tutorial
[link]

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

## Static IP

let's make sure that netplan is set up
```Command
ls /etc/netplan/
```

now let's make the file to house your static IP address
```Command
sudo nano /etc/netplan/01-network-manager-all.yaml
```

copy the code and paste it in
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
```Command
ip a
```

## Remove SSH Welcome
- this is not necessary to remove unless 
- you don't like seeing all the information every time 
- you SSH into your Server

```Command
sudo nano /etc/ssh/sshd_config
```
Once the file is opened, find the PrintMotd field and set its value to no.

```Command
sudo vim /etc/pam.d/sshd
```
Then find the following two lines:
- session    optional     pam_motd.so  motd=/run/motd.dynamic
- session    optional     pam_motd.so noupdate

Once you locate them, comment them down by placing # in front of each line

Keybinds For Vim
- Press I To Enter Edit Mode
- Press Escape To Leave Edit Mode
- Type :wq To Save And Close

this command will make it take effect
```Command
sudo systemctl restart ssh
```

## Clear Port 53
```bash
bash -c "$(wget -qLO - https://raw.githubusercontent.com/sokorid/Home-Lab-Stuff/main/How-To-Setup-Casaos-Through-Ubuntu-Server-For-Beginners/Clear_Port_53.sh)"
```

## Install Guide For Casa OS
Casa OS website:
https://casaos.io/

copy the command and paste it in ssh
```curl
curl -fsSL https://get.casaos.io | sudo bash
```

## Free SSL Certificates
https://www.duckdns.org/


## Dashboard Homarr
https://homarr.dev/docs/getting-started/installation/

More Icons
https://github.com/walkxcode/dashboard-icons/
