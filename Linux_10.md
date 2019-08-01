# Linux

## Shell



```
# vi /etc/profile
TOMCAT_HOME=/root/file/apache-tomcat-9.0.22
export TOMCAT_HOME

# . /etc/profile => profile 재가동
# echo $TOMCAT_HOME
/root/file/apache-tomcat-9.0.22

[shell]# vi case1.sh
#!/bin/sh
case "$1" in
  start | START | st) 
    echo "Start Tomcat..."
        $TOMCAT_HOME/bin/startup.sh
    echo "Started...";;
  stop)
    echo "Stop Tomcat..."
        $TOMCAT_HOME/bin/shutdown.sh
    echo "Stoped...";;
  check)
    echo "Check Tomcat..."
        ps -ef | grep tomcat
    echo "Checked...";;
esac
exit0
[shell]# case1.sh start(or stop or check)

[shell]# vi for1.sh
ctn=0
for i in $(ls *.sh)
do
  if [ $i = "var1.sh" ]
  then
        echo "OK: $i"
  fi
  ctn=`expr 1 + $ctn`
done
echo "COUNT: $ctn "
[shell]# for1.sh 
OK: var1.sh
COUNT: 12 

함수, argument, set
[shell]# vi while1.sh
startTomcat() {
  echo "Input Number is .. "$1
  echo "start Tomcat Function ..."
  return
}

stopTomcat() {
  echo "Input number is .. "$1 $2
  echo "stop Tomcat Function ..."
  return
}

echo "Tomcat Management Tool..."
while [ 1 ]
do
  echo "Input Command (start,stop,check,q)"
  read cmd

  case $cmd in
    start)
        startTomcat 10
        echo "Tomcat Started";;
    stop)
        stopTomcat 40 50
        echo "Tomcat Stopped";;
    q)
        echo "Exit"
        break;;
  esac
done
echo "Exit Shell Program"
[shell]# while1.sh
startTomcat() {
  echo "Input Number is .. " $1
  set $(date)
  echo $1 $2 $3 $4 $5 $6
  echo "start Tomcat Function ..."
  return
}

stopTomcat() {
  echo "Input number is .. "$1 $2
  echo "stop Tomcat Function ..."
  return
}

echo "Tomcat Management Tool..."
while [ 1 ]
do
  echo "Input Command (start,stop,check,q)"
  read cmd
  case $cmd in
    start)
        startTomcat 10
        echo "Tomcat Started";;
    stop)
        stopTomcat 40 50
        echo "Tomcat Stopped";;
    q)
        echo "Exit"
        break;;
  esac
done
echo "Exit Shell Program"
[shell]# while1.sh
Tomcat Management Tool...
Input Command (start,stop,check,q)
start
Input Number is ..  10
2019. 08. 01. (목) 10:40:12 KST
start Tomcat Function ...
Tomcat Started
Input Command (start,stop,check,q)
q
Exit
Exit Shell Program

```



```
[filetemp]# yum -y install wget*
[filetemp]# wget http://70.12.114.59/testa/tomcat.tar.gz
[filetemp]# wget http://70.12.114.59/testa/jdk1.8.tar.gz
[filetemp]# wget http://70.12.114.59/testa/eclipse.tar.gz

```



```
envset.sh
실행하면 jdk, tomcat, eclipse 다운받기

1. jdk 설치
/etc/jdk1.8
/usr/bin/java softlink

2. tomcat 설치
/etc/tomcat
/usr/bin/startcat softlink
/usr/bin/stopcat softlink

3. eclipse 설치
/etc/eclipse
/usr/bin/eclipse softlink

메뉴를 구성하여 설치를 진행한다.
단, 설치가 되어있을 경우 삭제 후 설치를 진행한다.
중간에 사용자에 물어 보면서 진행

```



```

1. Network Setiing - 70.12.114.211
2. /etc/yum.repos.d
3. /etc/sysconfig/selinux
4. Program install
5. profile setting
 1) JAVA_HOME
 2) CLASSPATH
 3) PATH
 4) TOMCAT_HOME
6. firewall config
systemctl restart network
- add in script
7. clone
```



----------------



## 하둡 프로그래밍 서버만들기





create - iso - - HADOOPSERVER1 C:\CENTOS\HADOOPSERVER1- 50 single

Memory 4GB, Processors cores 4

소프트웨어선택 GNOME데스크탑

설치대상 파티션설정 - 표준파티션, swap 4G, / ext4



DB : 70.12.114.210

```
# vi /etc/sysconfig/network-scripts/ifcfg-ens33 
BOOTPROTO="none"
IPADDR=70.12.114.211
NETMASK=255.255.255.0
GATEWAY=70.12.114.1
DNS1=168.126.63.1
# vi /etc/hosts
70.12.114.211 hadoopserver1
# systemctl restart network
# hostnamectl set-hostname hadoopserver1
# reboot
Network Adapter - Bridged On

# cd /etc/yum.repos.d/
# vi CentOS-Base.repo 
# vi CentOS-Sources.repo 
=> released updates 부분 삭제
[yum.repos.d]# wget http://download.hanbit.co.kr/centos/7/CentOS-Base.repo

# vi /etc/sysconfig/selinux
SELINUX = disabled

# vi /etc/profile
JAVA_PATH=/etc/jdk1.8
CLASSPATH=$JAVA_HOME/lib
TOMCAT_HOME=/root/installfiles/tomcat
export JAVA_HOME CLASSPATH TOMCAT_HOME
PATH=.:$JAVA_HOME/bin:$TOMCAT_HOME/bin:$PATH


# cd /root/installfiles/tomcat/conf
# vi server.xml 
PORT 80 변경

[installfiles]# wget -P /root/installfiles http://70.12.114.59/testa/eclipse.tar.gz

```





### clone

메모장 - VMware virtual machine configuration 열어서

displayName = "HADOOPSERVER2"변경

open

edit - network - advanced - generate

play - moved it

00:50:56:25:20:04



```
IP SETTING - 210
MAC ADDRESS SETTING
HOSTNAME
/etc/hosts
```

