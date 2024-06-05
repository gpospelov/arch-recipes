# Arch troubleshooting

## Btrfs problem

## BTRFS problems

Unallocated space, and disk full

```
sudo btrfs filesystem usage /
sudo btrfs filesystem show
sudo btrfs device usage /
```

**solution**

```
sudo btrfs filesystem resize 1:max /
sudo btrfs filesystem resize 1:max /home
sudo btrfs scrub start /mnt/btrfs
sudo btrfs scrub status /mnt/btrfs
```

**may be solution**

```
sudo btrfs balance start -dusage=100 / -v
sudo btrfs balance start -musage=100 / -v
sudo btrfs balance start -dusage=100 /home -v
sudo btrfs balance start -musage=100 /home -v

## btrfs-cleaner running all the time with `~10%` of CPU

```bash
btrfs quota disable /
```

seems helping

## Broken files after update

```bash
# `ldconfig` will report all screwed libraries
# install them manually
pacman -S systemd-sysvcompat--overwrite "*"

# check for other packages with missed files
pacman -Qk 2>/dev/null | grep -v ' 0 missing files' 
```

## Openssl warning

After update to openssl-3.0.7 the command `git pull` leads to

```
error:0A000152:SSL routines::unsafe legacy renegotiation disabled
```

Edit ` /etc/ssl/openssl.cnf`

```
At the very beginning of the file, insert the following config:

    reboot

At the end of the file, insert the following config:

[openssl_init]
ssl_conf = ssl_sect

[ssl_sect]
system_default = system_default_sect

[system_default_sect]
MinProtocol = TLSv1.2
CipherString = DEFAULT@SECLEVEL=1
Options = UnsafeLegacyRenegotiation


``

https://pipeawk.com/index.php/2022/05/19/openssl-enable-legacy-renegotiation/


## Perl locale warning (LC_ALL="")

Simple regeneration of the locale doesn't work, what is worked was

```
add line
LC_ALL=en_US.UTF-8

/etc/environment

## Screwed up file system

```
mount /dev/nvmep0n1p6 /mnt
# fsck -r /dev/nvme0n1p6
#arch-chroot /mnt

pacman --root /mnt --cache /mnt/var/cache/pacman/pkg -Syu
# rm /mnt/var/lib/pacman/db.lock

```
