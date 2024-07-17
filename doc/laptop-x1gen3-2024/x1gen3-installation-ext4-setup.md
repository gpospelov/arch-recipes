## Add user rights
```
pacman -S sudo
nano /etc/sudoers
>> jamesbond ALL=(ALL) ALL
```

## 10 min limits on 3 unsuccessfull login attempts

```
# change values in
/etc/security/faillock.conf
```

## Power consumption

[TLP - ArchWiki](https://wiki.archlinux.org/index.php/TLP)

```
pacman -S tlp
systemctl enable tlp.service
```

## Desktop

```
pacman -S xorg-server xorg-apps 
pacman -S nvidia nvidia-utils nvidia-settings
pacman -S plasma 
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

## First apps

```
pacman -S konsole gwenview kcolorchooser dolphin okular
```

## Other small utilities

```
pacman -S htop tree mlocate zip unzip wget tokei pacman-contrib screen ncdu
sudo /sbin/updatedb  # for locate
```

## Additional keyboard layout

```
Settings / Keyboard -> Layout
Main shortcut Win+Space
```

## Firefox

```
packman -S firefox

# install plugins privacy-badget, uBlock origin
```

### Minor KDE fixes

```
# Hidden pager
Settings > Window management/Virtual desktop -> add 6 desktops

# Shorcuts for desktop switching
Settings > keyboard> Shortcuts >  KWin 

# SDDM theme
Settings > Colors&Themes > Global Theme > Login SDDM

# Disable instant messaging
> Settings / Session / Background service / (disable) Accounts

# Do not let windows appear at startup
> Settings /Session / Desktop Session / (disable) Start with an empty session

# Remove animation
Settings > WIndows Management / Virtual Desktop Switching Animation

# konsole
Right-mouse-bottom on a toolbar / Toolbar shown / Main toolbar -> hide
Right mouse button/ Menu/Settings/Configure Keyboard Shortcuts -> Remove F10

```

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

## NTFS and exFat

```
pacman -S ntfs-3g exfat-utils
```

## Basic development

```
pacman -S git base-devel

.gitignore
/.gitignore
target
/CMakeLists.txt.user
build


git config --global core.excludesfile ~/.gitignore

```

## Install yay

```
cd software
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

## Rslsync

```
yay -S rslsync

rslsync --dump-sample-config > ~/.config/rslsync/rslsync.conf
> edit device_name and storage_path  (/home/jamesbond/.sync)

systemctl --user start rslsync
systemctl --user enable rslsync 

```

## Tuning Midnight Commander

```
# Press F9 to activate top menu, in "Options/Configuration" remove crosses from "Use internal editor/viewer"
# Don't forget to save configuration

nano .bashrc
export EDITOR=nano
export PAGER=less
```

## VisualStudio

```
yay -S vscode
```

### Extensions

- Markdown All In One, Yu Zhang
- Markdown PDF, yzane
- XML tools (Josh Johnson)

## Gitkraken

yay -S gitkraken

## Sound (pipewire based, not pulseaudio)

sof-firmware alsa-card-profiles alsa-lib alsa-plugins
pipewire-alsa

systemctl --user start pipewire

## skype, teams

```
yay -s skype
yay -s teams
```

## Music player

pacman -S elisa

## Development

cmake benchmark gtest libxml2 gdb valgrind gperf gcc clang
qt5 qtcreator

## Qt creator config

```
# Enable plugins
ToDo
Beautifier

# Editor
  - indentation

Settings > TextEditor > Display > Display right margin at column

Settings > Built And Run > General > [x] Save All files before the build

Settings > Kits > Manual(Desktop) > Compiler C/C++ -> gcc

# Settings Testing / Automatically Run All

# Go to C++ General and change indentation/tabs to 2 spaces
  Use Custom Settings, new profile COA
  AccessModifierOffset: -2
  IndentWidth:     2

# Beautifier
Edit / Preferences / Beautifier / CLang format
Use customized style "iter"

# Git
Setting / Version Control / Git / Instant Blame

# shortcuts
Edit / Preferences / Environment /  Keyboard
SwitchHeaderSource       alt-o (it was F4, remember SideBarOpenDocument conflicts with alt-o)
ClangFormat FormatFile   Ctrl+Shift+I (it was no, remember CppEditor.OpenIncludeHierarchy and Help.index conflicts with Ctrl-Shift-o)
GoTo                     Ctrl+G
FollowSymbolUnderCursor  F12 (was F2)

# Analyzer
Edit / Preferences / Analyzer
- make new custom diagnostic configuration
  - Clang-Tidy Checks - Use .clang-tidy config file
  - Clazy-Checks - disable non-pod-global-static

cppcoreguidelines-pro-type-reinterpret-cast

> Settings
PROJECTS=/home/pospelov/development/iter/projects
MYBUILD=build/Desktop-Debug
EPICS_BASE=/home/pospelov/development/iter/extern/epics/epics-base/
EPICS_HOST_ARCH=linux-x86_64
PVXS_DIR=/home/pospelov/development/iter/extern/epics/pvxs/

EPICS_BASE=/home/pospelov/development/iter/extern/epics/epics-base/
PROJECTS=/home/pospelov/development/iter/projects
EPICS_HOST_ARCH=linux-x86_64
MYBUILD=build/Desktop-Debug
LD_LIBRARY_PATH=${PROJECTS}/sequencer-plugin-epics/build-debug/lib/sequencer/plugins:${PROJECTS}/sequencer-plugin-control/build-debug/lib/sequencer/plugins:${PROJECTS}/sequencer-plugin-mathexpr/build-debug/lib/sequencer/plugins:${EPICS_BASE}/lib/${EPICS_HOST_ARCH}:${PVXS_DIR}/lib/${EPICS_HOST_ARCH}

```

