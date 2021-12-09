> 1. Изучено

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