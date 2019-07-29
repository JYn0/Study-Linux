# Linux

## 6.2 RAID

* Hard Disk 2(SCSI) 지우기 -> fstab 수정하고 마운트 해제하고 삭제하기

  ```
  # vi /etc/fstab
  # /dev/sdb1       /mydata  ext4   defaults        1 2 -> # 주석
  edit -> 클릭 -> remove
  ```



#### 여러개의 하드디스크를하나의 하드디스크처럼 사용하는 방식

#### => RAID와 LVM



### RAID 

(Redundant Array of Inexpensive/Independent Disks,

 저렴하게 독립적인 디스크들을 하나로 묶어서 사용한다)

* 비용을 줄이면서 성능을 향상시킨다



##### Linear RAID

* 순차적
* 신뢰성 보장 X

##### RAID0

* 속도 빠름
* 동시저장
* 복구 불능

##### RAID1

* 디스크 복구 가능
* Mirroring

##### RIAD5

* 패리티 사용
* 디스크 2장이 깨지면 복구 불가

##### RAID6

* ​





### Server1에 디스크 9개 붙이기

```
edit - add disk0-1 ~ 0-10(7빼고)
[~]# ls -l /dev/sd*
brw-rw---- 1 root disk 8,   0  7월 29  2019 /dev/sda
brw-rw---- 1 root disk 8,   1  7월 29  2019 /dev/sda1
brw-rw---- 1 root disk 8,   2  7월 29  2019 /dev/sda2
brw-rw---- 1 root disk 8,  16  7월 29  2019 /dev/sdb
brw-rw---- 1 root disk 8,  32  7월 29  2019 /dev/sdc
brw-rw---- 1 root disk 8,  48  7월 29  2019 /dev/sdd
brw-rw---- 1 root disk 8,  64  7월 29  2019 /dev/sde
brw-rw---- 1 root disk 8,  80  7월 29  2019 /dev/sdf
brw-rw---- 1 root disk 8,  96  7월 29  2019 /dev/sdg
brw-rw---- 1 root disk 8, 112  7월 29  2019 /dev/sdh
brw-rw---- 1 root disk 8, 128  7월 29  2019 /dev/sdi
brw-rw---- 1 root disk 8, 144  7월 29  2019 /dev/sdj

[~]# fdisk /dev/sdc~j
n p 1 e e t fd
[~]# fdisk /dev/sdb 
d(delete partition) w

[~]# mdadm --create /dev/md9 --level=linear  --raid-devices=2 /dev/sdb1 /dev/sdc1
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md9 started.
=> md9 폴더를 만들어서 리니어 레벨로, 디바이스 2개 sdb1과 sdc1을 가지고 포맷

reboot

[~]# ls -l /dev/md9
brw-rw---- 1 root disk 9, 9  7월 29  2019 /dev/md9 => 포맷완료
[~]# mdadm  --detail --scan => 두개가 합쳐진 물리적인 디스크
ARRAY /dev/md/9 metadata=1.2 name=server1:9 UUID=f4865dc5:e51fec2e:a5789da6:055b874f

[~]# mkfs.ext4 /dev/md9  => 포맷하기
[~]# mount /dev/md9 /linear => md9을 linear에 마운트
# df
/dev/md9         3028736     9216   2845956   1% /linear

[~]# mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/sdd1 /dev/sde1

해제하기
# vi /etc/fstab
# umount /dev/md9
# mdadm --stop /dev/md9
[~]# rm -rf /dev/md/*

다시 만들기
[~]# mdadm --create /dev/md9 --level=linear  --raid-devices=2 /dev/sdb1 /dev/sdc1
[~]# mkfs.ext4 /dev/md9
[~]# mkdir /linear
[~]# mount /dev/md9 /linear
[~]# df
/dev/md9         3028736     9216   2845956   1% /linear
[~]# vi /etc/fstab
[~]# df
/dev/md9         3028736     9216   2845956   1% /linear => 3GB
/dev/md0         2028368     6144   1901136   1% /raid0 => 2GB
/dev/md1         1014104     2564    942808   1% /raid1 => 1GB(1GB: 백업)
/dev/md5         2027408     6144   1900228   1% /raid5 => 2GB(1GB: 패리티, 빈공간)

```

* 각각의 Disk fdisk - fd
* mdadm - 디스크 묶기
* mkfs => 포맷
* forder 생성
* mount
* /etc/fstab




### 문제 발생과 조치 방법

(p.361)

```
[boot]# cp vmlinuz-3.10.0-123.el7.x86_64 /linear/testfile
reboot
edit - delete 3,5,7,9
pwd : 111111
vi /etc/fstab
linear, 0 -> 주석처리
[/]# mdadm --run /dev/mde9
[~]# mdadm --stop /dev/md9, 0
[~]# mdadm --run /dev/md5 => 복구
[~]# df
reboot

[~]# df
/dev/md5         2027408    10932   1895440   1% /raid5
/dev/md1         1014104     7352    938020   1% /raid1

```



##### 고장 난 하드디스크를 새로운 하드디스크로 교체하기

edit - SCSI 0:2, 0:4, 0:6, 0:9 다시 만들기

```
# fdisk /dev/sdc, sde, sdg, sdi
n p 1 e e t fd w
# ls /dev/sd*
# mdadm --stop /dev/md9
[~]# mdadm --create /dev/md9 --level=linear --raid-devices=2 /dev/sdb1 /dev/sdc1
[~]# mdadm --stop /dev/md0
[~]# mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/sdd1 /dev/sde1

[~]# mdadm /dev/md1 --add /dev/sdg1
mdadm: added /dev/sdg1
[~]# mdadm /dev/md5 --add /dev/sdi1
mdadm: added /dev/sdi1
# mdadm --detail /dev/md1
# mdadm --detail /dev/md5

# vi /etc/fstab
주석 해제

reboot


```



