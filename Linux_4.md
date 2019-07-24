# Linux



### Server3

1. JDK 설치

- /usr/bin/java, symbolic link

2. /etc/profile 환경 설정
3. eclipse 설치

- /usr/bin/eclipse, symbolic link

4. tomcat 설치


--------------------------

### --

1. network 설정

* /etc/sysconfig/network-scripts/ifcfg-ens33
* systemctl restart network



2. user 설정

* useradd
* permision
* chmod, chown, chgrp



3. tar, zip(unzip)

* cvf 묶는거, xvf 푸는거



4. rpm, yum

* yum(오픈소스만 가능) - localinstall, install(yum 서버에 접속해서 다운로드)



5. Program install

* /etc/profile 환경설정(JAVA_HOME, CLASSPATH)
* ln -s (Symbolic, /usr/bin)
* firewall-config



----------------

### 백업 

##### cron

```
[~]# vi /etc/crontab 

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
  10 11 24 * * root cp -r /home /backup -> 매월 24일 11시10분에 root권한(계정)으로 home폴더를 backup폴더로 복사해라
[~]# systemctl status crond
# systemctl restart crond


# cd /etc/
[etc]# ls cron*
cron.deny  crontab

cron.d:
0hourly  pcp-pmie  pcp-pmlogger  raid-check  sysstat  unbound-anchor

cron.daily:
0yum-daily.cron  certwatch  logrotate  man-db.cron  mlocate

cron.hourly:
0anacron  0yum-hourly.cron

cron.monthly:

cron.weekly:


run-parts 명령어는 다음에 나오는 디렉터리 안의 명령을 모두 실행한다.

```



##### at

몇시 몇분에 무엇을 해라(예약을 하면 한번만 실행되고 소멸된다.)

```
[~]# rdate -s time.bora.net => time.bora.net에 가서 시간을 가져와라

[~]# at 11:25 am today 
at> cp -r /home /backup
at> reboot<EOT>					Ctrl+d종료하면 <EOT>나옴
job 1 at Wed Jul 24 11:25:00 2019

# at -l
1	Wed Jul 24 11:25:00 2019 a root


```



매월 24일 12시30분에 실행하기

```
# vi /etc/crontab
	30 12 24 * * root run-parts /etc/cron.monthly
# cd /etc/cron.monthly/

[cron.monthly]# touch myBackup.sh
# chmod 755 myBackup.sh 
# ls -l
합계 0
-rwxr-xr-x 1 root root 0  7월 24 11:32 myBackup.sh

# vi myBackup.sh 
#!/bin/sh
set $(date)
fname="backup-$2$3.tar.xz"
tar cfJ /backup/$fname /home

# systemctl restart crond
```





### 네트워크

* 프로토콜 : 네트워크상으로 의사소통을 하는 약속, 규칙

##### TCP/IP

hostname

hostnamectl set-name server1

/etc/hosts

192.168.111.100 server1 -> IP 어드레스를 대신함

##### 도메인

: public, 누구나 밖에서 접속할 수 있도록, IP 어드레스를 대신함





### 오라클 DB 설치

```
[file]# unzip oracle-xe-11.2.0-1.0.x86_64.rpm.zip 
# cd Disk1/


(가상메모리 필요없으면 swapoff/swapfile 실행해 해제한 후 swapfile 파일 삭제)
swap 늘려주기
[Disk1]# dd if=/dev/zero of=/swapfile bs=1024 count=4194304
4194304+0 records in
4194304+0 records out
4294967296 bytes (4.3 GB) copied, 10.3586 s, 415 MB/s
[Disk1]# mkswap /swapfile
Setting up swapspace version 1, size = 4194300 KiB
no label, UUID=5a726b02-42e6-48a4-85e3-b4c49dcfd128
[Disk1]# swapon /swapfile
swapon: /swapfile: insecure permissions 0644, 0600 suggested.
Disk1]# swapon -s
Filename				Type		Size	Used	Priority
/dev/sda1                              	partition	2047996	75024	-1
/swapfile                              	file	4194300	0	-2

Disk1]# cd /etc/rc.d/
[rc.d]# ls -l rc.local 
-rw-r--r--. 1 root root 477  6월 10  2014 rc.local
[rc.d]# chmod 755 rc.local 
[rc.d]# chmod 755 rc.local 
touch /var/lock/subsys/local
swapon /swapfile
[rc.d]# reboot

[~]# swapon -s
Filename				Type		Size	Used	Priority
/dev/sda1                              	partition	2047996	0	-1
/swapfile        
[~]# cd /root/file/Disk1/
[root@server1 Disk1]# ls -l
합계 309884
-rw-rw-r-- 1 root root 317320273  8월 29  2011 oracle-xe-11.2.0-1.0.x86_64.rpm
drwxr-xr-x 2 root root        19  8월 29  2011 response
drwxrwxr-x 2 root root        25  8월 29  2011 upgrade
[Disk1]# yum -y localinstall oracle*
Installed:
  oracle-xe.x86_64 0:11.2.0-1.0  
Complete!

[~]# service oracle-xe configure
[8080]enter, [1521]enter, pwd enter, pwd enter, y enter
Installation completed successfully.

[~]# /etc/init.d/oracle-xe start
Starting oracle-xe (via systemctl):                        [  OK  ]
[~]# . /u01/app/oracle/product/11.2.0/xe/bin/oracle_env.sh 

[~]# vi /etc/bashrc
# vim:ts=4:sw=4
. /u01/app/oracle/product/11.2.0/xe/bin/oracle_env.sh
[~]# firewall-config
영구적 - 포트 1521 tcp, 포트 8080 tcp 추가 - 옵션 - Firewall 다시 불러오기

```

