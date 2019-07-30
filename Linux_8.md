# Linux

## 6.3 LVM

Server3

Logical Volume Manager 논리 하드디스크 관리자

두개의 디스크 합친 것을 여러개로 나누어 사용할 수 있다

각각의 나눠진 영역 : 디렉토리

사용자들에게 할당되어지는 디스크, 가변적



##### 순서

fdisk -> vgcreate(하나로 묶기) -> lvcreate(나누기) -> mkfs.ext4(포맷) -> mount -> /etc/fstab -> reboot



```
[~]# fdisk /dev/sdb
n p 1 e e t 8e p w

[~]# pvcreate /dev/sdb1
  Physical volume "/dev/sdb1" successfully created
[~]# pvcreate /dev/sdc1
[~]# vgcreate myVG /dev/sdb1 /dev/sdc1
[~]# vgdisplay

[~]# lvcreate --size 1G --name myLG1 myVG
 Logical volume "myLG1" created
[~]# lvcreate --size 3G --name myLG2 myVG
 Logical volume "myLG2" created
[~]# lvcreate --extents 100%FREE --name myLG3 myVG
 Logical volume "myLG3" created

[~]# mkfs.ext4 /dev/myVG/myLG1
[~]# mkfs.ext4 /dev/myVG/myLG2
[~]# mkfs.ext4 /dev/myVG/myLG3

# mkdir /lvm1
# mkdir /lvm2
# mkdir /lvm3

# mount /dev/myVG/myLG1 /lvm1
# mount /dev/myVG/myLG2 /lvm2
# mount /dev/myVG/myLG3 /lvm3

# vi /etc/fstab
/dev/myVG/myLG1 /lvm1   ext4    defaults        1       2
/dev/myVG/myLG2 /lvm2   ext4    defaults        1       2
/dev/myVG/myLG3 /lvm3   ext4    defaults        1       2

reboot

```



* HDD 3개를 신규로 탑재한다.(3G, 3G, 4G)

  3개의 하드를 LVM으로 나누시오

  2G,2G,2G,2G,2G

  /lm1(2G)   /lm2(2G)   /lm3(2G)   /lm4(2G)   /lm5(2G)





## 6.4 RAID에 CentOS 설치하기

RAID1에 Mirroring을 하고 disk1을 remove해도 실행 됨



* RAID1 가상머신에 새로운 하드디스크를 장착하고 RADI1 가상머신이 다시 결함 허용을 하도록 원상복구하기
  * 추가한 하드디스크는 SCSI 0:2로 변경하기

```
# mdadm --add /dev/md/swap /dev/sdb1
# mdadm --add /dev/md/root /dev/sdb2
```





