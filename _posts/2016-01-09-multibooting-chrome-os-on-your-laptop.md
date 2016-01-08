---
layout: post
title: "Multibooting Chrome OS on your Laptop"
description: "How to dual/triple/quadruple boot Chrome OS with Windows, Linux and/or Mac"
headline: "Multibooting Chrome OS on your Laptop"
modified: 2016-01-09 01:23:02 +0530
category: [Linux, Multiboot, Chrome, Computers]
tags: [linux, windows, multiboot, dual boot, chrome, chromium, chrome os]
imagefeature:
mathjax:
chart:
comments: true
featured: false
---
Other than the conventionally popular Windows and Mac OS, (and, among developers and geeks, Linux), one of the most popular operating systems for Netbooks and lightweight convertibles today is - Chrome OS. But if you have landed on this page, I am sure you already know that.

Now the problem is, Chrome OS was designed to be used only with certain type of hardware - low powered netbooks, so Google took a certain route, in which Chrome OS officially is available only on **Chromebooks** - certain types of netbooks that come preloaded with Chrome OS - and have to be certified by Google. Kind of like Mac OSX which you cannot use unless you use an iMac or a Macbook.

But ofcourse, you can use Chrome OS just as well on your own Laptop, if you so wish to, because, Chrome OS is actually based on - Chromium OS - the underlying opensource project.

There happens to be a kind guy [arnoldthebat](http://twitter.com/arnoldthebat), who makes regular releases of builds of Chromium OS that are pretty much compatible with almost all x86 (i386, or 32 bit) and amd64 (64 bit Intel or AMD) CPU based laptops. Most of your generic hardware like touchpads, speakers should work, and if you're not too unlucky(read have a Broadcomm card), most probably your WiFi  will work too. You can catch hold of his builds here - http://chromium.arnoldthebat.co.uk/
I'd recommend using the latest ***special*** build as those ones have the widest hardware support.

After you download the .img file, you can create a bootable USB on Linux using this command (given your USB drive is on /dev/sdb) -    
```bash
dd if=/path/to/downloaded/img of=/dev/sdb
```
You should boot from your USB drive (you'll need to enable legacy support if yours is a post-UEFI era BIOS), and play with the OS to make sure everything works, and if you like it, and want to permanently install it to your laptop, read on.

So, if you want Chromium OS to be the only operating system in your laptop, you can just press `Ctrl` + `Alt` `F2`, login with user:`chronos` password:`password` and run the command `sudo install`
*This **will** format your hard drive*

But instead if you want to install Chromium OS as an additional OS to your laptop then there is way for that too. Here are some necessary conditions though -
 1. You'll need to make 2 partitions, one 2GB and another at least 4GB in size. Your HDD should have that space, or you should create that by shrinking any existing partitions.
 2. You need to be using GRUB as your bootloader. If you are running Linux with Windows/Mac I am assuming you already are running GRUB. If not, then you can still install GRUB - a step that you [can figure out yourself](http://google.com/search?q=install+grub+bootloader)

First take a look at the bootable USB you made out of the Chromium OS disk. It will have two important partitions called - **ROOT_A** and **STATE**.
We need to create corresponding partitions on your hard disk (these can be larger size than the ones on the USB, a larger **STATE** partition gives you more local storage space. A larger **ROOT-A** partition is mostly of no use, as this partition's content won't change in the future).

Let's consider we made the following partitions -    
 * /dev/sda7  - ext2 - 2.15GB
 * /dev/sda8 - ext4 - 6GB

Now you should copy the corresponding partitions from your USB drive to your own hard disk using `dd`    

```bash
#For copying ROOT-A
dd if=/dev/sdb3 of=/dev/sda7
#For copying STATE
dd if=/dev/sdb1 of=/dev/sda8
```

Next step would be to add a corresponding boot entry for Chromium OS to your GRUB. For that edit `/etc/grub.d/40_custom` and add the following   

```
menuentry "Chrome OS" {
 insmod part_gpt
 insmod ext2
 set root=(hd0,gpt7)
 linux /boot/vmlinuz root=/dev/sda7 init=/sbin/init rootwait rw noresume console=tty2 i915.modeset=1 loglevel=1 quiet noinitrd tpm_tis.force=1
}
```
This is considering your's is a GPT partitioned HDD. If not, remove the `insmod part_gpt` line, and change the root to `(hd0,X)` where X = root partition number.
After this run `sudo update-grub2` to generate the grub.cfg.

Now there is tiny problem with Chromium OS - **It expects the STATE partition to be on /dev/sda1** This is hardcoded into the OS. Usually since your `/dev/sda1` would already be occupied by some OS (Windows or Linux), you cannot install the Chromium STATE partition there. For that we need to make Chromium expect STATE somewhere else.
Mount your ROOT-A partition, and edit the file `/sbin/chromeos_startup` on it.
Change the following line -    

```
STATE_DEV=${ROOTDEV_TYPE}1
```
Change the `1` at the end to the whatever partition you have put STATE on. If it was on `/dev/sda8`, change the **1** to **8**.

Also in the same file find and comment out this line   

```
mount -n -t ${FS_FORMAT_OEM} -o ${OEM_FLAGS} ${OEM_DEV} /usr/share/oem
```
This is because we have not made an OEM partition, so we can't mount it. We do not need the OEM partition to make Chromium OS work.

Finally you should be ready to reboot into Chromium OS be selecting it from the GRUB boot menu.
