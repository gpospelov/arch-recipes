# Arch (post-install, packages)

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
```

## Basic development

```
packman -S git base-devel
```

## C++/Python development

```
pacman -S python python-matplotlib python-pip python-numpy
pacman -S cmake boost gsl fftw libtiff valgrind
pacman -S qt5 qtcreator
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

## Dolphin file manager

```
Configure Dolphin / General / Miscellaneous - Switch between split views panes with tab key
System Settings / Workspace Behaviour / General Behaviour - Double click to open files and folders
```

## Qt creator and HighDPI

```
# Advice from here https://wiki.archlinux.org/index.php/User:Ctag/HiDPI#Qt_Creator
cp /usr/share/applications/org.qt-project.qtcreator.desktop ~/.local/share/applications
Exec=env QT_SCALE_FACTOR=1 env QT_AUTO_SCREEN_SCALE_FACTOR=0 env QT_SCREEN_SCALE_FACTORS=1.75 qtcreator %F

# For me the recipe above didn't work. Something worked, but what it was is unclear.
# Now qtcreator always looks nice. It must be remembering it's settings somewhere.

# either this
Exec=QT_SCALE_FACTOR=1 QT_AUTO_SCREEN_SCALE_FACTOR=0 QT_SCREEN_SCALE_FACTORS=1.75 qtcreator %F

# or in .bashrc
export QT_SCALE_FACTOR=1
export QT_AUTO_SCREEN_SCALE_FACTOR=0
export QT_SCREEN_SCALE_FACTORS=1.75

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

## Code

```
pacman -S code
```

### Extensions

- C/C++ IntelliSence Microsoft
- Clang-Format, Xavier Hellauer
- Clang-Tidy, notskm
- CMake, twxs
- CMake Tools, Microsoft
- Markdown All In One, Yu Zhang
- Markdown PDF, yzane
- Pylance Microsoft
- Python, Microsoft
- Remote-SSH, Microsoft
- Remote Explorer Microsoft
- XML tools (Josh Johnson)

Dokcer (Microsoft), GitGraph (mhutchie), GitKraken Authentification (GitKraken), IntelliCode (Microsoft), LatexWorkshop (James Yu), Markdown Lint (David Anson)
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

## Music player

#pacman -S elsa audacious audacity ardour pulseaudio-jack
strawbery, elise

## Steam

```
uncomment the [multilib] section in /etc/pacman.conf
pacman -S steam
pacman -S lib32-nvidia-utils
```

## Wine

```
sudo pacman -S wine winetricks
# run wincfg from terminal, adjust dpi to 240 for 4K
winetricks corefonts
```

## Better fonts

```
mkdir /usr/share/fonts/WindowsFonts
cp /windows/Windows/Fonts/* /usr/share/fonts/WindowsFonts/
chmod 644 /usr/share/fonts/WindowsFonts/*
fc-cache --force
```

## AceMoney

LC_ALL="ru_RU.CP1251" wine "/home/pospelov/.wine/drive_c/Program Files (x86)/AceMoney/AceMoney.exe"

## Find package installed by makepkg

pacman -Qm teams-for-linux

## Network

pacman -S bind net-tools

# System load viewer

- Widget from KDE store
- ksysguard


## aur yay

```
https://aur.archlinux.org/yay.git
yay -Y --gendb # to generate a development package database 
```

## Other

gwenview
spectacle

## Qt Creator config

```
# Beautifier
Edit / Preferences / Beautifier / CLang format
Use customized style "iter"

# shortcuts
Edit / Preferences / Environment /  Keyboard
SwitchHeaderSource       alt-o
ClangFormat FormatFile   Ctrl+Shift+I
GoTo                     Ctrl+G
FollowSymbolUnderCursor  F12

# Analyzer
Edit / Preferences / Analyzer
- make new custom diagnostic configuration
  - Clang-Tidy Checks - Use .clang-tidy config file
  - Clazy-Checks - disable non-pod-global-static

# Editor
  - indentation

```

## Cpupower

https://github.com/google/benchmark/blob/main/docs/reducing_variance.md

sudo cpupower frequency-set --governor performance
cpupower frequency-info -o proc