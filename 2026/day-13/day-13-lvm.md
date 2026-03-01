# Day 13 – Linux Volume Management (LVM)

## Challenge Tasks

### Task 1: Check Current Storage
Run: `lsblk`, `pvs`, `vgs`, `lvs`, `df -h`

 lsblk
NAME     MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
loop0      7:0    0 27.6M  1 loop /snap/amazon-ssm-agent/11797
loop1      7:1    0 27.8M  1 loop /snap/amazon-ssm-agent/12322
loop2      7:2    0 50.9M  1 loop /snap/snapd/25577
loop3      7:3    0   74M  1 loop /snap/core22/2163
loop4      7:4    0 48.1M  1 loop /snap/snapd/25935
loop5      7:5    0   74M  1 loop /snap/core22/2292
xvda     202:0    0   15G  0 disk
├─xvda1  202:1    0   14G  0 part /
├─xvda14 202:14   0    4M  0 part
├─xvda15 202:15   0  106M  0 part /boot/efi
└─xvda16 259:0    0  913M  0 part /boot
xvdf     202:80   0   10G  0 disk /mnt/data
xvdg     202:96   0    7G  0 disk

df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        14G  2.7G   11G  20% /
tmpfs           479M     0  479M   0% /dev/shm
tmpfs           192M  904K  191M   1% /run
tmpfs           5.0M     0  5.0M   0% /run/lock
/dev/xvda16     881M   89M  730M  11% /boot
/dev/xvda15     105M  6.2M   99M   6% /boot/efi
/dev/xvdf       9.8G   24K  9.3G   1% /mnt/data
tmpfs            96M   12K   96M   1% /run/user/1000

lvs : Tells the logical volume => Currently Nothing in output
pvs : tells which partition becomes physical volume => Currently Nothing in output
vgs : Tells about Volume group name => Currently Nothing in output

### Task 2: Create Physical Volume
```bash
pvcreate /dev/sdb   # or your loop device => 
pvs

pvs
  PV         VG Fmt  Attr PSize PFree
  /dev/xvdg     lvm2 ---  7.00g 7.00g

```
----------------------------------------------------------------------------------------------------
### Task 3: Create Volume Group

```bash
vgcreate devops-vg /dev/sdb
vgs

 vgs
  VG        #PV #LV #SN Attr   VSize  VFree
  devops-vg   1   0   0 wz--n- <7.00g <7.00g
```

----------------------------------------------------------------------------------------------------

### Task 4: Create Logical Volume
```bash
lvcreate -L 500M -n app-data devops-vg
lvs

lvs
  LV       VG        Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  app-data devops-vg -wi-a----- 500.00m
```
--------------------------------------------------------------------------------------------------

### Task 5: Format and Mount
```bash
mkfs.ext4 /dev/devops-vg/app-data
mkdir -p /mnt/app-data
mount /dev/devops-vg/app-data /mnt/app-data
df -h /mnt/app-data


 df -h /mnt/app-data
Filesystem                        Size  Used Avail Use% Mounted on
/dev/mapper/devops--vg-app--data  452M   24K  417M   1% /mnt/app-data

```

### Task 6: Extend the Volume
```bash
lvextend -L +200M /dev/devops-vg/app-data
resize2fs /dev/devops-vg/app-data
df -h /mnt/app-data

lvextend -L +200M /dev/devops-vg/app-data
  Size of logical volume devops-vg/app-data changed from 500.00 MiB (125 extents) to 700.00 MiB (175 extents).
  Logical volume devops-vg/app-data successfully resized.

lvm> resize2fs /dev/devops-vg/app-data
  No such command 'resize2fs'.  Try 'help'.
  
lvm>
root@ip-172-31-2-44:/home/ubuntu/90DaysOfDevOps/2026/day-13# resize2fs /dev/devops-vg/app-data
resize2fs 1.47.0 (5-Feb-2023)
Filesystem at /dev/devops-vg/app-data is mounted on /mnt/app-data; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 1
The filesystem on /dev/devops-vg/app-data is now 179200 (4k) blocks long.

root@ip-172-31-2-44:/home/ubuntu/90DaysOfDevOps/2026/day-13# df -h /mnt/app-data
Filesystem                        Size  Used Avail Use% Mounted on
/dev/mapper/devops--vg-app--data  637M   24K  594M   1% /mnt/app-data


```