> 1. Изучил

> 2. Жёсткая ссылка назначается физически на файл, соответственно использует права самого файла. 

> 3. Поднята VM с новой конфигурацией

> 4. 
`root@vagrant:~# fdisk -l`<br>
<details>   
<summary>Disk /dev/sda: 64 GiB, 68719476736 bytes, 134217728 sectors</summary>
<p><i>
Disk model: VBOX HARDDISK <br>
Units: sectors of 1 * 512 = 512 bytes <br>
Sector size (logical/physical): 512 bytes / 512 bytes <br>
I/O size (minimum/optimal): 512 bytes / 512 bytes <br>
Disklabel type: dos <br>
Disk identifier: 0x3f94c461 <br>
 <br>
Device     Boot   Start       End   Sectors  Size Id Type <br>
/dev/sda1  *       2048   1050623   1048576  512M  b W95 FAT32 <br>
/dev/sda2       1052670 134215679 133163010 63.5G  5 Extended <br>
/dev/sda5       1052672 134215679 133163008 63.5G 8e Linux LVM <br>
 <br>
 <br>
Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors <br>
Disk model: VBOX HARDDISK <br>
Units: sectors of 1 * 512 = 512 bytes <br>
Sector size (logical/physical): 512 bytes / 512 bytes <br>
I/O size (minimum/optimal): 512 bytes / 512 bytes <br>
 <br>

Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors <br>
Disk model: VBOX HARDDISK <br>
Units: sectors of 1 * 512 = 512 bytes <br>
Sector size (logical/physical): 512 bytes / 512 bytes <br>
I/O size (minimum/optimal): 512 bytes / 512 bytes</i></details><br>
`root@vagrant:~# fdisk /dev/sdb` <br>
>>Command (m for help): n <br>
>>Partition type <br>
>>   p   primary (0 primary, 0 extended, 4 free) <br>
>>   e   extended (container for logical partitions) <br>
>>Select (default p): `p` <br>
>>Partition number (1-4, default 1): `1` <br>
>>First sector (2048-5242879, default 2048): `2048` <br>
>>Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-5242879, default 5242879): `+2048M` <br>
>>Created a new partition 1 of type 'Linux' and of size 2 GiB. <br>
>>Command (m for help): `n` <br>
>>Select (default p): `p` <br>
>>Partition number (2-4, default 2): `2` <br>
>>First sector (4196352-5242879, default 4196352): `4196352` <br>
>>Last sector, +/-sectors or +/-size{K,M,G,T,P} (4196352-5242879, default 5242879): ` ` <br>
>>Created a new partition 2 of type 'Linux' and of size 511 MiB. <br>

`root@vagrant:~# /sbin/fdisk -l`
<details>   
    <summary>
    Device     Boot   Start     End Sectors  Size Id Type <br>
    /dev/sdb1          2048 4196351 4194304    2G 83 Linux <br>
    /dev/sdb2       4196352 5242879 1046528  511M 83 Linux <br>
    </summary>
<p>
Disk /dev/sda: 64 GiB, 68719476736 bytes, 134217728 sectors <br>
Disk model: VBOX HARDDISK <br>
Units: sectors of 1 * 512 = 512 bytes <br>
Sector size (logical/physical): 512 bytes / 512 bytes <br>
I/O size (minimum/optimal): 512 bytes / 512 bytes <br>
Disklabel type: dos <br>
Disk identifier: 0x3f94c461 <br>
 <br>
Device     Boot   Start       End   Sectors  Size Id Type <br>
/dev/sda1  *       2048   1050623   1048576  512M  b W95 FAT32 <br>
/dev/sda2       1052670 134215679 133163010 63.5G  5 Extended <br>
/dev/sda5       1052672 134215679 133163008 63.5G 8e Linux LVM <br>
 <br>
 <br>
Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors <br>
Disk model: VBOX HARDDISK <br>
Units: sectors of 1 * 512 = 512 bytes <br>
Sector size (logical/physical): 512 bytes / 512 bytes <br>
I/O size (minimum/optimal): 512 bytes / 512 bytes <br>
Disklabel type: dos <br>
Disk identifier: 0x8fe54f9a <br>
 <br>
