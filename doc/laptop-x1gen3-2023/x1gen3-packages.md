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

## ssh configuration

```
pacman -S openssh
systemctl enable sshd.service
systemctl start sshd.service
```

