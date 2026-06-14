
1.Ran **df -h** to confirm no additional volumes were attached yet.

![](1.png)

2.Created a new EBS volume in AWS, ensuring the availability zone matched our EC2 instance (e.g., us-east-1f).
![](2.png)


![](3.png)

![](4.png)
3.Attached the volume to your EC2 instance and verified it with **lsblk**.
![](5.png)

![](6.png)

![](7.png)

![](8.png)

4.Created a mount directory (e.g., /mnt/data).

![](9.png)

5.Formatted the new volume with **mkfs -t ext4**

![](10.png)

6.Mounted the volume and added its UUID to /etc/fstab for permanent mount means after vm reboot this volume will not detach
![](11.png)

![](12.png)

root@ip-172-31-64-170:/# vim /etc/fstab

![](13.png)

root@ip-172-31-64-170:/# mount -a

Above one is traditional way

Now below one is modern way (real env Devops)

Logical Volume Manager(LVM)

7.Initialized the disk as a physical volume with pvcreate, created a volume group with vgcreate, and added logical volumes using lvcreate.

Disk(s) -> Physical volume (pv) -> Volume group (vg) -> Logical volume (lv) -> Filesystem (ext4/xfs) 
-> Mount Point(/data)

![](14.png)

![](15.png)

![](16.png)

![](17.png)

![](18.png)

![](19.png)

![](20.png)

8.Formatted the logical volumes and mounted them to specific directories.

![](21.png)

![](22.png)

![](23.png)

![](24.png)

9.Extended a logical volume with lvextend and resized the filesystem with **resize2fs**

![](25.png)

![](26.png)

10.To safely remove LVM,  unmounted, removed the logical volumes, volume group, and physical volume, and wiped the disk clean.

**LVM Reverse Process (Delete Everything Safely)**

Unmount → Remove LV → Remove VG → Remove PV

![](27.png)

Remove entry from **`/etc/fstab`** (VERY IMPORTANT)

![](28.png)

**mount -a**
(No errors should appear.)

![](29.png)

![](30.png)

![](31.png)

![](32.png)



---


## 💡 Commands Summary

| Command | Purpose |
|---------|---------|
| `lsblk` | List all block devices (disks) |
| `pvcreate /dev/xvdf /dev/xvdg /dev/xvdh` | Create Physical Volumes on all 3 disks |
| `pvs` | View Physical Volumes |
| `vgcreate devops-vg /dev/xvdf /dev/xvdg /dev/xvdh` | Create Volume Group from all 3 PVs |
| `vgs` | View Volume Groups |
| `lvcreate -L 10G -n app-data devops-vg` | Create a 10G Logical Volume |
| `lvs` | View Logical Volumes |
| `mkfs.ext4` | Format LV with ext4 filesystem |
| `mount` | Mount the LV to a directory |
| `lvextend -L +1G` | Extend the LV by 1G |
| `resize2fs` | Resize filesystem after extending |
| `df -h` | Check mounted filesystem usage |

---

## 🧠 What I Learned

1. **LVM lets you combine multiple disks into one pool** - I attached 3 separate EBS volumes (10G + 12G + 14G) and merged them into a single 36G Volume Group. This is something traditional partitioning simply cannot do.

2. **The PV → VG → LV layered architecture gives real flexibility** - Instead of being stuck with fixed partition sizes, LVM lets you allocate storage dynamically from the pool as your application needs grow.

3. **Extending volumes online is safe and easy** - Using `lvextend` + `resize2fs`, I grew a mounted filesystem from 10G to 11G with zero downtime. This is extremely useful in production DevOps environments where you can't afford to stop services.

---
