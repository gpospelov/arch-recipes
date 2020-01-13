# Installing Arch + btrfs + dual boot with preinstalled Windows on Lenovo Thinkpad T480S.

This is an instruction how to install Arch Linux with btrfs file system
and dual boot for preinstalled Windows.
Was checked on Lenovo T480s in January, 2020. None of special Thinkpad
related tuning was required, all basic features (bluetooth, wifi, keyboard lighting and functional keys) were working.

It is assumed that there is Windows installed on system,
that there is unallocated space on drive for future Arch and that we have
Arch installation USB in hands.

## Starting installation

+ boot from usb disk
+ Check internet connection, update arch on usb

```
ping google.com
pacman -Syy
pacman -Sy archlinux-keyring
```

## Partition and format

Create file system, partitions, format to btrfs.

```
fdisk -l
fdisk /dev/nvme0n1

# Windows has already 4 partitions. Add swap and arch partitions.
# The final table should look something like

# /dev/nvme0n1p1      2048    1023999   1021952   499M Windows recovery environment
# /dev/nvme0n1p2   1024000    1228799    204800   100M EFI System
# /dev/nvme0n1p3   1228800    1261567     32768    16M Microsoft reserved
# /dev/nvme0n1p4   1261568  380112895 378851328 180.7G Microsoft basic data
# /dev/nvme0n1p5 963122224 1000215182  37092959   24G Linux filesystem
# /dev/nvme0n1p6 380113968  589829167 209715200   100G Linux filesystem

mkfs.btrfs -f -L arch /dev/nvme0n1p6

mkswap /dev/nvme0n1p7
swapon /dev/nvme0n1p7
```

## Prepare sub-volumes

```
mkdir /mnt/btrfs
mount -t btrfs /dev/nvme0n1p6  /mnt/btrfs
btrfs subvolume create /mnt/btrfs/root
btrfs subvolume create /mnt/btrfs/home
btrfs subvolume create /mnt/btrfs/snapshots
```

## Mount sub-volumes

```
cd
umount /mnt/btrfs

opt=noatime,compress=lzo,ssd,autodefrag,space_cache
mount -o subvol=root,$opt /dev/nvme0n1p6 /mnt
mkdir /mnt/home
mount -o subvol=home,$opt /dev/nvme0n1p6 /mnt/home
mkdir -p /mnt/boot/efi
mount /dev/nvme0n1p2 /mnt/boot/efi
```

## Install base packages

```
pacstrap /mnt base base-devel btrfs-progs
```

## Prepare partition table

```
genfstab -U /mnt >> /mnt/etc/fstab
```

## Edit partition table to become as

```

# /dev/sda4 LABEL=arch
UUID=e157cc8d-99db-4242-b70a-822667126a9e       /mnt/btrfs              btrfs           noatime,ssd,space_cache 0 0

# /dev/sda4 LABEL=arch
UUID=e157cc8d-99db-4242-b70a-822667126a9e       /               btrfs           noatime,compress=lzo,ssd,autodefrag,space_cache,subvol=/root    0 0

# /dev/nvme0n1p2
UUID=6025-A40E          /boot/efi       vfat            rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=iso8859-1,shortname=mixed,utf8,errors=remount-ro       0 2

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

locale-gen
nano /etc/locale.gen  # uncomment en_US.UTF-8

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
pacman -Su mkinitcpio, linux, linux-firmware
nano /etc/mkinitcpio.conf  # Remove fsck and add btrfs to HOOKS
mkinitcpio -p linux
```

## Prepare bootloader

```
pacman -S intel-ucode
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=arch
pacman -S os-prober
grub-mkconfig -o /boot/grub/grub.cfg
```

## Adding user

```
groupadd jamesbond
useradd -m -g jamesbond -G users,wheel,storage,power,network,video -s /bin/bash -c "Bond, James Bond"
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

At this point you have to be able to reboot
either to Windows or to Arch via grub.

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
pacman -S grub-btrfs
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

## Making snapshot

```
snapper list-configs
sudo snapper -c root list
sudo snapper -c root create --description "Before first global update"
```

## Rolling back snapshot

If you have killed the system and want's to get back: reboot from installation USB.

```
mount -o subvol=root /dev/sda4 /mnt
arch-chroot /mnt /bin/bash
mount /mnt/btrfs
cd /mnt/btrfs
mv root root.old

btrfs subvol snapshot /mnt/btrfs/snapshots/root_snaps/3/snapshot /mnt/btrfs/root

# reboot
```
