# Linux Disk Configuration using LVM

##  Project Overview


This repository demonstrates how to configure an additional disk in a Linux server using **Logical Volume Manager (LVM)**.
LVM provides flexible disk management, allowing administrators to easily extend, reduce, and manage storage volumes.

This guide covers the complete workflow:

* Creating Physical Volumes (PV)
* Creating Volume Groups (VG)
* Creating Logical Volumes (LV)
* Formatting and mounting the filesystem
* Making mounts persistent

---

##  Lab Environment

| Component        | Details                        |
| ---------------- | ------------------------------ |
| Operating System | Linux (RHEL / CentOS / Ubuntu) |
| Server Type      | Virtual Machine                |
| Disk Added       | /dev/sdb                       |
| Access           | Root / Sudo                    |

---

#  LVM Configuration Steps

## Step 1: Verify Available Disks

```bash
lsblk
fdisk -l
```

---

## Step 2: Create Physical Volume (PV)

```bash
pvcreate /dev/sdb
```

Verify

```bash
pvs
```

---

## Step 3: Create Volume Group (VG)

```bash
vgcreate data_vg /dev/sdb
```

Verify

```bash
vgs
```

---

## Step 4: Create Logical Volume (LV)

```bash
lvcreate -L 10G -n data_lv data_vg
```

Verify

```bash
lvs
```

---

## Step 5: Create Filesystem

```bash
mkfs.ext4 /dev/data_vg/data_lv
```

---

## Step 6: Create Mount Point

```bash
mkdir /data
```

---

## Step 7: Mount Logical Volume

```bash
mount /dev/data_vg/data_lv /data
```

Verify

```bash
df -h
```

---

## Step 8: Persistent Mount (Important)

Edit `/etc/fstab`

```bash
vi /etc/fstab
```

Add the following entry

```
/dev/data_vg/data_lv   /data   ext4   defaults   0 0
```

Apply changes

```bash
mount -a
```

---

##  Verification

Check disk usage

```bash
df -h
```

Check block devices

```bash
lsblk
```

Check LVM configuration

```bash
pvs
vgs
lvs
```

---

##  LVM Architecture

```
Disk (/dev/sdb)
      ↓
Physical Volume (PV)
      ↓
Volume Group (VG)
      ↓
Logical Volume (LV)
      ↓
Filesystem (ext4)
      ↓
Mount Point (/data)
```

---

##  Main Benefits of LVM

* Dynamic disk resizing
* Easy storage expansion
* Snapshot support
* Flexible storage management

---

##  References

Linux Manual Pages

```bash
man lvm
man pvcreate
man vgcreate
man lvcreate
```

---

##  Author

Shaik Navab Jhon

---

⭐ If you found this useful, consider giving the repository a star.
