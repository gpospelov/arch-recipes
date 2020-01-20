# Arch (post-install, packages)

## Git and firefox

```
packman -S git firefox
```

## Add color to packman

```
nano /etc/pacman.conf
> uncomment 'Color' in Misc options
```

## Minor KDE fixes

```
# Hidden pager
> Workspace Benabior/Virtual desktop -> add 6 desktops

# Shorcuts for desktop switching
> Shortcuts > Global shortcuts > KWin 

# SDDM login theme
> /usr/lib/sddm/sddm.conf.d/default

# Disable instant messaging
> Settings / Startup And Shutdown / Disable "Accounts"

# Change backgrount for SDDM manager
> Settings /Startup And Shutdown / Login Screen / Right side of the screen (Background) 

# Firefox font size
> firefox about:config
> layout.css.devToPixes -> -1.0 to 1.3

```

## Rslsync

```
# https://aur.archlinux.org/packages/rslsync/
git clone <repo>; cd <repo>; makepkg -si

rslsync --dump-sample-config > ~/.config/rslsync/rslsync.conf
> edit device_name and storage_path  (/home/jamesbond/.sync)

systemctl --user start rslsync
systemctl --user enable rslsync 

```

## KeyPass

```
pacman -S keepassxxc
```

#### Code

```
pacman -S code
```

#### Boostnote

```
# https://aur.archlinux.org/packages/boostnote
git clone <repo>; cd <repo>; makepkg -si
```

#### Smartgit

```
https://aur.archlinux.org/smartgit.git
git clone <repo>; cd <repo>; makepkg -si
```

#### PyCharm

```
pacman -S pycharm-community-edition
```

## Skype

```
https://aur.archlinux.org/skypeforlinux-stable-bin.git
git clone <repo>; cd <repo>; makepkg -si
```

## Spotify

```
curl -sS https://download.spotify.com/debian/pubkey.gpg | gpg --import
https://aur.archlinux.org/spotify.git
git clone <repo>; cd <repo>; makepkg -si
https://aur.archlinux.org/blockify.git
git clone <repo>; cd <repo>; makepkg -si
```

## C++/Python development

```
pacman -S python python-matplotlib python-pip python-numpy
pacman -S cmake boost gsl fftw libtiff valgrind
pacman -S qt5 qtcreator
```

## Printing 

```
pacman -S cups

systemctl enable  org.cups.cupsd.service
systemctl enable cups-browsed.service
```

+ Setup printers from web interface at [Home - CUPS 2.3.0](http://localhost:631)


## Font issues on Intelli-j

```
Settings > Appearance & Behavior > Appearance > Use custom font -> to size 22
Settings > Appearance & Behavior > Editor > font -> to size 22

obsolete>  -Dide.ui.scale=2.0 in Help | Edit Custom VM Options
```

## Disabling mouse button

```
xinput --list --short
xinput --list-props 18
# Start 'xev' and click button to find id

> ButtonPress event, serial 40, synthetic NO, window 0x5600001,
>     root 0x196, subw 0x0, time 41503438, (12,131), root:(1872,172),
>     state 0x0, button 8, same_screen YES

# back/forward will be buttons 8 and 9
nano .Xmodmap # add following lines
! Disable button 8 and 9
pointer = 1 2 3 4 5 6 7 0 0 10
```

## Tuning Midnight Commander

```
# Press F9 to activate top menu, in "Options/Configuration" remove crosses from "Use internal editor/viewer"
# Don't forget to save configuration

nano .bashrc
export EDITOR=nano
export PAGER=less
```

## Tensorflow

```
pacman -S python-tensorflow-opt-cuda

git clone https://aur.archlinux.org/python-keras.git
pacman -S python-keras-applications python-keras-preprocessing
makepkg -si
```

## Virtualbox

```
# And enable "Intel Virtualization technology + Intel VT-d feature in BIOS of laptop

pacman -S linux-headers virtualbox
```

## Spellcheck

```
pacman -Su hunspell hunspell-en_US
```

## Thunderbird

```
pacman -S thunderbird hunspell
```

When add new Exchange account, type in your mail and password,
Click "Continue" and "Manual config" without waiting for automatic config.
Then setup normal IMAP, otherwise it will connect to Exchange server via commercial plugin.

## NTFS and exFat

```
pacman -S ntfs-3g exfat-utils
```


## Fonts

```
# https://aur.archlinux.org/packages/ttf-ms-fonts/
git clone <repo>; cd <repo>; makepkg -si
# https://aur.archlinux.org/packages/ttf-vista-fonts/
git clone <repo>; cd <repo>; makepkg -si

pacman -Su ttf-dejavu
```

## Additional keyboard layout

```
Settings / Input Devices / Keyboard -> Layout
```

## Libreoffice

For me `libreoffice-fresh` experience was unsuccessful, so use `still` branch.

```
pacman -Su libreoffice-still
```

## Texlive

```
pacman -Su texlive-core texlive-latexextra pylatex
```

## Other small utilities

```
pacman -S htop tree mlocate zip unzip wget
sudo /sbin/updatedb  # for locate
```