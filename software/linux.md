# Linux Commands
## General Commands
### Show CPU Info
```sh
cat /proc/cpuinfo
```
### Generate SSL Certificate
```sh
openssl req -new -newkey rsa:2048 -nodes -keyout server.key -out server.csr
```

### Format USB Drive
``` sh
fdisk /dev/sdd
o
w
fdisk /dev/sdd
n
sudo cryptsetup luksFormat /dev/sdd1
YES
cryptsetup luksOpen /dev/sdd1 LUKS001
mkfs.vfat /dev/mapper/LUKS001 -n LUKS001
cryptsetup luksClose LUKS001
```
### Remove a user from a group
```sh
sudo deluser user group
```

### Show Kernel Version
```sh
uname -r
```
### Set Locale
```sh
sudo dpkg-reconfigure locales
```

### Show OS Version
```sh
lsb_release -a #=> Show OS Version
cat /proc/version
cat /etc/issue
cat /etc/os-release
```
### Shutdown VPS
```sh
sudo shutdown -h now
```
### Generate SSH Key
```sh
ssh-keygen -t rsa -C "your_email@example.com"
ssh-keygen -t rsa -f temp.key
```

### Make Symbolic Link
```sh
ln -s /path/to/file /path/to/symlink
```
### Search Package
```sh
# Debian
sudo apt-cache search <package_name>

# Red Hat
sudo yum search <package_name>
```
### Show Package Information
```sh
sudo yum info <package_name>
sudo yum --enablerepo=epel info <package_name>
```
### Execute a Command at Boot
```sh
# Debian
/etc/rc.local #=> Put your cmd in this file
```
### Show Running Processes
```sh
ps auxf
```
### Show Groups
```sh
# Red Hat
sudo yum grouplist
```
### Change Hostname
```sh
sudo vi /etc/sysconfig/network
  HOSTNAME=<new_hostname>
  
# You can apply the new hostname without reboot
sudo hostname <new_hostname>
```
### Add EPEL Repos
```sh
curl -O http://dl.fedoraproject.org/pub/epel/beta/7/x86_64/epel-release-7-0.2.noarch.rpm
sudo rpm -ivh epel-release-7-0.2.noarch.rpm
yum --enablerepo=epel info zabbix
yum --enablerepo=epel install zabbix
```
### Show login history of users
```sh
sudo last
```
### Show who is logged in
```sh
sudo who
```
### Find out when was the system last rebooted
```sh
sudo last reboot
```
### See when did someone last log in to the system
```sh
sudo lastlog
```
## Network
### Get HTTP Server Headers
```sh
curl -I http://example.com
```
### Show Established Connections
```sh
netstat -an | grep ESTABLISHED
netstat -natp
```
### Show Listening Ports (udp/tcp)
```sh
netstat -nlutp
```
## Others
### AWS (Amazon Web Services)
```sh
sudo -s #=> Become root
```
### Video Card Informations
```sh
lspci | grep VGA #=> Identify your hardware (even if not configured at all)
find /dev -group video #=> Check if the correct kernel driver is loaded
glxinfo | grep -i vendor #=> Check if the correct X driver is loaded
```
### Terminal Softwares
```sh
apt-get install w3m #=> Terminal Web Browser
```
### How to mount a remote directory over ssh on Linux
```sh
sudo apt-get install sshfs
sudo usermod -a -G fuse <user_name>
sshfs my_user@remote_host:/path/to/directory <local_mount_point>

#To unmount a ssh-mounted directory:
fusermount -u <local_mount_point>

# If you would like to automatically mount over ssh upon boot, set up passwordless ssh login, and append the following in /etc/fstab.
sudo vi /etc/fstab
sshfs#my_user@remote_host:/path/to/directory <local_mount_point> fuse user 0 0
```
### Kernel Upgrade (official)
```sh
# Show available kernel images
apt-cache search linux-image

# Install kernel
sudo apt-get install linux-image-x.x.x-xx
```
### Kernel Upgrade (not-official)
```sh
# Add Debian backports
sudo vim /etc/apt/sources.list
  deb http://http.debian.net/debian wheezy-backports main
```
  
### ISO to USB Stick
```sh
# Get USB Stick place
sudo ls -l /dev/disk/by-id/*usb*
cd ~/downloads
sudo dd if=<isofile> of=/dev/sdb bs=4M; sync
# 
```
## To classified
```sh
head .bash_history #=> Show last commands
```


You can download the driver for your video card for Ubuntu 64bit from here. Assuming that you are using Ubuntu 64bit now. If you installed Ubuntu 32 bit, there is 331 version of the same driver for Ubuntu 32bit. Save your driver somewhere where you can easily access it, like your user home directory or inside a newly created nvidia directory in your user home directory.

To be able to install your nvidia driver you have to remove your previous video driver with this code in a terminal window:
```
sudo apt-get remove nvidia*
```
After you finish with this one, you should also blacklist the nouveau driver by editing this file:
```
sudo gedit /etc/modprobe.d/blacklist-nouveau.conf
```
…and add these lines at the end:
```
    blacklist nouveau
    blacklist lbm-nouveau
    options nouveau modeset=0
    alias nouveau off
    alias lbm-nouveau off
```
If, by any chance, there is no blacklist-nouveau.conf present in /etc/modprobe.d/, you can save your file as blacklist-nouveau.conf when prompted.

And you can also disable the Kernel Nouveau by typing these lines in a terminal window:
```
    echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf
```
and after that
```
    sudo update-initramfs -u
```
Now you can reboot your computer, and when you get to the login prompt, press Ctrl+Alt+F1 to exit to the terminal console. Login with your username and password.

Go to the directory where you saved your nvidia driver using the command cd in the terminal console. Eg. cd nvidia considering that you are already in your user home directory after you login. You can use command dir to be able to see your exact driver's name.

To stop your display manager or the X server, you can type in the console this code:
```
   sudo stop lightdm   or

   sudo lightdm stop
```
If you are not using lightdm as your default display manager (DM), replace lightdm with your default display manager, which can be either kdm or gdm or whatever your display manager is.

You should get a message in the terminal console saying --> lightdm stopped/waiting
```
Install Linux Headers using

  sudo apt-get install linux-headers-generic
And now you can finally install the nvidia driver using a code similar to this one:

  sudo sh NVIDIA-Linux-x86_64.....run    (for Ubuntu 64bit)  
or

  sudo sh NVIDIA-Linux-x86.....run    (for Ubuntu 32bit)
If you don't type the exact name of the driver, you'll get this message: NVIDIA-Linux... could not be found and you should type again the code for installing the driver.

Nvidia installer automatically installs the driver, and at the end it will ask you whether you want to save your new X configuration. Press Yes. After reboot and getting to your desktop and changing your NVIDIA settings as you please you should open a terminal window and type in this code:

  sudo nvidia-xconfig
to save your new nvidia configuration in /etc/X11/xorg.conf.
```
It can happen that after reboot your system shows a black screen or enters the low graphics mode. To fix this you should exit again to the console terminal, login with your username and password, and use the code provided above sudo nvidia-xconfig and also make use of the following tutorial. It is meant to fix the greeter assuming that they haven't fixed this bug in Ubuntu 14.04.
