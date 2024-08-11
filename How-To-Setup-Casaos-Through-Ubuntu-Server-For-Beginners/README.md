## YouTube Video Tutorial
https://youtu.be/7O3H2OiVSq4

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
## SSH Open

now let's check to make sure the server and all of its packages are up to date
```Command
sudo apt-get -y update
```
now let's install the updates
```Command
sudo apt-get -y upgrade
```

now let's install two helpful packages
- they're not necessary but I recommend it
```Command
sudo apt-get install -y neofetch htop
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
sudo bash -c "$(wget -qLO - https://raw.githubusercontent.com/sokorid/Home-Lab-Stuff/main/How-To-Setup-Casaos-Through-Ubuntu-Server-For-Beginners/Clear_Port_53.sh)"
```

## Install Guide For Casa OS
Casa OS website:
https://casaos.io/

copy the command and paste it in ssh
```curl
curl -fsSL https://get.casaos.io | sudo bash
```

just in case you accidentally changed it to a port that is not available this is to fix it
```bash
sudo bash -c "$(wget -qLO - https://raw.githubusercontent.com/sokorid/Home-Lab-Stuff/main/How-To-Setup-Casaos-Through-Ubuntu-Server-For-Beginners/change_casaos_web_ui_port.sh)"
```


## Free SSL Certificates
https://www.duckdns.org/

login with one of the options
- copy your token
- create a valid domain
- once it's created change your current ip to the IP address of your server
- and then click update IP
 
## NginxProxyManager
now inside of NginxProxyManager go to SSL Certificates
- Add Let's Encrypt Certificate
- now let's type in your domain like this
- yourdomainname.duckdns.org
- Press Enter To Confirm your domain
- *.yourdomainname.duckdns.org
- Press Enter To Confirm your domain
- Select Use a DNS challenge
- DNS Provider Select DuckDNS
- replace this with your token, your-duckdns-token
- leave this blank Propagation Seconds
- now select I Agree to the Let's Encrypt Terms of Service
- now click save

if it fails wait a little bit and try it again

## Pi-hole
if you're looking for a better ad block list here's a website that should help you do that

https://avoidthehack.com/best-pihole-blocklists
- I recommend using this one https://firebog.net/
- anything colored green means it receives the most updates as soon as it's available
- in Pi-hole go to Add Adlist
- take the URL that you copied and paste it inside of Address:
- then click add
- repeat this to your happy
- once you're done go to tools
- and go to Update Gravity
- click update
- and it will download all the necessary data from those URLs that you added to the Adlist
- once it's completed you should be good to go

## VaultWarden
to do anything with this you need to do a proxy to make it Force https:// 
otherwise, it will not allow you to do anything
- go to NginxProxyManager
- go to host and to Proxy Hosts
- add a new Proxy Host
- enter in your domain that you would like to use for example
- psw.yourdomainname.duckdns.org
- Press Enter To Confirm your domain
- go to Forward Hostname / IP
- Enter the IP address of your server
- go to Forward Port
- enter in whatever Port you created in the install of VaultWarden
- it's typically if installed through casaos it is 10380 but you could change that to whatever
- now select Block Common Exploits and add it
- now go to SSL
- and add you're already created SSL Certificate
- now select these two options to enable them Force SSL and HTTP/2 Support
- now click save
- now go to your newly created URL for example
- https:// psw.yourdomainname.duckdns.org
- and now you should be done
- now if you want to use this on your browser go and download the extension BitWarden
- choose self-hosted and enter your domain that you created for example https:// psw.yourdomainname.duckdns.org
- now login and you should be good to go


## Dashboard Homarr
https://homarr.dev/docs/getting-started/installation/

More Icons
https://github.com/walkxcode/dashboard-icons/
