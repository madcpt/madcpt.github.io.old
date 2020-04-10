---
title: Arch Linux UEFI Installation Guide
date: 2020-02-13
permalink: /posts/2020/02/arch-installation/
tags:
---

Arch Linux is one of the most popular linux distros. [Archlinux Website](https://www.archlinux.org/) is where you can find anything you need to know about archlinux. Please check out the forums and mailing lists to get yout feet wet. Also glance through the wiki if you want to learn more about Arch.

<!-- more --> 

Arch Linux is light-weighted, and you need to be really careful when manipulating the system. So I will introduce my experience of installing Arch Linux with dual boot under UEFI.

### Hardware Details

I am using [XPS 15-9550](https://www.dell.com/en-us/work/shop/dell-laptops-and-notebooks/xps-15-laptop/spd/xps-15-9550-laptop) produced by DELL. I have one 256GB SSD with Deepin system (another popular Linux Distro) pre-installed.

Before all it goes, I strongly recommend that you take a minute reading your hardware producer's guidance (if there is any). For example, DELL users should refer to [How to Install Ubuntu Linux on your Dell PC](https://www.dell.com/support/article/us/en/04/sln151664/how-to-install-ubuntu-linux-on-your-dell-pc?lang=en). Also, check the corresponding tutorial page in arch linux wiki, for me this would be [Arch Linux Wiki: XPS 15 9550](https://wiki.archlinux.org/index.php/Dell_XPS_15_9550)

### Pre-Installation

+ If you are using UEFI for booting, make sure that Secure Boot is disabled. 
+ Set SATA mode to AHCI (there have been many discussion about which SATA mode is superior and my default setting was RAID, but I would just stick to AHCI for now).
+ Making a bootable USB flash drives with the newest arch linux image. I recommend `dd` for this step. For example, you can install the arch linux in USB by simply using `dd bs=4M if=path/to/archlinux.iso of=/dev/sdx status=progress oflag=sync`.
+ Boot the live environment with USB.

### During Installation

The Arch Linux Community provides an elaborate [installation guidance](https://wiki.archlinux.org/index.php/Installation_guide), and in my experience it's the best way to follow the guidance step-by-step. Please refer to the official guidance and the [arch linux forums](https://bbs.archlinux.org/) for specific questions! But still, I have some additional information for reference.

#### Dual Boot

Particularly, I already had another Linux Distro installed in my laptop, and I preferred not to removing the original OS. I made possible dual booting with GRUB, which is originally planted in my machine when installing the first Linux Distro.

I had a SSD, instead of HDD, in my machine, so the hard drive mounting settings may be a little bit different. In most guidance you would find something like `/dev/sda1` for device location, while in my case I need to specify `/dev/nvme0n1px` in disk partition.

Steps for dual booting is quite simple:

+ mount the new system into EFI partition with `mount /dev/nvme0n1px /mnt/boot/efi`;
+ chroot into the new system with `chroot /mnt`;
+ update GRUB with `update-grub` so that GRUB was not removed or reinstalled, everything should work just fine with the first OS in my machine (if anything goes wrong, you can always reinstall GRUB with `grub-install`);
+ let's reboot!

#### Shared Swap Partition (Optional)

Since I am going to have two Linux distros in my laptop, I was wondering if I can make them share the same swap, and it seems to work! However, **DO NOT USE hibernate** because for hibernate it is mandatory to use swap partition, which could be extremely dangerous if you boot another system!

![](https://i.imgur.com/PSLDCzJ.png)


### Post Installation

#### Desktop Environment
Most people would recommend KDE or GNOME, but I am a super fan of [Deepin Desktop Environment (DDE)](https://wiki.archlinux.org/index.php/Deepin_Desktop_Environment)!

#### Useful Repositories
+ [AUR](https://wiki.archlinux.org/index.php/Arch_User_Repository)
+ [Archlinuxcn](https://www.archlinuxcn.org/)

#### Wine Applications
+ [deepin-wechat](https://aur.archlinux.org/packages/deepin-wine-wechat/)
+ [deepin-tim](https://aur.archlinux.org/packages/deepin.com.qq.office/)