Device     Boot   Start     End Sectors  Size Id Type <br>
/dev/sdb1          2048 4196351 4194304    2G 83 Linux <br>
/dev/sdb2       4196352 5242879 1046528  511M 83 Linux <br>
</p></details>

> 5. `sfdisk -d /dev/sdb|sfdisk --force /dev/sdc`
<details> <summary>Checking that no-one is using this disk right now ... OK</summary><br>
<br>
Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors<br>
Disk model: VBOX HARDDISK<br>
Units: sectors of 1 * 512 = 512 bytes<br>
Sector size (logical/physical): 512 bytes / 512 bytes<br>
I/O size (minimum/optimal): 512 bytes / 512 bytes<br>
<br>
>>> Script header accepted.<br>
>>> Script header accepted.<br>
>>> Script header accepted.<br>
>>> Script header accepted.<br>
>>> Created a new DOS disklabel with disk identifier 0x8fe54f9a.<br>
/dev/sdc1: Created a new partition 1 of type 'Linux' and of size 2 GiB.<br>
/dev/sdc2: Created a new partition 2 of type 'Linux' and of size 511 MiB.<br>
/dev/sdc3: Done.<br>
<br>
New situation:<br>
Disklabel type: dos<br>
Disk identifier: 0x8fe54f9a<br>
Device     Boot   Start     End Sectors  Size Id Type<br>
/dev/sdc1          2048 4196351 4194304    2G 83 Linux<br>
/dev/sdc2       4196352 5242879 1046528  511M 83 Linux<br>
<br>
The partition table has been altered.<br>
Calling ioctl() to re-read partition table.<br>
Syncing disks.<br>
</details> 

> 6. `sudo mdadm -C /dev/md0 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1`
> 7. `sudo mdadm -C /dev/md1 --level=0 --raid-devices=2 /dev/sdb2 /dev/sdc2`
> 8. `sudo pvcreate /dev/md0 /dev/md1`
> 9. `sudo vgcreate vol_group1 /dev/md0 /dev/md1`
> 10. `sudo lvcreate -L 100M -n logical_vol1 vol_group1 /dev/md1`
> 11. `sudo mkfs.ext4 /dev/vol_group1/logical_vol1`
> 12. `mkdir /tmp/new`<br>
`sudo mount /dev/vol_group1/logical_vol1 /tmp/new`
> 13. `wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz`

>14. `lsblk`
```NAME                          MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                             8:0    0   64G  0 disk
├─sda1                          8:1    0  512M  0 part  /boot/efi
├─sda2                          8:2    0    1K  0 part
└─sda5                          8:5    0 63.5G  0 part
  ├─vgvagrant-root            253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1          253:1    0  980M  0 lvm   [SWAP]
sdb                             8:16   0  2.5G  0 disk
├─sdb1                          8:17   0    2G  0 part
│ └─md0                         9:0    0    2G  0 raid1
└─sdb2                          8:18   0  511M  0 part
  └─md1                         9:1    0 1018M  0 raid0
    └─vol_group1-logical_vol1 253:2    0  100M  0 lvm   /tmp/new
sdc                             8:32   0  2.5G  0 disk
├─sdc1                          8:33   0    2G  0 part
│ └─md0                         9:0    0    2G  0 raid1
└─sdc2                          8:34   0  511M  0 part
  └─md1                         9:1    0 1018M  0 raid0
    └─vol_group1-logical_vol1 253:2    0  100M  0 lvm   /tmp/new
``` 
> 15.   `root@vagrant:~# gzip -t /tmp/new/test.gz`<br>
        `root@vagrant:~# echo $?` <br>
        `0` <br> 
> 16. `root@vagrant:~# pvmove /dev/md1 /dev/md0`
> 17. `root@vagrant:~# mdadm --fail /dev/md0 /dev/sdb1` <br>
`mdadm: set /dev/sdb1 faulty in /dev/md0`
> 18. `dmesg`<br>
[ 5468.811362] md/raid1:md0: Disk failure on sdb1, disabling device.<br>
               md/raid1:md0: Operation continuing on 1 devices.<br>
> 19.   `root@vagrant:~# gzip -t /tmp/new/test.gz`
        `root@vagrant:~# echo $?`
        `0`
> 20.   `vagrant destroy`
