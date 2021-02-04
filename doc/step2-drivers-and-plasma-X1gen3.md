https://wiki.archlinux.org/index.php/Lenovo_ThinkPad_X1_Extreme#PRIME_offloading

# Installing drivers and Plasma.

Here we assume Lenovo Extreme X1 Gen.3 with second Nvidia GPU on board.

## Add user rights
```
pacman -S sudo
nano /etc/sudoers
>> jamesbond ALL=(ALL) ALL
```

## Power consumption

[TLP - ArchWiki](https://wiki.archlinux.org/index.php/TLP)

```
pacman -S tlp
systemctl enable tlp.service
```

## Desktop

```
pacman -S xorg-server xorg-apps xorg-xinit
pacman -S nvidia nvidia-utils nvidia-settings nvidia-prime
pacman -S plasma
```

and optimus-manager

## Validating nvidia

```
lsmod | grep nvidia
lspci -nnk | grep '\[03'
```
## Desktop applications

If you have huge ssd...

```
pacman -S okular konsole kate mc gimp inkscape
```

## ssh configuration

```
pacman -S openssh
systemctl enable sshd.service
systemctl start sshd.service
```

## Display login manager configuration

```
pacman -S sddm
systemctl enable sddm.service
nano /usr/share/sddm/scripts/Xsetup
# add following lines to file
xrandr --setprovideroutputsource modesetting NVIDIA-0
xrandr --auto
```

*tricks*

```
journalctl -b --unit=sddm.service
systemctl status display-manager.service
```
## Bluetooth

```
pacman -S bluez
pacman -S bluez-utils
systemctl enable bluetooth.service
# restart plasma to see widget in taskbar
```

## Sound

```
pacman -S pulseaudio sof-firmware alsa-card-profiles alsa-lib alsa-plugins
```
