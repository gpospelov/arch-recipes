# Installing Arch + ext4 AMD Ryzen 9950x

Gigabyte B650M Aurus Elixate Ax Rev 1.1

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

```
fdisk -l

# Disk 512 Gb
fdisk /dev/nvme0n1

/dev/nvme0n1p1   512G  Linux swap
mkfs.ext4 /dev/nvme0n1p1

# Disk 2Tb
fdisk /dev/nvme1n1

/dev/nvme1n1p1   1G EFI System
/dev/nvme1n1p2   96G  Linux swap
/dev/nvme1n1p3   256G Linux filesystem
/dev/nvme1n1p4   1.5T Linux

mkfs.ext4 /dev/nvme1n1p3
mkfs.ext4 /dev/nvme1n1p4

mkfs.fat -F 32 /dev/nvme0n1p1
mkswap /dev/nvme1n1p2
swapon /dev/nvme1n1p2
```

## Mount volumes

```
mkdir -p /mnt/boot
mount /dev/nvme1n1p1 /mnt/boot
mount /dev/nvme1n1p3 /mnt

mkdir -p /mnt/boot
mount /dev/nvme1n1p5 /mnt/home

mkdir -p /mnt/mnt/space
mount /dev/nvme0n1p1 /mnt/mnt/space/home

```

## Install base packages, prepare partition table

```
pacstrap -K /mnt base linux linux-firmware amd-ucode
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt

```

## Time, locale

```
pacman -Su nano less


ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime
hwclock --systohc --utc

nano /etc/locale.gen  # uncomment en_US.UTF-8
locale-gen

nano /etc/locale.conf
LANG=en_US.UTF-8

echo 'oblepiha' > /etc/hostname
```

## Network

```
nano /etc/hosts
127.0.0.1 localhost
::1 localhost
127.0.1.1 centauria.localdomain localhost
```

## Set root password

```
passwd
```

## Mkinit, loader

```
mkinitcpio -P

pacman -S amd-ucode
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=arch
pacman -S os-prober
grub-mkconfig -o /boot/grub/grub.cfg
```

# Adding user

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

