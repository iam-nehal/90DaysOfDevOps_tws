
1.Ran **df -h** to confirm no additional volumes were attached yet.

![[Pasted image 20260212233011.png]]

2.Created a new EBS volume in AWS, ensuring the availability zone matched our EC2 instance (e.g., us-east-1f).
![[Pasted image 20260210222636.png]]


![[Pasted image 20260210223152.png]]

![[Pasted image 20260210223442.png]]
3.Attached the volume to your EC2 instance and verified it with **lsblk**.
![[Pasted image 20260210223709.png]]

![[Pasted image 20260210224039.png]]

![[Pasted image 20260211202418.png]]

![[Pasted image 20260211202544.png]]

4.Created a mount directory (e.g., /mnt/data).

![[Pasted image 20260211203341.png]]

5.Formatted the new volume with **mkfs -t ext4**

![[Pasted image 20260211204439.png]]

6.Mounted the volume and added its UUID to /etc/fstab for permanent mount means after vm reboot this volume will not detach
![[Pasted image 20260211204904.png]]

![[Pasted image 20260211212903.png]]

root@ip-172-31-64-170:/# vim /etc/fstab

![[Pasted image 20260211213455.png]]

root@ip-172-31-64-170:/# mount -a

Above one is traditional way

Now below one is modern way (real env Devops)

Logical Volume Manager(LVM)

7.Initialized the disk as a physical volume with pvcreate, created a volume group with vgcreate, and added logical volumes using lvcreate.

Disk(s) -> Physical volume (pv) -> Volume group (vg) -> Logical volume (lv) -> Filesystem (ext4/xfs) 
-> Mount Point(/data)

![[Pasted image 20260211214904.png]]

![[Pasted image 20260211215110.png]]

![[Pasted image 20260211220743.png]]

![[Pasted image 20260211220829.png]]


![[Pasted image 20260211221740.png]]

![[Pasted image 20260211222129.png]]

![[Pasted image 20260211222411.png]]

8.Formatted the logical volumes and mounted them to specific directories.

![[Pasted image 20260211222712.png]]

![[Pasted image 20260211223030.png]]

![[Pasted image 20260211223300.png]]

![[Pasted image 20260211224225.png]]

9.Extended a logical volume with lvextend and resized the filesystem with **resize2fs**

![[Pasted image 20260211225140.png]]

![[Pasted image 20260211225402.png]]

10.To safely remove LVM,  unmounted, removed the logical volumes, volume group, and physical volume, and wiped the disk clean.

**LVM Reverse Process (Delete Everything Safely)**

Unmount → Remove LV → Remove VG → Remove PV

![[Pasted image 20260211230042.png]]

Remove entry from **`/etc/fstab`** (VERY IMPORTANT)

![[Pasted image 20260211230445.png]]

**mount -a**
(No errors should appear.)

![[Pasted image 20260211230856.png]]

![[Pasted image 20260211231108.png]]

![[Pasted image 20260211231525.png]]


![[Pasted image 20260211231925.png]]
