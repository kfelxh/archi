# ArchLinux Installation Guide 

## Description:

This is my personal guide to install Arch Linux without any script.

**Please note that my needs are different than yours.** So, if this guide works for you, you can fork without any problem, well, with that said, let's get started!

> Official Installation Guide and ArchLinux 

[ArchLinux Offical Guide](https://wiki.archlinux.org/title/Installation_guide "ArchLinux Official")  

<!--ELIMINADO-->
## Step 1
We start our Arch Linux and select the first option.

[![Screenshot-1.png](https://i.postimg.cc/7hDTNgpq/Screenshot-1.png)](https://postimg.cc/S2TsSzSP)

## Step 2
Well, we are now in root mode to be able to install arch.

[![Screenshot-2.png](https://i.postimg.cc/mk5Br43K/Screenshot-2.png)](https://postimg.cc/wRLSWSGk)

## Step 3

Well, the first thing is to configure the keyboard layout. 
Normally it is already configured in English but since I speak Spanish I configure my keyboard layout in Latin Spanish which would be

```bash
loadkeys es
loadkeys la-latin1
```

## Step 4

Configure the internet, my recommendation is to connect your desktop or laptop computer with an Ethernet cable so as not to have so much problem in this regard (and then do it in the post installation) since what we want is to install arch.

<hr>

***Ethernet connection simply plug in your cable***

***Wifi consult the official archlinux guide:***

[Wifi ArchLinux](https://wiki.archlinux.org/title/Iwd#iwctl "ArchLinux Official Guide")

<hr>

We check if we have internet.

```bash
ping www.google.com

64 bytes from 93.184.216.34: icmp_seq=0 ttl=56 time=11.632 ms
64 bytes from 93.184.216.34: icmp_seq=1 ttl=56 time=11.726 ms
64 bytes from 93.184.216.34: icmp_seq=2 ttl=56 time=10.683 ms
```
**If this appears it is because we have internet connectivity.**


## Step 5 

We update the repositories to generate the *"core"* and *"extra"* files


```bash
sudo pacman -Syu
```


[![Sin-t-tulo.png](https://i.postimg.cc/L6yF2m4T/Sin-t-tulo.png)](https://postimg.cc/KRgH5SMk)

**It is not necessary to update, what we want is to generate these files so as not to have problems in our installation.**

## Step 6

#### Disk Partition

```bash
cfdisk
```

[![Captura-de-pantalla-2023-10-28-214537.png](https://i.postimg.cc/qRsLRQ2r/Captura-de-pantalla-2023-10-28-214537.png)](https://postimg.cc/dk0dN2WN)


> **UEFI = GPT**

> **BIOS = DOS**

<hr>

***Check if we have UEFI***

```bash
ls /sys/firmware/efi/efivars
```

> Note: If you need to change the partition table since we have UEFI, we simply run

```bash
lsblk
```
*We select our hard drive*

```bash
fdisk /dev/sda
```
We write *"g"* to change the format to GPT
and then *"w"* to write the changes.

And ready.
<hr>

#### (Create Partitions)

> Note: If you need to change the partition table since we have UEFI, we simply run

To personal taste I create four different partitions.

``/boot``

``/root``

``/home``

``/swap``

**I leave this part to personal option (you will have to investigate according to your needs)**
<hr>

> I've attached this screenshot of what my partitions look like at the end.

[![Captura-de-pantalla-2023-10-29-133110.png](https://i.postimg.cc/Jzq6zfyf/Captura-de-pantalla-2023-10-29-133110.png)](https://postimg.cc/CRRHv6s7)






