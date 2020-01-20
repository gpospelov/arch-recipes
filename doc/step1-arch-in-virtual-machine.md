# Installing Arch + btrfs on virtual machine.

Here we are installing Arch Linux in virtual machine (Oracle VM virtual box) installed in another Arch Linux.

## Download Arch image

+ Download latest image and checksum file [from one of the mirror](https://www.archlinux.org/download/).
+ Assuming two files `archlinux-2020.01.01-x86_64.iso.sig` and `archlinux-2020.01.01-x86_64.iso` in same directory
+ Check signature

```
pacman-key -v archlinux-2020.01.01-x86_64.iso.sig
```

## Starting installation

+ Start virtual box, create new machine, boot from downloaded image
+ Check internet connection, update arch on usb

```
ping google.com
pacman -Syy
pacman -Sy archlinux-keyring
```

## Partition and format

Create file system, make partitions, format to btrfs.

```
fdisk -l
fdisk /dev/sda

# create new GPT partition table using 'g'
# create partitions 'n' to have something like
# /dev/sda1  1M      bios boot (type 4)
# /dev/sda2  8G      Linux Filesystem
# /dev/sda3  32.6G   Linux Filesystem

mkfs.btrfs -f -L arch /dev/sda3

mkswap /dev/sda2
swapon /dev/sda2
```

## Prepare sub-volumes

```
mkdir /mnt/btrfs
mount -t btrfs /dev/sda3  /mnt/btrfs
btrfs subvolume create /mnt/btrfs/root
btrfs subvolume create /mnt/btrfs/home
btrfs subvolume create /mnt/btrfs/snapshots
```

## Mount sub-volumes

```
cd
umount /mnt/btrfs

opt=noatime,compress=lzo,ssd,autodefrag,space_cache
mount -o subvol=root,$opt /dev/sda3 /mnt
mkdir /mnt/home
mount -o subvol=home,$opt /dev/sda3 /mnt/home
```

## install base packages

```
pacstrap /mnt base base-devel btrfs-progs
```

## Prepare partition table

```
genfstab -U /mnt >> /mnt/etc/fstab
```

## Edit partition table to become as

```
# Static information about the filesystems.
# See fstab(5) for details.

# <file system> <dir> <type> <options> <dump> <pass>

# /dev/sda4 LABEL=arch
UUID=e157cc8d-99db-4242-b70a-822667126a9e       /mnt/btrfs              btrfs           noatime,ssd,space_cache 0 0

# /dev/sda4 LABEL=arch
UUID=e157cc8d-99db-4242-b70a-822667126a9e       /               btrfs           noatime,compress=lzo,ssd,autodefrag,space_cache,subvol=/root    0 0

# /dev/sda4 LABEL=arch
UUID=e157cc8d-99db-4242-b70a-822667126a9e       /home           btrfs           noatime,compress=lzo,ssd,autodefrag,space_cache,subvol=/home    0 0

# /dev/sda3
UUID=4b92bcfb-41f2-469d-84fa-c2be52c5e88a       none            swap            defaults,pri=-2 0 0
```

## Change root

```
arch-chroot /mnt /bin/bash
```

## Install editor

By default Arch come in 2020 without much packages. You will have to install 
quite a lot, even editors, mkinitcpio etc...

```
pacman -Su nano
```

## Set locale

```
rm /etc/localtime
ln -s /usr/share/zoneinfo/Europe/Berlin /etc/localtime
hwclock --systohc --utc

nano /etc/locale.gen  # uncomment en_US.UTF-8
locale-gen

nano /etc/locale.conf
LANG=en_US.UTF-8
```

## Setup hostname

```
echo 'myhost' > /etc/hostname
nano /etc/hosts
127.0.0.1	localhost
::1		localhost
127.0.1.1	myhost.localdomain	myhost
```

## Set root password

```
passwd
```

## Mkinitcpio

```
pacman -Su mkinitcpio linux linux-firmware
nano /etc/mkinitcpio.conf  # Remove fsck and add btrfs to HOOKS
mkinitcpio -p linux
```

## Prepare bootloader

```
pacman -S grub
grub-install --target=i386-pc /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
```

## Adding user

```
groupadd jamesbond
useradd -m -g jamesbond -G users,wheel,storage,power,network,video -s /bin/bash -c "Bond, James Bond" jamesbond
passwd jamesbond
```

## Network manager

Don't forget to make it here before the reboot.

```
pacman -S networkmanager
systemctl enable NetworkManager
```

## Finally reboot

```
exit
umount -R /mnt
reboot
```

<hr>

At this point you have to reboot successfully to your installed Arch via grub.

<hr>

Now comes snapper and btrfs final configuration.
Please note, that this is a minimal configuration to be able 
to make snapshots manually and be able
to restore system to previous state using installation USB.
I'm not registering snapshot in grub and not using advanced
snapper's roll back or automatic snapshot features.

## Snapper

```
pacman -S snapper
snapper -c root create-config /
snapper -c home create-config /home
btrfs subvolume delete /.snapshots
btrfs subvolume delete /home/.snapshots

btrfs subvolume create /mnt/btrfs/snapshots/root_snaps
btrfs subvolume create /mnt/btrfs/snapshots/home_snaps
mkdir /home/.snapshots
mkdir /.snapshots

mount -o subvol=snapshots/home_snaps /dev/sda4 /home/.snapshots
mount -o subvol=snapshots/root_snaps /dev/sda4 /.snapshots

```

Add lines to /etc/fstab and reboot

```
# /dev/sda4 LABEL=arch
UUID=e157cc8d-99db-4242-b70a-822667126a9e       /.snapshots             btrfs           noatime,compress=lzo,ssd,discard,space_cache,subvol=snapshots/root_snaps        0 0

# /dev/sda4 LABEL=arch
UUID=e157cc8d-99db-4242-b70a-822667126a9e       /home/.snapshots        btrfs           noatime,compress=lzo,ssd,discard,space_cache,subvol=snapshots/home_snaps        0 0
```

## How to make snapshot

```
snapper list-configs
sudo snapper -c root list
sudo snapper -c root create --description "Before first global update"
```

## How to roll back

If you have killed the system and want to get back: reboot from installation USB.

```
mount -o subvol=root /dev/sda4 /mnt
arch-chroot /mnt /bin/bash
mount /mnt/btrfs
cd /mnt/btrfs
mv root root.old

btrfs subvol snapshot /mnt/btrfs/snapshots/root_snaps/3/snapshot /mnt/btrfs/root

# reboot
```
