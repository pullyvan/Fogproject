# Fogproject

[Fog project](https://fogproject.org/) is an open source project mean to deploy and capture image with partclone and pxe. There are many tools that can also be used depending what you're expecting have a look on their website [Fog project](https://fogproject.org/) 


## Table of content

* [Dependicies](#dependicies)
* [Service installed by fog](#service-installed-by-fog)
* [Installation](#installation)
* [Create an image](#create-an-image)
* [Register a Host (pxe client)](#register-a-host-pxe-client)
* [Deploy the image to a computer](#deploy-the-image-to-a-computer)
* [Capture an image from a fog client](#capture-an-image)
* [Deploy the image you have created and capture it](#deploy-the-image-to-a-computer)

## Dependicies 
* git
* openssh-server (usefull to manage through ssh)
* vim (optional)

Here is an example of how to install package in Debian based OS (ubuntu,xubuntu, etc...)
```
apt install -y git openssh-server vim 
```
## Service installed by fog

* DHCP server from fog or other (windows server for example)
* apache (web server)
* PXE
* TFTP
* NFS
## Preparation
**Please be aware that fog use a dhcp service which will conflict if there is same service from Windows Server or elsewhere from the same subnet**

In this tutorial you may need to have a little of linux knowledge

First of all you need to configure your network interface and set a static ip, here is an example for Ubuntu using netplan

In case your public network can have a dhcp server feel free to not set up a local network interface

```
vim /etc/netplan/01-network-manager-all.yaml

---
network:
  version: 2
  renderer: networkd
  ethernets:
    eth1:	#eth1 is the interface name you can check by doing "ip a"
     addresses: [192.168.83.35/24]
     gateway4: 192.168.83.1
     nameservers:
       addresses: [8.8.8.8,8.8.4.4]


#Optional if can use your dhcp on a public network    
# intern network which will be use for fog (local network) in this example
      
    eth2: addresses:
      - 192.168.10.35/24
     
```


## Installation

For the installation I recommend to use a debian based OS. For this example I will use Ubuntu 22.04. You have to go on [download](https://fogproject.org/download) from their website and get the download link from github

```
# make sure to update
sudo apt update
# Download the project
wget https://github.com/FOGProject/fogproject/archive/1.5.10.tar.gz
# extract the compress file
tar -xvf 1.5.10.tar.gz
# go to bin
cd fogproject-1.5.10/bin
./installfog.sh
```
Be aware that fog use a dhcp service
``` 
Starting Debian based Installation

   ######################################################################
   #     FOG now has everything it needs for this setup, but please     #
   #   understand that this script will overwrite any setting you may   #
   #   have setup for services like DHCP, apache, pxe, tftp, and NFS.   #
   ######################################################################
   # It is not recommended that you install this on a production system #
   #        as this script modifies many of your system settings.       #
   ######################################################################
   #             This script should be run by the root user.            #
   #      It will prepend the running with sudo if root is not set      #
   ######################################################################
   #            Please see our wiki for more information at:            #
   ######################################################################
   #             https://wiki.fogproject.org/wiki/index.php             #
   ######################################################################

 * Here are the settings FOG will use:
 * Base Linux: Debian
 * Detected Linux Distribution: Ubuntu
 
 ## To chose the interface please be aware that it will has a DHCP server so maybe do not put on production interface
 
 * Interface: eth2
 * Server IP Address: 192.168.10.35
 * Server Subnet Mask: 255.255.255.0
 * Hostname: HP-Z230-Tower-Workstation
 * Installation Type: Normal Server
 * Internationalization: No
 * Image Storage Location: /images
 * Using FOG DHCP: Yes
 * DHCP router Address: 192.168.10.1
 * Send OS Name, OS Version, and FOG Version: Yes
```
Then let Fog install all the necessary package after it finish jit will asked you to log in from the webpage in this example (192.168.10.35) or (192.168.83.35 if you use public network). Saved your database with command it will show from the fog webpage.

Finally you can enjoy the fog project server from the fog webpage. (default user : fog)(default password: password)

## Fog usage

### Create an image

First of all you need to go on the webpage (192.168.83.35) to create a placeholder for your image, go to image and select create an image
Set it up with your need

![Fog create an image screen](img/fog_image_created.png)

### Register a Host (pxe client)
Still on the webpage, go to the pc that you wanted make an image of (capture) or just want to register for other purposes

* Boot from PXE (efi or legacy depending the OS you installed if efi you may have to enable efi pxe from BIOS)
* After fog pxe client is launched from pxe you can perform full registration
* Put the DNS name you want your machine OS to have in the hostname

![Fog full registration 01](img/fog_registration_pxe_01.png)

* select the image you need

![Fog full registration select the image](img/fog_registration_pxe_image_selection.png)

**DO NOT DEPLOY YET IF WANT TO CAPTURE OR ELSE IT WILL ERASE YOUR HARD DRIVE**

![Fog full registration basic answer](img/fog_registration_pxe_basic_answer.png)

### Capture an image

#### * From web interface

From the web interface you can go on host and select the host you want then click on capture **(orange icon)**
![Fog web interface capture](img/fog_web_interface_host.png)

#### * From PXE

Boot into pxe and let it capture it 


### Deploy the image to a computer

#### * From PXE

* Boot in pxe
* register the computer
* reboot and choose to deploy at final question

If you forget to deploy after the registration you can do it from the web interface or the pxe client

#### * Deploy from web interface

From the web interface select the host and click deploy which will clone the image you captured **(green icon)** into that computer (you need to register the deploy machine beforehand in pxe)
![Fog web interface to deploy](img/fog_web_interface_host.png)
#### * Deploy from pxe

* boot in PXE
* select deploy image
![Fog PXE interface to deploy](img/fog_pxe_interface_deploy.png)
