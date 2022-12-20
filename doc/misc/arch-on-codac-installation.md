

mkswap  /dev/nvme0n1p2
mkfs.ext4 /dev/nvme0n1p3
mkfs.fat -F 32 /dev/nvme0n1p1
mount /mnt/nvme0n1p3 /mnt

swapon /dev/nvme0n1p3 

mkdir -p /mnt/boot/efi
mount /dev/nvme0n1p1 /mnt/boot/efi

~~mount --mkdir /dev/nvme0n1p1 /mnt/boot~~

pacstrap -K /mnt base base-devel linux linux-firmware
genfstab -U /mnt >> /mnt/etc/fstab

arch-chroot /mnt

pacman -Su nano
rm /etc/localtime
ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime
hwclock --systohc --utc

nano /etc/locale.gen  # uncomment en_US.UTF-8
locale-gen

nano /etc/locale.conf
LANG=en_US.UTF-8

echo 'supmvvm' > /etc/hostname
nano /etc/hosts
127.0.0.1	localhost
::1		localhost
127.0.1.1	supmvvm.localdomain	supmvvm

passwd

mkinitcpio -P

pacman -S intel-ucode
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=arch
pacman -S os-prober
grub-mkconfig -o /boot/grub/grub.cfg

