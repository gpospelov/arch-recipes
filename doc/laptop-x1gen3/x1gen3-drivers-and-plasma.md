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

chmod 700 .ssh/
chmod 644 .ssh/id_rsa.pub
chmod 600 .ssh/id_rsa
chmod 644 .ssh/config 
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

## SDDM and scaling

https://wiki.archlinux.org/title/SDDM

```
/etc/sddm.conf.d/dpi.conf

[X11]
ServerArguments=-nolisten tcp -dpi 94
```

and

```
/etc/sddm.conf.d/hidpi.conf

[Wayland]
EnableHiDPI=true

[X11]
EnableHiDPI=true
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


  515  sudo pacman -R pulseeffects
  516  sudo pacman -R pipewire
  517  sudo pacman -R gst-plugin-pipewire
  518  sudo pacman -R pipewire-media-session
  519  sudo pacman -R pipewire-pulse
  520  sudo pacman -R pulseaudio-alsa
  521  sudo pacman -R plasma-pa

sudo pacman -Rcd pulseaudio

sudo pacman -Syu pipewire-pulse