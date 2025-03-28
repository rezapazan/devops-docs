# Disk Partitioning

## Partition Table

There are two types:
1- MBR (Master Boot Record)
2- GPT (GUID Partition Table)

### MBR

- 512MB from beginning of the disk is used for boot loader: 466 for boot loader, 4\*16 for metadata.
- Only 4 primary partitions.

### GPT

- No limitation on primary partitions (approx 128).
- +2TB Disks.

## File System

Used by OS & kernel to store, recall and use the data on disk.

- ext4 (Ubuntu) (better with small data)
- ext3
- ext2
- xfs (CentOS) (better with large data)

When linux boots, it understands partitions from `/etc/fstab`. It works with UUID.

### INode (Index Note)

- Metadata for disk, keeps where is what
- Limitation on count. Some systems have dynamic inode.
- Separate `/boot`(2~3GB), `/`(30~50GB), `/var`(full of logs & data, LVM). Partitions that may have side effects on each other.

## General Disk Commands

```bash
$ df -ih
# for inodes
$ lsblk
$ du -sh
$ ncdu
# graphical output of du
$ fdisk
$ gparted
```

## LVM (Logical Volume Management)

- PV: Physical Volume
- VG: Volume Group
- LV: Logical Volume

**Note:** Resize is supported well with ext4 & ext3.

### LVM Commands

```bash
$ vgs
# shows volume groups
$ lvs
$ vgextend
$ lvextend /dev/sda3 /dev/mapper/ubuntu--vg-ubuntu--lv
$ resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
$ mkfs.ext4 /dev/sda
# formats the disk
$ mount -a
# looks on fstab & mounts everything again
```
