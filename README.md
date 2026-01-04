
## Proxmox

<a id="readme_top"></a>



#### Overview

Proxmox delivers powerful, enterprise-grade solutions with full access to all functionality for everyone - highly reliable and secure. The software-defined and open platforms are easy to deploy, manage and budget for. 
 
+ https://www.proxmox.com/en/

#### Guides

If you want to start you new Smart Home and set the foundations from the start the first component Proxmox will allow you to start small but also have the capability to easily expand and add more complicated software later.  
The links below will take you to all the guides I am working on starting with how to install Proxmox, moving on to more advanced setups like GPU passthrough and adding different application to build your Smart Home starting with Home Assistant and more.



+ #### Coming Soon!!














Listed below is all the software I use, select the one you want to know more about.  



<summary><u>Table of Contents</u></summary>

+ <a href="#Hardware_Needed_for_Proxmox">Hardware Needed for Proxmox</a>
	+ <a href="#Demo_System">Demo System</a>

+ <a href="#Installing_Proxmox">Installing Proxmox</a>
	+ <a href="#Installing_Proxmox">Post Installation of Proxmox</a>

+ <a href="#Proxmox_to_USB_Drive">Connect to a External Drive</a>
	+ <a href="#Proxmox_to_USB_Drive">Connect to a USB Drive</a>
	+ <a href="#Proxmox_to_NFS">Connect to a NAS device with NFS</a>

+ <a href="#NVIDIA_Drivers">Installing NVIDIA Drivers</a>
	+ <a href="#NVIDIA_Drivers">NVIDIA Drivers on Proxmox</a>
	+ <a href="#NVIDIA_Drivers">NVIDIA Drivers on LXC's</a>

</details>  






<a href="https://github.com/HomeStudiosDIY/HomeStudiosDIY/blob/main/README.md">Back to Home Page</a>






<p align="right"><a href="#readme_top">back to top</a></p>


## Hardware to run Proxmox:
<a id="Hardware_Needed_for_Proxmox"></a>

Hardware you can use to run Proxmox on. I have listed three different types and there uses.



### Demo System:
<a id="Hardware_Needed_for_Proxmox"></a>

Other once I have done for friends

Just running Proxmox, Home Assistant and Frigate
NUC & charger Intel Celeron(R) CPU 847E 1.1ghz 6gb RAM Chrome OS Cloud Ready

Google Coral - Edge TPU Accelerator - Mini PCIe - Frigate - AI - MobileNet V2


USB Disk for Cam recordings



### Friends Device:
<a id="Hardware_Needed_for_Proxmox"></a>
7 Cam -
Google Coral USB Edge TPU ML Accelerator coprocessor for Raspberry Pi and Other Embedded Single Board Computers
HP ProDesk 405 G4 Mini
CPU: Ryzen 5 PRO 2400GE (QUAD CORE)
SSD: 256GB SSD
RAM: 16GB DDR4

USB Disk for Cam recordings





### My Production System:
<a id="Hardware_Needed_for_Proxmox"></a>

AMD Ryzen 7 5800X
126GB Ram
NVIDIA GeForce RTX 3090
x2 6 Port 2.5 Gigabit PCI-e x4 Ethernet Network Card
USB Coral

Google Coral USB Edge TPU ML Accelerator coprocessor for Raspberry Pi and Other Embedded Single Board Computers


Synology 


Here with some config settings to help you get you ProxMox Server setup and working!!  

I run all my application on a LXC inside Docker but you can run the LXC application directly this was just my preference for consistency as not all application I use can run directly on a LXC. All my LXC config and Docker Compose files will also be sheared.



https://community-scripts.github.io/ProxmoxVE/





https://community-scripts.github.io/ProxmoxVE/scripts?id=scaling-governor










<p align="right"><a href="#readme_top">back to top</a></p>

## Installing Proxmox
<a id="Installing_Proxmox"></a>

Here is a link to my YouTube video covering all the steps if you don't like reading.

+ Link Coming Soon!!




You will need a Computer and also a USB Stick to install Proxmox


Download Proxmox installation file
https://www.proxmox.com/en/




First you will need to make a bootable USB disk using software called rufus


You can download the latest version of rufus using the following link.  
https://rufus.ie/en/




boot from the USB




Proxmox installer

```
nomodeset
```








<p align="right"><a href="#readme_top">back to top</a></p>

### Post Installation of Proxmox
<a id="Installing_Proxmox"></a>

Overview 







apt update && apt list --upgradable



Install one of the microcode packages according to your CPU manufacturer.



Depending on the type of CPU you have it's a good standard to instll the corret CPU drivers

+ #### Intel CPU

	apt install intel-microcode

+ #### AMD CPU

	apt install amd64-microcode

Reboot the Proxmox host.

reboot

Verify that microcode is loaded.



journalctl -k --grep="microcode updated early to"

You should see similar output like this.

Sep 10 11:38:55 pve kernel: microcode: microcode updated early to revision 0x24000024, date = 2022-09-02
Note: The date displayed does not correspond to the version of the [intel-microcode] package installed. It does show the last time Intel updated the microcode that corresponds to the specific hardware being updated.







https://community-scripts.github.io/ProxmoxVE/scripts?id=post-pve-install

```
bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/post-pve-install.sh)"
```









<p align="right"><a href="#readme_top">back to top</a></p>

## Connect a USB Drive
<a id="Proxmox_to_USB_Drive"></a>



# Coming Soon!!









<p align="right"><a href="#readme_top">back to top</a></p>

