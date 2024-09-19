# LVM Setup Guide

Welcome to the LVM Setup Guide! This repository provides step-by-step instructions to create and manage Logical Volume Management (LVM) on your Linux system. Whether you're a beginner or just need a quick reference, this guide is designed to be simple and easy to follow.

## Table of Contents
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Steps to Create LVM](#steps-to-create-lvm)
4. [Conclusion](#conclusion)

## Introduction
Logical Volume Management (LVM) allows you to manage disk space more flexibly than traditional partitioning. With LVM, you can easily resize, create, and manage disk volumes without needing to worry about the underlying physical storage.

## Prerequisites
Before you begin, ensure you have:
- A Linux environment with root access.
- A new hard disk drive added to your virtual machine.

## Steps to Create LVM

### 1. Install a New Hard Disk Drive
To add a new hard disk:
- Open **VirtualBox Manager**.
- Navigate to **Settings** > **Storage** > **Controller: SATA**.
- Click **Add Attachment** > **Hard Disk** > **Create** > **Next** > **Next**.
- Set the size to **250 MB** and choose a folder name.
- Click **Finish**, then **Choose**, and **OK**.

### 2. Make a Partition to Use It
- Open the terminal and run: `fdisk -l` to list your disks.
- Check the partitions with: `lsblk`.
- Type `fdisk /dev/sdb` to start partitioning.
  - Type `m` for help.
  - Type `n` to create a new partition.
  - Choose **Primary** (P).
  - Accept the default for the partition number.
  - Accept the default for the first and last sectors (this creates a partition for Linux).
  - Change the type to LVM by typing `t` and then the hex code `8e`.
  - Type `w` to write changes.
- Run `fdisk -l` again to confirm your changes.

### 3. Designate Physical Volume (PV)
- Create the physical volume with: `pvcreate /dev/sdb1`.
- Display the physical volume details using: `pvdisplay`.

### 4. Manage Volume Group (VG)
- Create a volume group with: `vgcreate vg1 /dev/sdb1`.
- Check the volume group information with: `vgdisplay`.

### 5. Manage Logical Volume (LV)
- Create a logical volume using: `lvcreate -L 200M -n datalv vg1`.
- View logical volume details with: `lvdisplay`.

### 6. Apply a Filesystem
- Format the logical volume with: `mkfs.ext4 /dev/vg1/datalv`.
- Create a mount point using: `mkdir /linux`.
- Mount the logical volume: `mount /dev/vg1/datalv /linux/`.
- Check your filesystem usage with: `df -Th`.

### 7. Set Up Permanent Mounting
- Check current mounts with: `cat /etc/mtab`.
- Edit the fstab file: `nano /etc/fstab`.
- Add this line at the end:
  ```
  /dev/mapper/vg1-datalv /linux ext4 defaults 0 0
  ```
- Save and exit.

### 8. Set a Mount Point
- Run: `mount -av` to mount all filesystems specified in fstab.

## Conclusion
Congratulations! You’ve successfully set up LVM on your Linux system. This guide should help you manage your disk space more efficiently. If you have any questions or feedback, feel free to reach out.
