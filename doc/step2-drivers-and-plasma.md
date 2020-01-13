# Installing drivers and Plasma.

Here we assume Lenovo T480S which has second Nvidia GPU on board.

## Add user rights
```
nano /etc/sudoers
>> jamesbond ALL=(ALL) ALL
```

## Package installation

[TLP - ArchWiki](https://wiki.archlinux.org/index.php/TLP)

```
pacman -S tlp
systemctl enable tlp.service
systemctl enable tlp-sleep.service
```

## Desktop

```
pacman -S xorg-server xorg-apps xorg-xinit
pacman -S nvidia nvidia-utils nvidia-settings
pacman -S plasma
```

## Validating nvidia

```
lsmod | grep nvidia
lspci -nnk | grep '\[03'
```

## Bumblebee

```
pacman -S bumblebee
gpasswd -a jamesbond bumblebee
systemctl enable bumblebeed.service

# checking that it is running
optirun -b none nvidia-settings -c :8

# compare FPS for Intel .vs. Nvidia by running
pacman -S mesa-demos
glxgears -info
optirun glxgears -info
```

## Desktop application

```
pacman -S kde-applications
```

## ssh configuration

```
pacman -S openssh
systemctl enable sshd.service
systemctl start sshd.service
```

## SDDM configuration

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

## High DPI settings 

+ Application/system settings -> fonts: Force Fonts DPI: 140
+ Application/system settings -> Displays: Scale: 1.3 


## Bluetooth

```
pacman -S bluez
pacman -S bluez-utils
systemctl enable bluetooth.service
# restart plasma to see widget in taskbar
```

## Sound

```
pacman -S pulseaudio
```
