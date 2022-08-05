---
title: "Setting up Arch Linux with BTRFS, Encryption, and Swap"
date: 2022-08-04
modified: 2022-08-05T10:50:00-05:00
tags:
  - Arch Linux
header:
  image: /assets/images/headers/2022-08-04-arch-encrypt-swap-header.png
  teaser: /assets/images/headers/2022-08-04-arch-encrypt-swap-header.png
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
    gdisk /dev/nvme0n1
    ```

3. Create a FAT32 system on the EFI partition, e.g. `nvme0n1p1` or `sda1`.

    ```shell
    mkfs.fat -F 32 /dev/nvme0n1p1
    ```

4. Initialize encryption on the Linux filesystem partition. You'll be prompted to enter a password to encrypt your system twice.

    ```shell
    cryptsetup -y -v luksFormat /dev/nvme0n1p2
    cryptsetup open /dev/nvme0n1p2 cryptroot
    ```

5. Create a BTRFS file system on the newly encrypted Linux filesystem partition and mount it.

    ```shell
    mkfs.btrfs /dev/mapper/cryptroot
    mount /dev/mapper/cryptroot /mnt
    ```

6. Change into `/mnt` and create subvolumes for `root`, `home`, `snapshots`, `var/log`, and `swap`. The names of the subvolumes here are [recommended for use with `snapper`](https://wiki.archlinux.org/title/snapper#Suggested_filesystem_layout), a program that will help automate snapshots for us. I add an additional subvolume for our swap file since it needs to be on a non-snapshotted subvolume.

    ```shell
    cd /mnt
    btrfs subvolume create @
    btrfs subvolume create @home
    btrfs subvolume create @snapshots
    btrfs subvolume create @var_log
    btrfs subvolume create @swap
    ```

7. Unmount `cryptroot` and then remount subvolumes and boot partition. For BTRFS mount options, I use `noatime` (disables file writing access times to improve performance), `zstd` (file compression), and `space_cache=v2` (new implementation of a free space tree for BTRFS cache).

    ```shell
    cd
    umount /mnt
    mount -o noatime,compress=zstd,space_cache=v2,subvol=@ /dev/mapper/cryptroot /mnt
    mkdir -p /mnt/{boot,home,.snapshots,var/log,swap}
    mount -o noatime,compress=zstd,space_cache=v2,subvol=@home /dev/mapper/cryptroot /mnt/home
    mount -o noatime,compress=zstd,space_cache=v2,subvol=@snapshots /dev/mapper/cryptroot /mnt/.snapshots
    mount -o noatime,compress=zstd,space_cache=v2,subvol=@var_log /dev/mapper/cryptroot /mnt/var/log
    mount -o noatime,subvol=@swap /dev/mapper/cryptroot /mnt/swap
    mount /dev/nvme0n1p1 /mnt/boot
    ```

8. Make sure `swap` subvolume is not being snapshotted, then create a swap file (my rule of thumb is 2 GB for VMs or 0.5 times the system RAM in GB; change `count=` parameter to your swap file size, in MB) and turn it on.

    ```shell
    cd /mnt/swap
    chattr +C /mnt/swap
    dd if=/dev/zero of=./swapfile bs=1M count=4096 status=progress
    chmod 0600 ./swapfile
    mkswap -U clear ./swapfile
    swapon ./swapfile
    ```

9. From here you can continue with the official install guide by installing the necessary base packages on your system. I use the packages below. Be sure to replace `intel-ucode` with `amd-ucode` if using an AMD processor. 

    ```shell
    cd
    pacstrap /mnt base base-devel linux linux-firmware intel-ucode zsh zsh-completions sudo vim git btrfs-progs dosfstools e2fsprogs exfat-utils ntfs-3g smartmontools networkmanager dialog man-db man-pages texinfo
    ```

10. Continue with the install guide by generating your `fstab`, `arch-chroot`ing into `/mnt`, and setting time zone, system locale, hostname, and root password.
11. For the boot manager, I use `grub` because there are some packages that play nicely with restoring snapshots from the GRUB interface that we will see later.

    ```shell
    pacman -S grub efibootmgr
    ```

12. Edit `/etc/mkinitcpio.conf`. Add `btrfs` to `MODULES` and make sure `HOOKS` looks like the following:

    ```
    HOOKS=(base udev autodetect modconf block encrypt filesystems keyboard fsck)
    ```

13. Regenerate your `initramfs`.

    ```shell
    mkinitcpio -P
    ```

14. Generate your bootloader.

    ```shell
    grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
    ```

15. Obtain the UUID of the partition that the system was installed on. You can see this with the `blkid` command (don't use `cryptroot`; use the UUID of e.g. `/dev/nvme0n1p2` or `/dev/sda2`). It should look something like `5f5b0b02-318c-4980-bcb5-793d44fe4387`.

16. Edit `/etc/default/grub` and at the line beginning with `GRUB_CMDLINE_LINUX_DEFAULT`, insert at the end with a space after `quiet`. Replace the UUID with the one on your system partition.

    ```
    root=/dev/mapper/cryptroot cryptdevice=UUID=5f5b0b02-318c-4980-bcb5-793d44fe4387:cryptroot 
    ```

17. Regenerate `grub.cfg`

    ```shell
    grub-mkconfig -o /boot/grub/grub.cfg
    ```

18. At this stage you can continue with the official install guide to enable networking, add a user, and enable `sudo`.
19. Reboot the system and remove the Arch Linux install USB. You should be prompted to enter a password for your encrypted partition before logging in! If not, something in the process didn't go quite right (e.g. UUID wasn't typed correctly). With the install USB, you can remount all the partitions and `arch-chroot` to fix this.
20. We're now going to set up `snapper`.

    ```shell
    sudo pacman -S snapper
    ```

21. When we create a snapshot configuration with `snapper`, it will also create a subvolume and folder called `/.snapshots`, even though we created the `snapshots` subvolume mounted on `/.snapshots` earlier. To remedy this we will: 
    1. Unmount our `snapshots` subvolume mounted at `/.snapshots` and delete the `/.snapshots` folder. 

        ```shell
        sudo umount /.snapshots
        sudo rm -r /.snapshots
        ```
    2. Create our `snapper` config and let `snapper` do its weird thing. 
    
        ```shell
        sudo snapper -c root create-config /
        ```

    3. Confirm that `snapper` did its weird thing and delete the newly created subvolume and folder.

        ```shell
        sudo btrfs subvolume list /
        sudo btrfs subvolume delete /.snapshots
        ```

    4. Recreate the folder and remount it to our `snapshots` subvolume.

        ```shell
        sudo mkdir /.snapshots
        sudo mount -a
        ```

22. Let's give read, write and execute access from our snapshots (so we can access them).

    ```shell
    sudo chmod 750 /.snapshots
    ```

23. Edit `snapper` config at `/etc/snapper/configs/root`, add ALLOW_USERS=”[your username here, replace the brackets too]” and change frequency to that [listed on the `snapper` wiki page](https://wiki.archlinux.org/title/snapper#Set_snapshot_limits). 
24. Enable the `snapper` services. If on an SSD, enable TRIM.

    ```shell
    sudo systemctl enable --now snapper-timeline.timer
    sudo systemctl enable --now snapper-cleanup.timer
    sudo systemctl enable fstrim.timer
    ```

25. Home stretch! Next I'll install `yay`, an AUR helper. It helps us install and manage packages from the AUR.

    ```shell
    git clone https://aur.archlinux.org/yay
    cd yay
    makepkg -si PKGBUILD
    ```

26. Install `snap-pac-grub` and `rsync`. `snap-pac-grub` takes a system snapshot after every single install with `pacman` (so we can revert if an upgrade breaks something) and then makes these snapshots accessible and bootable from GRUB. Since our boot partition is not being snapshotted (only root), I'll use `rsync` to copy the files of `/boot` during every Linux kernel upgrade. 

    ```shell
    yay -S snap-pac-grub rsync
    ```

27. Edit `/etc/mkinitcpio.conf` and add `grub-btrfs-overlayfs` to the end of `HOOKS`, and then regenerate your `initramfs`. We did something like this earlier. This step enables booting from our snapshots from GRUB.

    ```shell
    sudo mkinitcpio -P
    ```

28. Sync `/boot` to `/.bootbackup` because `snapper` cannot backup `/boot`. 

    ```shell
    sudo rsync -a --delete /boot /.bootbackup
    ```
29. Create the folder `/etc/pacman.d/hooks` and in it, create a hook which will sync `/boot` whenver there is a kernel update.

    ```shell
    sudo mkdir /etc/pacman.d/hooks
    sudo vim /etc/pacman.d/hooks/50-bootbackup.hook
    ```

    ```
    [Trigger]
    Operation = Upgrade
    Operation = Install
    Operation = Remove
    Type = Path
    Target = usr/lib/modules/*/vmlinuz
    
    [Action]
    Depends = rsync
    Description = Backing up /boot...
    When = PostTransaction
    Exec = /usr/bin/rsync -a --delete /boot /.bootbackup
    ```

30. Reboot and make an image for rollback to this base system in case something bad goes wrong.

    ```shell
    sudo snapper -c root create --description “Clean BTRFS install with Snapper”
    ```

31. Finished! Phew, that was a handful. But this establishes an excellent base system from which to work off of. You can install your favorite desktop environment/window manager and programs. I use KDE.

    ```shell
    sudo pacman -S xorg xorg-xinit xf86-input-libinput xf86-input-wacom mesa plasma-meta sddm konsole xdg-user-dirs xdg-utils tlp tlp-rdw reflector firefox dolphin ark kate kio-gdrive okular elisa vlc gwenview gimp krita kcalc spectacle kcharselect ksystemlog packagekit-qt5 kvantum noto-fonts noto-fonts-cjk noto-fonts-emoji fcitx5-mozc fcitx5-qt fcitx5-gtk fcitx5-configtool bitwarden libreoffice-still hunspell hunspell-en_us print-manager skanpage cups ghostscript gsfonts cups-pdf hplip bluez bluez-utils pulseaudio-bluetooth neofetch kdeconnect
    ```

## Resources

* [Arch Linux Install Guide](https://wiki.archlinux.org/title/Installation_guide)
* [dm-crypt Wiki Page](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system)
* [Snapper Wiki Page](https://wiki.archlinux.org/title/snapper)
* Ermanno Ferrari's [YouTube video](https://youtu.be/co5V2YmFVEE) where much of this documentation was based off of
