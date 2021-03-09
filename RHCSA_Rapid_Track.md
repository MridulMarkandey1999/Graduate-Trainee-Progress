# Chapter 8

## Mounting File Systems Manually
- The mount command allows the root user to manually mount a file system. The first argument
of the mount command specifies the file system to mount. The second argument specifies the
directory to use as the mount point in the file-system hierarchy.

- There are two common ways to specify the file system on a disk partition to the mount command:

  • With the name of the device file in /dev containing the file system

  • With the UUID written to the file system, a universally-unique identifier.

## Identifying the Block Device

- Use the lsblk command to list the details of a specified block device or all the available devices.
```
[root@host ~]# lsblk
NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
vda 253:0 0 12G 0 disk
├─vda1 253:1 0 1G 0 part /boot
├─vda2 253:2 0 1G 0 part [SWAP]
└─vda3 253:3 0 11G 0 part /
vdb 253:16 0 64G 0 disk
└─vdb1 253:17 0 64G 0 part
```
- The /mnt directory exists by
default and is intended for use as a temporary mount point.
You can use /mnt directory, or better yet create a subdirectory of /mnt to use as a temporary
mount point, unless you have a good reason to mount it in a specific location in the file-system
hierarchy.

## Mounting by Block Device Name

```
[root@host ~]# mount /dev/vdb1 /mnt/data
```

## Mounting by File-system UUID

- The lsblk -fp command lists the full path of the device, along with the UUIDs and mount
points, as well as the type of file system in the partition. If the file system is not mounted, the
mount point will be blank.

```
[root@host ~]# lsblk -fp
NAME FSTYPE LABEL UUID MOUNTPOINT
/dev/vda
├─/dev/vda1 xfs 23ea8803-a396-494a-8e95-1538a53b821c /boot
├─/dev/vda2 swap cdf61ded-534c-4bd6-b458-cab18b1a72ea [SWAP]
└─/dev/vda3 xfs 44330f15-2f9d-4745-ae2e-20844f22762d /
/dev/vdb
└─/dev/vdb1 xfs 46f543fd-78c9-4526-a857-244811be2d88

```
- Mount the file system by the UUID of the file system.

```
[root@host ~]# mount UUID="46f543fd-78c9-4526-a857-244811be2d88" /mnt/data
```
## Automatic Mounting of Removable Storage Devices

- If you are logged in and using the graphical desktop environment, it will automatically mount any
removable storage media when it is inserted.
The removable storage device is mounted at /run/media/USERNAME/LABEL where USERNAME
is the name of the user logged into the graphical environment and LABEL is an identifier, often the
name given to the file system when it was created if one is available.
Before removing the device, you should unmount it manually.
