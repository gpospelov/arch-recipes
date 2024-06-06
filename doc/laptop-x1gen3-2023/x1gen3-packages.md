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
pacman -S nvidia nvidia-utils nvidia-settings
pacman -S plasma 
```

## NTFS and exFat

```
pacman -S ntfs-3g exfat-utils
```

## sddm

```
pacman -S sddm
systemctl enable sddm.service
```

*tricks*

```
journalctl -b --unit=sddm.service
systemctl status display-manager.service
```

**scaling**

https://wiki.archlinux.org/title/SDDM

- 96, scale 100%
- 144, scale 150%

```
/etc/sddm.conf.d/dpi.conf

[X11]
ServerArguments=-dpi 96
```

and

```
/etc/sddm.conf.d/hidpi.conf

[Wayland]
EnableHiDPI=true

[X11]
EnableHiDPI=true
```

With `ServerArguments=-dpi 96` I set System Settings / Display to the scale 150

## ssh configuration

```
pacman -S openssh
systemctl enable sshd.service
systemctl start sshd.service

# permissions for user .ssh
chmod 700 .ssh/
chmod 644 .ssh/id_rsa.pub 
chmod 600 .ssh/id_rsa
```

## Basic development

```
packman -S git base-devel
```

## Install yay

## Rslsync

```
yay -S rslsync

rslsync --dump-sample-config > ~/.config/rslsync/rslsync.conf
> edit device_name and storage_path  (/home/jamesbond/.sync)

systemctl --user start rslsync
systemctl --user enable rslsync 

```

## Password for skypeforlinux, slack, etc

```
pacman -S gnome-keyring seahorse

# run seahorse, it will show one existing keying, opened already, with the name 'login'.
# With the right mouse button mark it as 'Default'
# Then all applications, supporting keyring, will store their passwords in login keyring.
```

## Firefox

```
packman -S firefox

# install plugins privacy-badget, uBlock origin

# Firefox font size
> firefox about:config
> layout.css.devPixelsPerPx -> -1.0 to 1.3
```

## Other small utilities

```
pacman -S htop tree mlocate zip unzip wget
sudo /sbin/updatedb  # for locate
```

## VisualStudio

https://aur.archlinux.org/yay.git
yay -S visual-studio-code-bin


### Extensions

- Markdown All In One, Yu Zhang
- Markdown PDF, yzane

- C/C++ IntelliSence Microsoft
- Clang-Format, Xavier Hellauer
- Clang-Tidy, notskm
- CMake, twxs
- CMake Tools, Microsoft
- Pylance Microsoft
- Python, Microsoft
- Remote-SSH, Microsoft
- Remote Explorer Microsoft
- XML tools (Josh Johnson)

Dokcer (Microsoft), GitGraph (mhutchie), GitKraken Authentification (GitKraken), IntelliCode (Microsoft), LatexWorkshop (James Yu), Markdown Lint (David Anson)



## Other packages

yay -S konsole okular smartgit keepassxc mc visual-studio-code-bin libreoffice-still gwenview spectacle kate gimp inkscape

## Texlive

```
yay -S ttf-ms-fonts
pacman -Su texlive-core texlive-latexextra texlive-xetex texlive-plaingeneric texlive-fontsrecommended  texlive-fontsextra 
```

## COnfiguring konsole

Settings / Toolbar shown / Main toolbar -> hide

## Additional keyboard layout

```
Settings / Input Devices / Keyboard -> Layout
Main shortcut Win+Space
```

## Music player

pacman -S elisa

## Tuning Midnight Commander

```
# Press F9 to activate top menu, in "Options/Configuration" remove crosses from "Use internal editor/viewer"
# Don't forget to save configuration

nano .bashrc
export EDITOR=nano
export PAGER=less
```

## Tuning plasma

### System load viewer

- Widget from KDE store
- ksysguard

### Minor KDE fixes

```
# Hidden pager
> Workspace Benabior/Virtual desktop -> add 6 desktops

# Shorcuts for desktop switching
> Shortcuts > Global shortcuts > KWin 

# Change backgrount for SDDM manager
> Settings /Startup And Shutdown / Login Screen / Right side of the screen (Background) 

# Disable instant messaging
> Settings / Startup And Shutdown / Background service / (disable) Accounts

# Do not let windows appear at startup
> Settings /Startup And Shutdown / Desktop Session / (disable) Start with an empty session

# Firefox font size
> firefox about:config
> layout.css.devToPixes -> -1.0 to 1.3

```

## Dolphin file manager

```
Configure Dolphin / General / Miscellaneous - Switch between split views panes with tab key
System Settings / Workspace Behaviour / General Behaviour - Select items by double clicking
```

## Development

cmake benchmark gtest libxml2 gdb valgrind gperf

## Qt

pacman -S qt5 qtcreator

Scaling:

- Start typing Qt creator in start menu
- With Right mouse button edit application
- In the field environment variable put
- QT_SCALE_FACTOR=1 QT_AUTO_SCREEN_SCALE_FACTOR=0 QT_SCREEN_SCALE_FACTORS=1.5


## Optimal Qt creator HighDPI settings

** Variant 1**

- sddm `ServerArguments=-dpi 96`
- Display setting Global Scale 150%
- qtcreator editor font 10
- Environment: QT_SCALE_FACTOR=1 QT_AUTO_SCREEN_SCALE_FACTOR=0 QT_SCREEN_SCALE_FACTORS=1.0 in application start icon menu
- Arguments: QT_SCREEN_SCALE_FACTORS=1.0 qtcreator %F

- firefox layout.css.devPixelsPerPx -> -1.0

Reasonable menu of VSCode, SmartGit
Better toolbar layout

** Variant 2**

- sddm `ServerArguments=-dpi 144`
- Display setting Global Scale 100%
- QT_SCALE_FACTOR=1 QT_AUTO_SCREEN_SCALE_FACTOR=0 QT_SCREEN_SCALE_FACTORS=1.5
- firefox layout.css.devPixelsPerPx -> -1.0 -> 1.3

too small menu of VSCode

