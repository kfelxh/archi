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

<hr>

We run the following command to check if our partitions were created.

```bash
fdisk -l 
```
It prints the following to us...

[![Captura-de-pantalla-2023-10-29-211742.png](https://i.postimg.cc/vBhHdcB6/Captura-de-pantalla-2023-10-29-211742.png)](https://postimg.cc/KkKyPctZ)

## Step 7

**Format partitions**

## ``/boot``

*Without EUFI (Legacy mode)*

```bash
mkfs.ext2 /dev/sda1
```
*With UEFI Activated*

```bash
mkfs.vfat -F32 /dev/sda1
```
## ``/root``

```bash
mkfs.ext4 /dev/sda2
```

## ``/home``

```bash
mkfs.ext4 /dev/sda3
```

## ``/swap``

***formatting***

```bash
mkswap /dev/sda4
```
***activating***

```bash
swapon /dev/sda4
```

**Mounting partitions**

*Without EUFI (Legacy mode)*

## ``/root``

```bash
mount /dev/sda2 /mnt
```
**creating directories inside /mnt to mount the /boot and /home partitions**

```bash
mkdir /mnt/home
```

```bash
mkdir /mnt/boot
```

**mounting both partitions in the created directories**

```bash
mount /dev/sda1 /mnt/boot
```

```bash
mount /dev/sda3 /mnt/home
```

*With UEFI*

```bash
mount /dev/sda2 /mnt
```

**creating directories inside /mnt to mount the */boot/efi* and /home partitions**

```bash
mkdir /mnt/home
```
```bash
mkdir -p /mnt/boot/efi
```

**mounting both partitions in the created directories**

```bash
mount /dev/sda1 /mnt/boot/efi
```

```bash
mount /dev/sda3 /mnt/home
```
And ready. 

<hr>

### Installing the Base System

> Packages to install 

* ``base base-devel`` 

* ``grub``

* ``os-probes``

* ``ntfs-3g``

* ``networkmanager``

> **If you are using UEFI**

* ``efibootmgr``

* ``gvfs``

* ``gvfs-afc``

* ``gvfs-mtp``

* ``xdg-user-dirs``

### For BIOS

```bash
pacstrap /mnt base base-devel grub os-prober ntfs-3g networkmanager gvfs gvfs-afc gvfs-mtp xdg-user-dirs linux linux-firmware nano dhcpcd
```

### For UEFI

```bash
pacstrap /mnt base base-devel efibootmgr os-prober ntfs-3g networkmanager grub gvfs gvfs-afc gvfs-mtp xdg-user-dirs linux linux-firmware nano dhcpcd
```

### Additional packages

**Wifi**

```bash
pacstrap /mnt networkmanager netctl wpa_supplicant dialog
```
**Touchpad (laptop)** 

```bash
pacstrap /mnt xf86-input-synaptics
```
**Generate fstab**

```bash
genfstab -pU /mnt >> /mnt/etc/fstab
```

## Step 8

### Enter the base system

```bash
arch-chroot /mnt
```

<hr>

### Configure the base system

> Creating Hostname

```bash
echo asuka > /etc/hostname
```
> Set time zone (Spain)

<!--- ln -sf /usr/share/zoneinfo/America/Guatemala /etc/localtime --->


```bash
ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime
```
> Set system language (Spanish)

```bash
echo LANG=es_GT.UTF-8 > /etc/locale.conf
```
Generate Archive

```bash
locale-gen
```

> Set Clock

```bash
hwclock -w
```
<hr>

### Install Grub

Without UEFI

```bash
grub-install /dev/sda
```

With UEFI

```bash
grub-install --efi-directory=/boot/efi --bootloader-id='Arch Linux' --target=x86_64-efi
```

Update Grub

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```
<hr>

### Root Password

```bash
passwd
```

### Create User

```bash
useradd -m -g users -G audio,lp,optical,storage,video,wheel,games,power,scanner -s /bin/bash asuka
```

```bash
passwd
```
<hr>

### Exit Chroot

```bash
exit
```
## Step 9

### unmount partitions

* /boot or /boot/efi
* /home
* /

> with UEFI

```
umount /mnt/boot/efi
```

```
umount /mnt/home
```

```
umount /mnt
```
> with BIOS

```
umount /mnt/boot/
```

```
umount /mnt/home
```

```
umount /mnt
```

## Step 10

### Reboot System

```
reboot
```

[![Captura-de-pantalla-2023-10-29-224121.png](https://i.postimg.cc/ryZzFV52/Captura-de-pantalla-2023-10-29-224121.png)](https://postimg.cc/YjgtdwXd)

**Enable Network Manager**

```
systemctl start NetworkManager.service
```

```
systemctl enable NetworkManager.service
```


**Graphics server**

```
sudo pacman -S xorg-server xorg-xinit
```

**Install Mesa**

```
sudo pacman -S mesa mesa-demos
```

**Video Drives**

*Intel Drivers*
```
sudo pacman -S xf86-video-intel intel-ucode
```
<hr>

> Note: Well, I emphasize again that this is my personal guide to installing Arch Linux without scripts. So if it meets your needs, go ahead.
>But also see the official Arch Linux guide.
>Bye

##### By Luis 🧊 