## Connect to your NAS with NFS
<a id="Proxmox_to_NFS"></a>






# Coming Soon!!

To connect to a NAS device with NFS you will have to setup some paths/directory’s this is how I have done mine but you can use your own location.   




If you need sub folders you will need to make the directory tree.
	
```
mkdir /mnt/data
```

```
mkdir /mnt/data/stream
```

```
mkdir /mnt/data/usb
```

```
mkdir /mnt/data/photos
```
```
mkdir /mnt/pve/disk4tb/frigate
```

```
mkdir /mnt/pve/disk4tb/downloads
```

The following will be needed to auto connect to you NFS shears.

``` 
nano /etc/fstab
```
NFS IP Address Share location - Proxmox map Location - Connection method - Setting

Change for you system

```
10.0.0.1:/volume1/Stream/ /mnt/data/stream nfs defaults 0 0
10.0.0.1:/volume1/Photos-Link /mnt/data/photos nfs defaults 0 0
10.0.0.1:/volume1/Downloads /mnt/data/downloads nfs defaults 0 0
```

Once you have saved your config you need to run the following.



Reload systemd:
```
systemctl daemon-reload  
```
Manually Mount shares:
```
mount -a
```







<p align="right"><a href="#readme_top">back to top</a></p>

# Installing NVIDIA Drivers <a id="NVIDIA_Drivers"></a>


Only follow this if you have a NVIDIA card to use the GPU 



+ ### Install NVIDIA Drivers on Proxmox		<a id="nvidia_drivers_proxmox"></a>

	You will need to get the latest NVIDIA drivers from the following site



	Install build packages:
	```
	apt install build-essential pve-headers-$(uname -r)
	```

	Setup Guide
	```
	apt update && apt upgrade -y && apt install pve-headers build-essential software-properties-common make nvtop htop -y
	```
	```
	update-initramfs -u
	```


	system is now ready to get the drivers

	https://www.nvidia.com/en-gb/drivers/




	<img width="1544" height="531" alt="NVIDIA_get_location" src="https://github.com/user-attachments/assets/3e99584d-27c6-4294-9e59-bf277f6af1aa" />


	https://uk.download.nvidia.com/XFree86/Linux-x86_64/570.172.08/NVIDIA-Linux-x86_64-570.172.08.run



	wget https://uk.download.nvidia.com/XFree86/Linux-x86_64/550.142/NVIDIA-Linux-x86_64-550.142.run


	Make sure you use the same file name

	```
	chmod +x NVIDIA-Linux-x86_64-550.144.03.run
	```

	```
	./NVIDIA-Linux-x86_64-550.144.03.run --dkms
	```


	nvtop

	```
	nvidia-smi  
	```


<p align="right"><a href="#readme_top">back to top</a></p>


+ ### Installing NVIDIA Drivers on your LXC's:		<a id="install-nvidia-drivers-on-proxmox"></a>

	I have the following LXC setup to use my NVIDA card (immich, Jellyfin, Plex, Ollama and Tadarr)

	```
	pct push LXC_Number NVIDIA-Linux-x86_64-550.144.03.run /root/NVIDIA-Linux-x86_64-550.144.03.run
	```

	```
	chmod +x NVIDIA-Linux-x86_64-580.95.05.run
	```
	```
	./NVIDIA-Linux-x86_64-580.95.05.run --no-kernel-modules
	```

	```
	apt install gpg curl
	```


	```
	curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
	```

	```
	apt update  
	```
	```
	apt install nvidia-container-toolkit  
	```



	only if you run Docker  
	```
	nvidia-ctk runtime configure --runtime=docker
	```


	Next we will need to edit the a file
	```
	nano /etc/nvidia-container-runtime/config.toml  
	```


	#no-cgroups = false  
	to  
	no-cgroups = true  


	Next we need to find the card details so we can map this to the LXC

	```
	ls -al /dev/nvidia*
	```


	<img width="866" height="330" alt="NVIDIA_card_details" src="https://github.com/user-attachments/assets/c6d7a02a-f831-4d3c-a65a-bdd8fb697127" />



	now we need to update the config file for the LXC 



	lxc.cgroup2.devices.allow: c 195:* rwm

	```
	nano /etc/pve/lxc/105.conf
	```


	```
	lxc.cgroup2.devices.allow: c 195:* rwm
	lxc.cgroup2.devices.allow: c 234:* rwm
	lxc.cgroup2.devices.allow: c 237:* rwm
	lxc.mount.entry: /dev/nvidia0 dev/nvidia0 none bind,optional,create=file
	lxc.mount.entry: /dev/nvidiactl dev/nvidiactl none bind,optional,create=file
	lxc.mount.entry: /dev/nvidia-modeset dev/nvidia-modeset none bind,optional,create=file
	lxc.mount.entry: /dev/nvidia-uvm dev/nvidia-uvm none bind,optional,create=file
	lxc.mount.entry: /dev/nvidia-uvm-tools dev/nvidia-uvm-tools none bind,optional,create=file
	lxc.mount.entry: /dev/nvidia-caps/nvidia-cap1 dev/nvidia-caps/nvidia-cap1 none bind,optional,create=file
	lxc.mount.entry: /dev/nvidia-caps/nvidia-cap2 dev/nvidia-caps/nvidia-cap2 none bind,optional,create=file
	```




	```
	nvidia-smi  
	```








nano etc/resolv.conf




<p align="right"><a href="#readme_top">back to top</a></p>




First LXC 






<!--


-->

