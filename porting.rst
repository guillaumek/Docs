################################
Porting Volumio to new Platforms
################################


Here you'll find the standard procedure I'll follow to port Volumio on newer Platforms. I designed Volumio's filesystem
to be easily adapted to various ArmHF capable platforms, this was initially time consuming, but it payed in the long run, 
as a new porting will require a limited amount of time.
I won't cover here the process of creating Volumio's rootfs. I will write about that in the future. 



download image
write image to sd 
resize image to 1,4 gb
download volumio's tar 


fdisk -l


Disk /dev/mmcblk0: 7948 MB, 7948206080 bytes
22 heads, 16 sectors/track, 44101 cylinders, total 15523840 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x125dde26

        Device Boot      Start         End      Blocks   Id  System
/dev/mmcblk0p1            2048     4194303     2096128   83  Linux

Disk /dev/sda: 7742 MB, 7742685184 bytes
239 heads, 62 sectors/track, 1020 cylinders, total 15122432 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x000a86b5

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048      206847      102400    c  W95 FAT32 (LBA)
/dev/sda2          206848     3074047     1433600   83  Linux


mkdir /mnt/1

mount /dev/sda2 /mnt/1

cd /mnt/1

rm -rf *

sync

cd /

tar xvf vol    -C /mnt/1




cd /mnt/1

Now copy modules, kernel and /udev rules if any and you're ready


cd /
umount /dev/sda2




