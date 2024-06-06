# Installing Arch + ext4 Lenovo Thinkpad X1gen3.

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
fdisk /dev/nvme1n1

/dev/nvme1n1p1     2048    411647    409600   200M EFI System
/dev/nvme1n1p2   411648  33966079  33554432    16G Linux swap
/dev/nvme1n1p3 33966080 500117503 466151424 222.3G Linux filesystem

mkfs.fat -F 32 /dev/nvme1n1p1
mkswap /dev/nvme1n1p2
swapon /dev/nvme1n1p2
mkfs.ext4 /dev/nvme1n1p3
```

## Mount volumes

```
mount /dev/nvme1n1p3 /mnt
mkdir -p /mnt/boot/efi

mount /dev/nvme0n1p2 /mnt/boot/efi
mount --mkdir /dev/efi_system_partition /mnt/boot
```

## Install base packages, prepare partition table

```

pacstrap -K /mnt base linux linux-firmware
genfstab -U /mnt >> /mnt/etc/fstab
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

echo 'supmvvm' > /etc/hostname
```

## Network

```
nano /etc/hosts
127.0.0.1	localhost
::1		localhost
127.0.1.1	supmvvm.localdomain	supmvvm

```

## Set root password

```
passwd
```

## Mkinit, loader

```
mkinitcpio -P

pacman -S intel-ucode
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

