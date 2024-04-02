# Installing Arch + btrfs + dual boot with preinstalled Windows on Lenovo Thinkpad X1gen3.

## Starting installation

+ boot from usb disk
+ Check internet connection, update arch on usb

```
ping google.com
pacman -Syy
pacman -Sy archlinux-keyring
timedatectl set-ntp true
```

## Partition and format

Create file system, make partitions, format to btrfs.

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

mkfs.ext4 /dev/nvme0n1p6

mkswap /dev/nvme0n1p5
swapon /dev/nvme0n1p5
```

## Mount sub-volumes and Windows efi partition

```
mount /dev/nvme0n1p6 /mnt

mkdir -p /mnt/boot/efi

mount /dev/nvme0n1p2 /mnt/boot/efi
```

## Install base packages

```
pacstrap -K /mnt base base-devel linux linux-firmware
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
arch-chroot /mnt
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
ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime
hwclock --systohc --utc

nano /etc/locale.gen  # uncomment en_US.UTF-8
locale-gen

nano /etc/locale.conf
LANG=en_US.UTF-8
```

## Setup hostname

``` 
echo 'centauria' > /etc/hostname
nano /etc/hosts
127.0.0.1	localhost
::1		localhost
127.0.1.1	myhost.localdomain	centauria
```

## Set root password

```
passwd
```

## Mkinitcpio

```
pacman -Su mkinitcpio linux linux-firmware
mkinitcpio -p linux
```

## Prepare bootloader

```
pacman -S intel-ucode
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=arch
nano /etc/default/grub
# uncomment GRUB_DISABLE_OS_PROBER=false
pacman -S os-prober
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

At this point you have to be able to reboot
either to Windows or to Arch via grub.
