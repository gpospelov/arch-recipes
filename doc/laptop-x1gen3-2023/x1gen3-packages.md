# Installing drivers and Plasma.

Here we assume Lenovo Extreme X1 Gen.3 with second Nvidia GPU on board.

## Add user rights
```
pacman -S sudo
nano /etc/sudoers
>> jamesbond ALL=(ALL) ALL
```

## Desktop

```
pacman -S xorg-server xorg-apps xorg-xinit
pacman -S nvidia nvidia-utils nvidia-settings
pacman -S plasma 
```

## sddm

```
pacman -S sddm
systemctl enable sddm.service
```

**scaling**

https://wiki.archlinux.org/title/SDDM

96 - scale 100
144 - scale 150

```
/etc/sddm.conf.d/dpi.conf

[X11]
ServerArguments=-dpi 144
```

and

```
/etc/sddm.conf.d/hidpi.conf

[Wayland]
EnableHiDPI=true

[X11]
EnableHiDPI=true
```

## ssh configuration

```
pacman -S openssh
systemctl enable sshd.service
systemctl start sshd.service
```

## Install yay

## Other packages

yay -S konsole keepassxc mc visual-studio-code-bin libreoffice-still gwenview spectacle kate

