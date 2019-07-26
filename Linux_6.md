# Linux

### 하드디스크 관리

* Personal 컴퓨터 하드디스크 IDE 방식(4개)
* 요즘에는 SATA 방식
* 서버는 SCSI 방식(16개)
* SA-SCSI 최대 65535개 연결가능



SERVER1 - Edit virtual machine settings - 

CD/DVD(IDE), Hard Disk(SCSI) - Advanced... 



##### Hard Disk 1GB 추가하기

add - hard disk - create - SCSI - 

single file(multiple files : access 속도를 빠르게 하기위해) - scsi0-1.vmdk 

```
[~]# ls /dev/sd*
/dev/sda  /dev/sda1  /dev/sda2  /dev/sdb
[~]# ls -l /dev/sd*
brw-rw---- 1 root disk 8,  0  7월 26  2019 /dev/sda
brw-rw---- 1 root disk 8,  1  7월 26  2019 /dev/sda1 => swap
brw-rw---- 1 root disk 8,  2  7월 26  2019 /dev/sda2 => root
brw-rw---- 1 root disk 8, 16  7월 26  2019 /dev/sdb

[~]# fdisk /dev/sdb => fdisk 파티션Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
n / p / 1 / enter / enter / p / w

Command action
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
   g   create a new empty GPT partition table
   G   create an IRIX (SGI) partition table
   l   list known partition types
   m   print this menu
   n   add a new partition
   o   create a new empty DOS partition table
   p   print the partition table
   q   quit without saving changes
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit
   x   extra functionality (experts only)

[~]# mkfs.ext4 /dev/sdb1 => 포맷
[~]# mkdir /mydata
[~]# mount /dev/sdb1 /mydata => sdb1을 mydata로 마운트를 건다
mydata 폴더는 1GB 하드가 됨
[~]# umount /dev/sdb1 => 분리

[~]# mount /dev/sdb1 /mydata
[~]# df
[~]# vi /etc/fstab 
/dev/sdb1       /mydata  ext4   defaults        1 2

```

--------

##### server2에 HDD 2개를 설치 하고 

##### HDD 1G(1)

##### HDD 2G(2개) 둘다 Primary

##### /data1

##### /data2

##### /data3에 mount하시오

```
[~]# ls /dev/sd*
# fdisk /dev/sdb
# mkfs.ext4 /dev/sdb1
# mkdir /data1
# mount /dev/sdb1 /data1
# df
# vi /etc/fstab 
/dev/sdb1    /data1  ext4   defaults    1 2

# fdisk /dev/sdc
   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048     2097151     1047552   83  Linux
/dev/sdc2         2097152     4194303     1048576   83  Linux

# mkfs.ext4 /dev/sdc1
# mkfs.ext4 /dev/sdc2
# mkdir /data2
# mkdir /data3
# mount /dev/sdc1 /data2
# mount /dev/sdc2 /data3
# df
# vi /etc/fstab
/dev/sdc1    /data2  ext4   defaults    1 2
/dev/sdc2    /data3  ext4   defaults    1 2




```
