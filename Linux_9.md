# Linux

DB 켜두고 RAID2의 디스크 하나를 제거해도, Mirroring으로 작동 되는지 확인하기



### 공유폴더만들기

```
yum -y install cifs*
mount -t cifs //70.12.114.59/file ~/file
```





## 브릿지

DB : 70.12.114.210

IPADDR : 70.12.114.211

NETMASK : 255.255.255.0

GATEWAY : 70.12.114.1

DNS : 168.126.63.1



```
cmd
> rundll32 "C:\Program Files <x86>\VMware\VMware Player\vmnetui.dll" VMNetUI_ShowStandalone
Brideged to : Intel(R)~ 변경 후 OK

edit 0 Network Adapter - Bridged
vi /etc/sysconfig/network-scripts/ifcfg-ens33
vi /etc/hosts
IPADDR 수정
systemctl restart network

```



```
# firewall-cmd --zone=public --add-port=1521/tcp --permanent
```





## 7 셸 스크립트 프로그래밍



```
[~]# mkdir shell
[shell]# vi temp
#!/bin/sh
~~
exit 0(esc -> :set nu)
[shell]# chmod 744 *
[shell]# cp name.sh /usr/bin/
=> 어디서든 name.sh를 실행할 수 있음

[shell]# vi var1.sh
v1="String..."
echo $v1
echo "$v1"
echo '$v1'
echo \$v1
echo 3+4
[shell]# var1.sh 
String...
String...
$v1
$v1
3+4

[shell]# vi var2.sh
echo "Input number...?"
read v1 v2
echo "Input number is $v1 $v2"
echo "Input number is "$v1
[shell]# var2.sh
Input number...?
3 5
Input number is 3 5

특수문자에는 \을 붙여줘야함
[shell]# vi var3.sh
n1=`expr 100 + 111`
echo $n1
n2=`expr \( $n1 + 100 \) \* 2`
echo $n2
[shell]# var3.sh
211
622

2자릿수 4개의 숫자를 입력받아 합과 평균을 구하시오
[shell]# vi var4.sh
echo "Input 4 number...?"
read v1 v2 v3 v4
echo "Input number is $v1 $v2 $v3 $v4"
sum=`expr $v1 + $v2 + $v3 + $v4`
echo "sum = "$sum
avg=`expr $sum \/ 4`
echo "avg = "$avg

```



```
[shell]# vi para1.sh
echo $0
echo $1
echo $2
echo $3
[shell]# para1.sh 10 20 30
./para1.sh
10
20
30

para1.sh 10 20 30 40 평균을 출력 하시오
echo $0
echo $1
echo $2
echo $3
echo $4

avg=`expr \( $1 + $2 + $3 + $4 \) \/ 4`
echo "avg = " $avg


home.tar.xz로 만들어 mybackup폴더로 이동
[shell]# vi para2.sh
tar cvfJ $1.tar.xz $1
mv $1.tar.xz $2
echo "end.."
[shell]# para2.sh /home /mybackup


root 디렉토리에서 sh 파일을 모두 찾아 /shfile에 복사 한다. 단, /shfile은 직접 만드시오
[shell]# vi para3.sh
echo "start ..."
find $1 -name "*.$2" -exec cp {} $3 \;
echo "end.."
[shell]# para3.sh /root sh /shfile
```



```
[shell]# vi if1.sh
temp=$1
if [ $temp = "ok" ]
then
  echo "OK!!!"
else
  echo "No!!!"
fi
[shell]# if1.sh ok
OK!!!
[shell]# if1.sh okk
NO!!!

실행하면 tomcat이 설치되어있으면 ok 출력하고 tomcat 실행한다. 실행 후 실행된 프로세스를 출력 한다.(ps)
[shell]# vi if2.sh
echo "start..."
tomcat=/root/file/apache-tomcat-9.0.22
if [ -e $tomcat ]
then
  echo "ok"
  $tomcat/bin/startup.sh
  ps -ef | grep tomcat
else
  echo "Not installed..."
fi
echo "end...
[shell]# if2.sh


```

