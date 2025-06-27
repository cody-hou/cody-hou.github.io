---
title: "Setting Up Arch Linux With BTRFS, Encryption, and Swap"
date: 2022-08-04
last_modified_at: 2024-10-24
tags:
  - Arch Linux
  - Guides
header:
  image: /assets/images/headers/2022-08-04-arch-encrypt-swap-header.png
  teaser: /assets/images/headers/2022-08-04-arch-encrypt-swap-header.png
---

This post was last updated at 2024-10-24. The [Arch Linux install guide](https://wiki.archlinux.org/title/Installation_guide) on the Arch wiki may have more up-to-date instructions on installation.
{: .notice--warning}

This blog post will demonstrate how I set up a new Arch Linux install with BTRFS, an encrypted filesystem with `dm-crypt`, encrypted swap partition via a swap file, and (optionally) hibernation. This post assumes some basic familiarity with the command line, Linux distros, and terminology around installing a Linux distro.

## Why?

* **Why Arch:** Arch Linux is a rolling release distro. That means getting to play with the latest features! You get to decide how _you_ like the software: the customization is endless. Plus, the comprehensive official Arch Linux repositories with the Arch User Repository (AUR) centralizes the installation of software. However, being on the bleeding edge and being hands-on with the software means that an update or change might accidentally break the system, which leads to the next point.  
* **Why BTRFS:** While BTRFS supports many advanced features, such as copy-on-write, one of the main advantages of this filesystem is the ability to easily create snapshots in a space-efficient manner. Snapshots can be restored from an Arch Linux install USB, which is handy especially if you can no longer log into the system. 
* **Why encryption:** Data security is important, especially on laptops. As someone who has access and may locally store protected health information (PHI), ensuring that these data cannot be accessed if my device is lost or stolen is paramount. BTRFS supports encryption with swap files.
* **Why hibernation:** While optional these days as flash storage is commonplace, hibernation can be an excellent option for preserving battery life while preserving the data of your open applications. As I occasionally need to switch between my primary Arch install and secondary Windows install for certain applications, hibernation allows me to quickly pause my work and start where I left off.

## Installation

The steps below assumes the system you're installing Arch Linux to uses UEFI (which has been the standard for some time now), not legacy BIOS.
{: .notice--warning}

1. Boot into an Arch Linux install USB. Verify the system is using UEFI boot mode by checking the UEFI bitness. Establish a network connection with [`iwd`](https://wiki.archlinux.org/title/iwd) if using Wi-Fi and verify the system clock.

    ~~~ bash
    cat /sys/firmware/efi/fw_platform_size
    iwctl
    timedatectl
    ~~~

2. Create a 1 GB EFI partition and a Linux filesystem partition (usually the rest of the disk space) for Linux filesystem.
    
    * The disk name usually looks like `sda` for SATA-attached disks or `nvme0n1` for NVME drives. You can check this with `fdisk -l`. I will use `nvme0n1` from now on.
    * For NVME disks, the first partition is named `nvme0n1p1` (`sda1` for SATA devices) and the second partition is `nvme0n1p2` (`sda2` for SATA devices).
    * You can use several different tools for this, including `fdisk` as we used before, but I use [`gdisk`](https://man.archlinux.org/man/gdisk.8.en) as it defaults to GPT over MBR.
    * If formatting and overwriting  an existing disk, use `wipefs --all` followed by the device name (e.g. `/dev/nvme0n1`). For good measure, I also clear the partition data and create a new GPT table in `gdisk` with the `o` option.
    * In `gdisk`, create a new partition with `n` and follow the prompts. I make my first partition the EFI partition (press Enter to accept default first sector, then type `+1G`) and the second my filesystem partition (press Enter and accept all defaults).
    * In `gdisk`, use `ef00` to set the filesystem of the first partition to EFI. 

    ~~~ bash
    gdisk /dev/nvme0n1
    ~~~

3. Create a FAT32 system on the EFI partition, e.g. `nvme0n1p1` or `sda1`.

    ~~~ bash
    mkfs.fat -F 32 /dev/nvme0n1p1
    ~~~

4. Initialize encryption on the Linux filesystem partition. You'll be prompted to enter a password to encrypt your system twice.

    ~~~ bash
    cryptsetup -y -v luksFormat /dev/nvme0n1p2
    cryptsetup open /dev/nvme0n1p2 cryptroot
    ~~~

5. Create a BTRFS file system on the newly encrypted Linux filesystem partition and mount it.

    ~~~ bash
    mkfs.btrfs /dev/mapper/cryptroot
    mount /dev/mapper/cryptroot /mnt
    ~~~

6. Change into `/mnt` and create subvolumes for `root`, `home`, `snapshots`, `var/log`, and `swap`. The names of the subvolumes here are [recommended for use with `snapper`](https://wiki.archlinux.org/title/snapper#Suggested_filesystem_layout), a program that will help automate snapshots for us. I add an additional subvolume for our swap file since it needs to be on a non-snapshotted subvolume.

    ~~~ bash
    cd /mnt
    btrfs subvolume create @
    btrfs subvolume create @home
    btrfs subvolume create @snapshots
    btrfs subvolume create @var_log
    btrfs subvolume create @swap
    ~~~

7. Unmount `cryptroot` and then remount subvolumes and boot partition. For BTRFS mount options, I use `noatime` (disables file writing access times to improve performance), `zstd` (file compression), and `space_cache=v2` (new implementation of a free space tree for BTRFS cache).

    ~~~ bash
    cd
    umount /mnt
    mount -o noatime,compress=zstd,space_cache=v2,subvol=@ /dev/mapper/cryptroot /mnt
    mkdir -p /mnt/{boot,home,.snapshots,var/log,swap}
    mount -o noatime,compress=zstd,space_cache=v2,subvol=@home /dev/mapper/cryptroot /mnt/home
    mount -o noatime,compress=zstd,space_cache=v2,subvol=@snapshots /dev/mapper/cryptroot /mnt/.snapshots
    mount -o noatime,compress=zstd,space_cache=v2,subvol=@var_log /dev/mapper/cryptroot /mnt/var/log
    mount -o noatime,subvol=@swap /dev/mapper/cryptroot /mnt/swap
    mount /dev/nvme0n1p1 /mnt/boot
    ~~~

8. Create a swap file and turn it on. My rule of thumb is 2 GB for VMs or 0.5 times the system RAM in GB; change the `size=` parameter as appropriate. If using hibernation, set the size equal to the memory of the computer.

    ~~~ bash
    cd /mnt/swap
    btrfs filesystem mkswapfile --size 4g --uuid clear ./swapfile
    swapon ./swapfile
    ~~~

9. Install the necessary base packages on your system. I use the packages below. Be sure to replace `intel-ucode` with `amd-ucode` if using an AMD processor. 

    ~~~ bash
    cd
    pacstrap -K /mnt base base-devel linux linux-firmware intel-ucode zsh zsh-completions sudo vim git btrfs-progs dosfstools e2fsprogs exfat-utils ntfs-3g smartmontools networkmanager dialog man-db man-pages texinfo pacman-contrib
    ~~~

10. Continue with the install guide by generating your `fstab`, `arch-chroot`ing into `/mnt`, and setting up time zone, system locale, hostname, networking, users, and sudo

    ~~~ bash
    genfstab -U /mnt >> /mnt/etc/fstab
    arch-chroot /mnt

    # Look in /usr/share/zoneinfo/ for your region and city!
    ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime
    hwclock --systohc

    # Edit /etc/locale.gen and uncomment necessary locales
    vim /etc/locale.gen
    locale-gen

    # Add name of machine to /etc/hostname
    vim /etc/hostname

    # Edit /etc/hosts and add uncommented info below
    # 127.0.0.1    localhost
    # ::1          localhost
    # 127.0.1.1    [replace all this text and brackets with name of machine]
    vim /etc/hosts

    # Set root password
    passwd

    # Set username and password. Also add to the wheel group for sudo. I use the zsh shell.
    useradd -m -G wheel -s /bin/zsh cody
    passwd cody
    chfn -f “Cody Hou” cody

    # Uncomment wheel line using vim. Use EDITOR=nano if preferred.
    visudo
    ~~~

11. For the boot manager, I use `grub` because there are some packages that play nicely with restoring snapshots from the GRUB interface that we will see later.

    ~~~ bash
    pacman -S grub efibootmgr
    ~~~

12. Edit `/etc/mkinitcpio.conf`. Add `btrfs` to `MODULES` and add `encrypt` to `HOOKS`, as in the following example. If using hibernate, also add `resume` following `filesystems`.

    ~~~
    HOOKS=(base udev autodetect microcode modconf kms keyboard keymap consolefont block encrypt filesystems fsck)
    ~~~

13. Regenerate your `initramfs`.

    ~~~ bash
    mkinitcpio -P
    ~~~

14. Generate your bootloader.

    ~~~ bash
    grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
    ~~~

15. Obtain the UUID of the partition that the system was installed on. You can see this with the `blkid` command (don't use `cryptroot`; use the UUID of e.g. `/dev/nvme0n1p2` or `/dev/sda2`). It should look something like `5f5b0b02-318c-4980-bcb5-793d44fe4387`.

16. Edit `/etc/default/grub` and at the line beginning with `GRUB_CMDLINE_LINUX_DEFAULT`, insert at the end with a space after `quiet`. Replace the UUID with the one on your system partition.

    ~~~
    root=/dev/mapper/cryptroot cryptdevice=UUID=5f5b0b02-318c-4980-bcb5-793d44fe4387:cryptroot 
    ~~~

17. Regenerate `grub.cfg`.

    ~~~ bash
    grub-mkconfig -o /boot/grub/grub.cfg
    ~~~

18. Add or configure any additional software. For example, I make sure to enable the following services.

    ~~~ bash
    # Networking on reboot
    systemctl enable NetworkManager.service

    # TRIM on SSDs
    systemctl enable fstrim.timer

    # pacman cache cleaner
    systemctl enable paccache.timer
    ~~~

19. Exit chroot, reboot the system, and remove the Arch Linux install USB. You should be prompted to enter a password for your encrypted partition before logging in! If not, something in the process didn't go quite right (e.g. UUID wasn't typed correctly). With the install USB, you can remount all the partitions and `arch-chroot` to fix this.

20. We're now going to set up `snapper`.

    ~~~ bash
    sudo pacman -S snapper
    ~~~

21. When we create a snapshot configuration with `snapper`, it will also create a subvolume and folder called `/.snapshots`, even though we created the `snapshots` subvolume mounted on `/.snapshots` earlier. To remedy this we will: 
    1. Unmount our `snapshots` subvolume mounted at `/.snapshots` and delete the `/.snapshots` folder. 

        ~~~ bash
        sudo umount /.snapshots
        sudo rm -r /.snapshots
        ~~~
    2. Create our `snapper` config and let `snapper` do its weird thing. 
    
        ~~~ bash
        sudo snapper -c root create-config /
        ~~~

    3. Confirm that `snapper` did its weird thing and delete the newly created subvolume and folder.

        ~~~ bash
        sudo btrfs subvolume list /
        sudo btrfs subvolume delete /.snapshots
        ~~~

    4. Recreate the folder and remount it to our `snapshots` subvolume.

        ~~~ bash
        sudo mkdir /.snapshots
        sudo mount -a
        ~~~

22. Let's give read, write and execute access from our snapshots (so we can access them).

    ~~~ bash
    sudo chmod 750 /.snapshots
    ~~~

23. Edit `snapper` config at `/etc/snapper/configs/root`, add ALLOW_USERS=”[your username here, replace the brackets too]” and change frequency to that [listed on the `snapper` wiki page](https://wiki.archlinux.org/title/snapper#Set_snapshot_limits). 
24. Enable the `snapper` services. If on an SSD, enable TRIM.

    ~~~ bash
    sudo systemctl enable --now snapper-timeline.timer
    sudo systemctl enable --now snapper-cleanup.timer
    sudo systemctl enable fstrim.timer
    ~~~

25. Home stretch! Next I'll install `yay`, an AUR helper. It helps us install and manage packages from the AUR. You can also use `paru` if you wish.

    ~~~ bash
    git clone https://aur.archlinux.org/yay
    cd yay
    makepkg -si PKGBUILD
    ~~~

26. Install `snap-pac-grub` and `rsync`. `snap-pac-grub` takes a system snapshot after every single install with `pacman` (so we can revert if an upgrade breaks something) and then makes these snapshots accessible and bootable from GRUB. Since our boot partition is not being snapshotted (only root), I'll use `rsync` to copy the files of `/boot` during every Linux kernel upgrade. 

    ~~~ bash
    yay -S snap-pac-grub rsync
    ~~~

27. Edit `/etc/mkinitcpio.conf` and add `grub-btrfs-overlayfs` to the end of `HOOKS`, and then regenerate your `initramfs`. We did something like this earlier. This step enables booting from our snapshots from GRUB.

    ~~~ bash
    sudo mkinitcpio -P
    ~~~

28. Our BTRFS snapshots will not backup `/boot` as it is on a different partition. If an update to a newer kernel version causes instability, we will want to restore the older kernel image. Create the folder `/etc/pacman.d/hooks` and in it, create a first hook which will sync `/boot` before a kernel update.

    ~~~ bash
    sudo mkdir /etc/pacman.d/hooks
    cd /etc/pacman.d/hooks
    sudo vim 0-bootbackup-pretransaction.hook
    ~~~

    ~~~
    [Trigger]
    Operation = Upgrade
    Operation = Install
    Operation = Remove
    Type = Path
    Target = /usr/lib/modules/*/vmlinuz
    
    [Action]
    Depends = rsync
    Description = Backing up /boot before committing transaction...
    When = PreTransaction
    Exec = /usr/bin/rsync -a --delete /boot /.bootbackup/pretransaction
    ~~~

29. Duplicate this hook and name it as `95-bootbackup-posttransaction.hook`, this time to copy the new kernel after updating.

    ~~~ bash
    sudo cp 0-bootbackup-pretransaction.hook /etc/pacman.d/hooks/95-bootbackup-posttransaction.hook
    ~~~

    ~~~
    [Trigger]
    Operation = Upgrade
    Operation = Install
    Operation = Remove
    Type = Path
    Target = /usr/lib/modules/*/vmlinuz
    
    [Action]
    Depends = rsync
    Description = Backing up /boot after committing transaction...
    When = PostTransaction
    Exec = /usr/bin/rsync -a --delete /boot /.bootbackup/posttransaction
    ~~~

30. Reboot, and you're finished! Phew, that was a handful. But this establishes an excellent base system from which to work off of. You can install your favorite desktop environment/window manager and programs. I use KDE.

    ~~~ bash
    sudo pacman -S plasma-meta mesa sddm konsole xdg-user-dirs xdg-utils tlp reflector firefox dolphin ark kate okular elisa vlc gwenview gimp krita kcalc spectacle kcharselect ksystemlog noto-fonts noto-fonts-cjk noto-fonts-emoji fcitx5-mozc fcitx5-configtool bitwarden libreoffice-still hunspell hunspell-en_us bluez bluez-utils neofetch kdeconnect
    ~~~

## Resources

* [Arch Linux Install Guide](https://wiki.archlinux.org/title/Installation_guide)
* [dm-crypt Wiki Page](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system)
* [Snapper Wiki Page](https://wiki.archlinux.org/title/snapper)
* Ermanno Ferrari's [YouTube video](https://youtu.be/co5V2YmFVEE) where much of this documentation was based off of
