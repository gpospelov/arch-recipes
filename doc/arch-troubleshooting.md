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
