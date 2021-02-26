## RAID CONCEPT

## Raid 0 
```sh
data saving is fast 
two hard disks
A,B,C,D--> data 
A,B goes in one hdd C,D goes in 2nd hard disk
If One hdd is gone whole data is gone
```
## Setup Raid 0
```sh
lsblk #can see 2 hdd-->sdb ,sdc 
fdisk /dev/sdb #partioning sdb
  n            #new_partion
  p            #primary
               #entre enter to create partion

command (m for help):t    #toggle ID
  hex code: fd            #changed type of partion 'LINUX' to LINUX raid autodetect

command (m for help):w
```
## Similarly do this with the 2nd harddisk
```sh
fdisk  /dev/sdc
    n
    p
    t
   :fd
    w
```
## to update information to kernal
```sh
partprobe
```
## Creating raid 0 now
```sh
mdadm -C /dev/md0 -l 0 -n 2 /dev/sdb1 /dev/sdc1  #-l=level 0 #n=number_of_hdd 
mkfs.ext4 /dev/md0      #formatting
mdadm --details /dev/md0     #to check and verify the raid0
```
## mounting and permanent mounting
```sh
mkdir /xyz
vim /etc/fstab
 /dev/md0  /xyz ext4 defaults 0 0
:wq!
mount -a
df -h
```
## failing and checking
```sh 
unmount and remove entry from fstab
mdadm /dev/md0 -f /dev/sdb1
mdm --detail /dev/md0 #it will only show sdc
mount /dev/md0 /xyz #it will not work and data will not show
```
# RAID LEVEL 1
mirroring--> 2same size hdd(same data copy in both hdd, data secure)

```sh
lsblk
fdisk /dev/sdb
 n
 t
:fd
w
fdisk /dev/sdc
n
t
:fd
w
partprobe

mdadm -C /dev/md0 -l 1 -n 2 /dev/sdb1 /dev/sdc1
mkfs.ext3 /dev/md0 #format
mdadm --detail /dev/md0  #verify
```
## now mounting
```sh
mkdir /data
mount /dev/md0 /data/
cd /data
touch aa bb cc
```
## now fail one hdd and verify
```sh
mdadm /dev/md0 -f /dev/sdc1
mdadm --detail /dev/md0 #u can see its faulty
#but if u cd /data data persists
```

## now removing hdd and adding one new to raid 1
```sh
mdadm /dev/md0 -r /dev/sdc1 #detached
fdisk /dev/sdd
n
t
:fd
w
partprobe
mkfs.ext3 /dev/sdd1
mdadm /dev/md0 --add /dev/sdd1
mdadm --detail /dev/md0 #verify
```
-------------------------------
# RAID LEVEL 5 (3 same size hdd, data saves in form of parity)
## if one hdd fails,2 working --> data will be secured
```sh
fdisk /dev/sdb
n t :fd :w
#same with /dev/sdc and /dev/sdd
partprobe 
mdadm -C /dev/md0 -l 5 -n 3 /dev/sdb1 /dev/sdc1 /dev/sdd1
mkfs.ext4 /dev/md0
mkdir /data1
mount /dev/md0 /data1
cd /data1 #put sm data
mdadm --detail /dev/md0 #verify
mdadm /dev/md0 -f /dev/sdc1 #in detail it willl show faulty
cd /data1 #data pesists
``` 
