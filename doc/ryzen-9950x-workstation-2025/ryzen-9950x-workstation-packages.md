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

rslsync --dump-sample-config > ~/ 
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

https://www.reddit.com/r/archlinux/comments/pyrcvk/cc_extension_doesnt_show_up_on_vs_code/
The code package in the repos uses the open-vsix.org repositories, as Microsoft's EULA prevents unofficial builds from using the official ones. Use visual-studio-code-bin. 
Alternatively you can install code-marketplace from AUR which adds a Pacman hook to edit a config file of the code package to enable the MS repositories. 


```
#yay -S vscode
yay -S visual-studio-code-bin

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
yay -S skypeforlinux-bin
yay -S teams
```

## Music player

pacman -S elisa

## Development

cmake benchmark gtest libxml2 gdb valgrind gperf gcc clang llvm
qt5 qtcreator breeze5

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

# Settings Testing /General/  Automatically Run All

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
SwitchHeaderSource       F4 (i.e. as default)
ClangFormat FormatFile   Ctrl+Shift+I (it was no, remember CppEditor.OpenIncludeHierarchy and Help.index conflicts with Ctrl-Shift-o)
GoTo                     Ctrl+G
FollowSymbolUnderCursor  F2 (i.e. default)

# # Analyzer
# Edit / Preferences / Analyzer
# - make new custom diagnostic configuration
#   - Clang-Tidy Checks - Use .clang-tidy config file
#   - Clazy-Checks - disable non-pod-global-static
# 
# cppcoreguidelines-pro-type-reinterpret-cast

> Settings -> Environment
PROJECTS=/home/pospelov/development/iter/projects
MYBUILD=build/Desktop-Debug
EPICS_BASE=/home/pospelov/development/iter/extern/epics/epics-base/
EPICS_HOST_ARCH=linux-x86_64
PVXS_DIR=/home/pospelov/development/iter/extern/epics/pvxs/
MYPLUGIN=build/Desktop-Debug/lib/oac-tree/plugins

Sequencer-gui project settings / Environment
LD_LIBRARY_PATH=${PROJECTS}/sequencer-plugin-epics/${MYPLUGIN}:${PROJECTS}/sequencer-plugin-sup/${MYPLUGIN}:${PROJECTS}/sequencer-plugin-control/${MYPLUGIN}:${PROJECTS}/sequencer-plugin-mathexpr/${MYPLUGIN}:${PROJECTS}/sequencer-plugin-system/${MYPLUGIN}:${PROJECTS}/sequencer-plugin-strings/${MYPLUGIN}:${EPICS_BASE}/lib/${EPICS_HOST_ARCH}:${PVXS_DIR}/lib/${EPICS_HOST_ARCH}

```

## Avahi network zero configuration

```

pacman -S nss-mdns avahi

# change line /etc/nsswitch.conf 
#hosts: mymachines resolve [!UNAVAIL=return] files myhostname dns
mymachines mdns_minimal [NOTFOUND=return] resolve [!UNAVAIL=return] files myhostname dns

sudo systemctl enable avahi-daemon.service

avahi-resolve-host-name pi5.local
# интересно, сначало показал mac-address, а после успешного ssh стал показывать IP
avahi-browse --all --ignore-local --resolve --terminate

```

## Printing 

```
pacman -S  cups

systemctl enable cups
systemctl start cups

see cups at http://localhost:631
```

## Installing printer Canon G500

- must be a better way but...

```
https://askubuntu.com/questions/1164660/driver-for-new-canon-ink-tank-printers

#pacman -S system-config-printer foomatic-db-gutenprint-ppds gutenprint
pacman -S system-config-printer cnijfilter2

- downloaded driver from
https://www.canon-europe.com/support/consumer/products/printers/pixma/g-series/pixma-g5050.html?type=drivers&language=en&os=linux%20(64-bit)

- Printer is in LAN mode, read IP from printer panel
From KDE Plasma in printer settings added printer
ipp://192.168.1.209/ipp/print
and add manually pdd file as a driver from just installed driver source
<driver-source>/cnijfilter2-source-5.90-1/ppd/canong5000.ppd
```

## Monitoring frequences

turbostat - to see frequences
sensors to see temperature
k10temp-pci-00c3 value


## More installations

kcompare kcachegrind adwaita-qt5 adwaita-qt6

## Webcam

cameractrls (cameractrlsgtk4)

## Virtualbox

```

pacman -S virtualbox
# and select mirror "virtualbox-host-modules-arch"
usermod -aG vboxusers jamesbond

```

