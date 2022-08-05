---
title: "Setting up Arch Linux with BTRFS, Encryption, and Swap"
date: 2022-08-04
tags:
  - Arch Linux
---

This blog post will demonstrate how I set up a new Arch Linux install with BTRFS, an encrypted filesystem with `dm-crypt`, and encrypted swap partition via a swap file. This post assumes some familiarity with the command line, Linux distros, and terminology around installing a Linux distro.

## Why?

* **Why Arch:** Arch Linux is a rolling release distro. That means getting to play with the latest features! You get to decide how _you_ like the software: the customization is endless. Plus, the comprehensive official Arch Linux repositories with the Arch User Repository (AUR) centralizes the installation of software. However, being on the bleeding edge and being hands-on with the software means that an update or change might accidentally break the system, which leads to the next point.  
* **Why BTRFS:** While BTRFS supports many advanced features, such as copy-on-write, one of the main advantages of this filesystem is the ability to easily create snapshots in a space-efficient manner. Snapshots can be restored from an Arch Linux install USB, which is handy especially if you can no longer log into the system. 
* **Why encryption:** Data security is important, especially on laptops. As someone who has access and may locally store protected health information (PHI), ensuring that these data cannot be accessed if my device is lost or stolen is paramount. BTRFS supports encryption with swap files.

## Installation

The steps below assumes the system you're installing Arch Linux to uses UEFI (which has been the standard for some time now), not legacy BIOS.
{: .notice--warning}

1. Boot into an Arch Linux install USB. Establish a network connection and update the system clock. If you're unsure of these steps, consult the [Arch Linux install guide](https://wiki.archlinux.org/title/Installation_guide).
2. Create a 512 MB partition for EFI and another partition (usually the rest of the disk space) for Linux filesystem. You can use several different tools for this, but I use `gdisk`. The disk name usually looks like `sda` for SATA-attached disks or `nvme0n1` for NVMEs. You can check this with `fdisk -l`. I will use `nvme0n1` from now on. For me, the first partition is `nvme0n1p1` (usually `sda1` for SATA devices) and my second partition is `nvme0n1p2` (`sda2` for SATA devices). 

    ```shell
    # gdisk /dev/nvme0n1
    ```

3. Create a FAT32 system on the EFI partition, e.g. `nvme0n1p1` or `sda1`.

    ```shell
    # mkfs.fat -F 32 /dev/nvme0n1p1
    ```

4. Initialize encryption on the Linux filesystem partition. You'll be prompted to enter a password to encrypt your system twice.

    ```shell
    # cryptsetup -y -v luksFormat /dev/nvme0n1p2
    # cryptsetup open /dev/nvme0n1p2 cryptroot
    ```

5. Create a BTRFS file system on the newly encrypted Linux filesystem partition and mount it.

    ```shell
    # mkfs.btrfs /dev/mapper/cryptroot
    # mount /dev/mapper/cryptroot /mnt
    ```

6. Change into `/mnt` and create subvolumes. The names of the subvolumes here are recommended for use with `snapper`, a program that will help automate snapshots for us.

    ```shell
    # cd /mnt
    # btrfs subvolume create @
    # btrfs subvolume create @home
    # btrfs subvolume create @snapshots
    # btrfs subvolume create @var_log
    # btrfs subvolume create @swap
    ```

7. Unmount `cryptroot` and then remount subvolumes and boot partition.

    ```shell
    # cd
    # umount /mnt
    # mount -o noatime,compress=zstd,space_cache=v2,subvol=@ /dev/mapper/cryptroot /mnt
    # mkdir -p /mnt/{boot,home,.snapshots,var/log,swap}
    # mount -o noatime,compress=zstd,space_cache=v2,subvol=@home /dev/mapper/cryptroot /mnt/home
    # mount -o noatime,compress=zstd,space_cache=v2,subvol=@snapshots /dev/mapper/cryptroot /mnt/.snapshots
    # mount -o noatime,compress=zstd,space_cache=v2,subvol=@var_log /dev/mapper/cryptroot /mnt/var/log
    # mount -o noatime,subvol=@swap /dev/mapper/cryptroot /mnt/swap
    # mount /dev/nvme0n1p1 /mnt/boot
    ```

8. Create a swap file (my rule of thumb is 2 GB for VMs or 0.5 times the system RAM in GB; change `count=` parameter to your swap file size, in MB) and turn it on.

    ```shell
    # cd /mnt/swap
    # chattr +C /mnt/swap
    # dd if=/dev/zero of=./swapfile bs=1M count=4096 status=progress
    # chmod 0600 ./swapfile
    # mkswap -U clear ./swapfile
    # swapon ./swapfile
    ```

9. 

## Resources

* [Arch Linux Install Guide](https://wiki.archlinux.org/title/Installation_guide)
* [dm-crypt Wiki Page](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system)
* [Snapper Wiki Page](https://wiki.archlinux.org/title/snapper)
:* Ermanno Ferrari's [YouTube video](https://youtu.be/co5V2YmFVEE) where much of this documentation was based off of
