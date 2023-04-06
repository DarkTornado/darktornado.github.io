---
layout: post
title: Install ArchLinux as Multi-Boot
date: 2023-04-05 00:00:00 +0900
img: archlinux-multiboot.png
---

## Before Installation
I shrunk `Windows Partition` and I used `Ventoy` to boot from the .iso file.
You can get Arch Linux .iso file from [This Link](https://archlinux.org/download/)

![image]({{site.url}}{{site.baseurl}}/assets/images/archlinux-multiboot/0.png)

<br>

***

## Live Boot with .iso file

![image]({{site.url}}{{site.baseurl}}/assets/images/archlinux-multiboot/1.png)

Arch Linux was booted.

![image]({{site.url}}{{site.baseurl}}/assets/images/archlinux-multiboot/2.png)

<br>

***

## Connect to Internet (with Wi-Fi)

If you use `wired ethernet`, you don't need to fallow this step.
You can use `Wi-Fi` via `iwctl`

```
# iwctl
```

```
[iwctl] device list     //find network interface in your PC
[iwctl] station {interface} scan     //search Wi-Fi
[iwctl] station {interface} get-networks     //print Wi-Fi list
[iwctl] station {interface} connect "Wi-Fi Name"     //connet to the Wi-Fi
```

If the Wi-Fi has password, you need to type password after `station {interface} connect "Wi-Fi Name"` command

```
[iwctl] exit    //exit iwctl
```

<br>

***

## Create Partition for Arch Linux and mount

Find the disk which has partition that you are going to install Arch Linux  via `lsblk` command.

![image]({{site.url}}{{site.baseurl}}/assets/images/archlinux-multiboot/3.png)


<br>
```
# cfdisk {disk}
```
![image]({{site.url}}{{site.baseurl}}/assets/images/archlinux-multiboot/4.png)

<br>
Select space you shrunk, and select `New` and press `Enter` key on your keyboard.

![image]({{site.url}}{{site.baseurl}}/assets/images/archlinux-multiboot/5.png)

<br>
Input new partition's size

![image]({{site.url}}{{site.baseurl}}/assets/images/archlinux-multiboot/6.png)

<br>
Select `Write` to write.

![image]({{site.url}}{{site.baseurl}}/assets/images/archlinux-multiboot/7.png)

<br>
Check what you selected again and type `yes` if everything is correct.

![image]({{site.url}}{{site.baseurl}}/assets/images/archlinux-multiboot/8.png)

<br>
`Quit` to exit

Check Partitions via `lsblk -f` and mount
```
# mount /dev/{Partition that you are going to install Arch Linux} /mnt
# mkdir -p /mnt/boot/efi
# mount /dev/{EFI Partition} /mnt/boot
```
![image]({{site.url}}{{site.baseurl}}/assets/images/archlinux-multiboot/9.png)

<br>

***

## Install Arch Linux

```
# pacstrap /mnt base base-devel linux linux-firmware vim
```
![image]({{site.url}}{{site.baseurl}}/assets/images/archlinux-multiboot/10.png)

<br>
Create `fstab`

```
# genfstab -U /mnt >> /mnt/etc/fstab
```

<br>

***

## Enter chroot

```
# arch-chroot /mnt
```

<br>

***

## Set password for root account, Create new Account

Arch Linux's root account's default password is not `root`.
I recommend that you set a root password now.
```
# passwd root     //input password after this command
# useradd {user name}
# passwd {user name}     //input password after this command
```

<br>

***

## Install Bootloader (grub)

```
# pacman -S grub efibootmgr dosfstools openssh os-prober mtools
# grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id={name}
# grub-mkconfig -o /boot/grub/grub.cfg
```
![image]({{site.url}}{{site.baseurl}}/assets/images/archlinux-multiboot/11.png)

<br>

***

## Install NetworkManager

```
# pacman -S networkmanager
# systemctl enable NetworkManager
```

If your PC supports Cellular and you'd like to use it, also install and enable modemmanager
```
# pacman -S modemmanager
# systemctl enable ModemManager
```


<br>

***

## Install Desktop Experience

Select DE what would you like.

- xfce
```
# pacman -S xfce4 xfce4-goodies
# pacman -S lightdm lightdm-gtk-greeter
# pacman -S xorg-server
# systemctl enable lightdm
# mkdir /home/{Account Name}
# chmod 777 /home/{Account Name}
```

- Gnome
```
# pacman -S gnome
# mkdir /home/{Account Name}
# chmod 777 /home/{Account Name}
# systemctl enable gdm.service
```

- KDE Plasma
```
# pacman -S plasma plasma-wayland-session
# pacman -S xorg-server
# mkdir /home/{Account Name}
# chmod 777 /home/{Account Name}
# systemctl enable sddm
```

- Cinnamon
```
# pacman -S cinnamon gnome-terminal
# pacman -S lightdm lightdm-gtk-greeter
# pacman -S xorg-server
# mkdir /home/{Account Name}
# chmod 777 /home/{Account Name}
# systemctl enable lightdm
```

- Mate
```
# pacman -S mate mate-extra 
# pacman -S lightdm lightdm-gtk-greeter
# pacman -S xorg-server
# mkdir /home/{Account Name}
# chmod 777 /home/{Account Name}
# systemctl enable lightdm
```

- LXDE
```
# pacman -S lxde lxdm
# mkdir /home/{Account Name}
# chmod 777 /home/{Account Name}
# systemctl enable lxdm
```

... ETC

<br>

***

## Reboot

Exit chroot and reboot
```
# exit
# reboot
```

![image]({{site.url}}{{site.baseurl}}/assets/images/archlinux-multiboot/12.png)

<br>
DE that you installed will be started automatically.

![image]({{site.url}}{{site.baseurl}}/assets/images/archlinux-multiboot/13.png)

<br>

***

## Make grub detect other OS (Multi-Boot)

Open `/usr/bin/grub-mkconfig` file and modify that part to `false`.
```
GRUB_DISABLE_OS_PROBER="false"
```
![image]({{site.url}}{{site.baseurl}}/assets/images/archlinux-multiboot/14.png)

<br>
Then, use `grub-mkconfig -o /boot/grub/grub.cfg` command to make grub detect other OS and add them to OS liut.
```
$ sudo grub-mkconfig -o /boot/grub/grub.cfg
```

<br>
If grub cannot find othe Linux you've installed with, just mount the partition thet Linux is installed and use `grub-mkconfig -o /boot/grub/grub.cfg` again.
