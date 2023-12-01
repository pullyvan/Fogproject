# Fogproject

[Fog project](https://fogproject.org/) is open source project mean to deploy and capture image via partclone and pxe. There are many tools that can also be used depending what you're expecting have a look on their website [Fog project](https://fogproject.org/) 

## Dependicies 
* git
* openssh-server (usefull to manage through ssh)

## Service installed by fog

* DHCP server from fog or other
* apache (web server)
* PXE
* TFTP
* NFS

## Installation

For the installation I recommend the use of debian based OS, for the example I will use Ubuntu 22.04

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
 
 * Interface: eno1
 * Server IP Address: 192.168.83.4
 * Server Subnet Mask: 255.255.255.0
 * Hostname: adminimt-HP-Z230-Tower-Workstation
 * Installation Type: Normal Server
 * Internationalization: No
 * Image Storage Location: /images
 * Using FOG DHCP: Yes
 * DHCP router Address: 192.168.83.1
 * Send OS Name, OS Version, and FOG Version: Yes
```
Then let Fog install all the necessary package after it finish it will asked you to log in from the webpage in this example (192.168.83.4). Saved your database with command it will show from the fog webpage.

Finally you can enjoy the fog project server from the fog webpage. (default user : fog)(default password: password)
## Create an image

First of all you need to create a placeholder for your image, go to image and select create an image
Set it up with your need

![Fog create an image screen](img/fog_image_created.png)

## Register a Host (pxe client)
Go to the pc that you wanted make an image of it (capture) or just want to register for other purposes

* boot from PXE (efi or legacy depending the OS you installed if efi you may have to enable efi pxe from BIOS)
* after fog pxe client is launched from pxe you can perform full registration
* Put the DNS name you want your machine OS to have in the hostname
```
![Fog full registration 01](img/fog_registration_pxe_01.png)
```
* select the image you need
```
![Fog full registration select the image](img/fog_registration_pxe_image_selection.png)
```
**DO NOT DEPLOY YET IF WANT TO CAPTURE OR IT WILL ERASE YOUR HARD DRIVE**
![Fog full registration basic answer](img/fog_registration_pxe_basic_answer.png)

## Capture an image

### From web interface

From the web interface you can go on host and select the host you want then click on capture icon(orange)
![Fog web interface capture]()

### From PXE

Boot into pxe and let it capture it 
(in case you forget to deploy during the registration process, feel free to deploy it from pxe interface)


## Deploy the image to a computer

### From PXE

* Boot in pxe
* register the computer
* reboot and choose to deploy at final question

If you forget to deploy after the registration you can do it from the web interface or the pxe client

### Deploy from web interface

From the web interface select the host and click deploy which will clone the image you captured(green icon) into that computer (you need to register the deploy machine beforehand in pxe)
![Fog PXE interface to deploy](img/fog_registration_pxe_01.png)
### Deploy from pxe
* boot in PXE
* select deploy image