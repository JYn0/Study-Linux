# Linux

## 명령어



* execute

```
touch runls.sh

vi
ls -l /etc
chmod 764 runls.sh
./runls.sh

./ 현재 폴더 밑에

cd
/home/nuser1
vi .bashrc
PATH=.:$PATH
export PATH
. ./.bashrc -> 현재 디렉터리 밑에있는 bashrc를 다시 가동해라

cd /home/allfile
runls.sh
```



* 허가권

```
chown -> 파일에 사용
chgrp mulit mudir -> mudir을 multi그룹에게 주겠다
chmod 770 mudir -> multi그룹만 접근 가능
x -> 폴더 접근
```



* 링크
  * 하드 링크
    * 파일사이즈가 동일하게 만들어지면서 링크가 걸리는 것
    * 동일한 형태의 파일을 다른 공간에 만들어 연결해서 사용하는 것
  * 심볼릭 링크
    * 바탕화면 바로가기 같은 기능
    * 원본파일의 위치만 지정



```
# ln linktest/ltest hlink
linktest밑에 ltest파일을 하드링크로 만들겠다
# ln -s linktest/ltest slink
linktest밑에 ltest파일을 심볼릭링크로 만들겠다
# ls -il
37275818 -rw-------  2 root root 1525  7월 23 10:09 hlink
 37276492 lrwxrwxrwx  1 root root   14  7월 23 10:11 slink -> linktest/ltest

# cd linktest
# ls -il
37275818 -rw------- 2 root root 1525  7월 23 10:09 ltest
같은 inode블록을 가리킴

# rm * (ltest 지우기)
# cd ..
# ls -il
37275818 -rw-------  1 root root 1525  7월 23 10:09 hlink => 파일의 보존성
37276492 lrwxrwxrwx  1 root root   14  7월 23 10:11 slink -> linktest/ltest => 기능X 의미X


/root
# java -version
java version "1.7.0_51"
OpenJDK Runtime Environment (rhel-2.4.5.5.el7-x86_64 u51-b31)
OpenJDK 64-Bit Server VM (build 24.51-b03, mixed mode)

# vi.bashrc (/usr/bin/vi ./.bashrc)
PATH=.:$PATH;
export PATH
# . ./.bashrc
$PATH
bash: .:.:/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:/sbin:/root/bin: 그런 파일이나 디렉터리가 없습니다
#which java -> java의 위치
/usr/bin/java

# cd /usr/bin
# ls -la java
lrwxrwxrwx. 1 root root 22  7월 19 02:18 java -> /etc/alternatives/java
허가권 맨 앞에 l과 화살표 -> 심볼릭
symbolic link의 대표적인 예
어디서든 java를 실행하면 /etc/alternatives/java가 실행됨


```



자바 링크파일로 만들기

java.sun.com

Java SE Development Kit 8u221

Linux x64 tar.gz download

```
# tar xvf jdk-8u221-linux-x64.tar.gz 
# mv jdk1.8.0_221 jdk1.8
/root/file
# cp -r jdk1.8 /etc

# cd /usr/bin
# ln -s /etc/jdk1.8/bin/java java
java로 심볼릭링크를 걸겠다

# java -version
java version "1.8.0_221"
Java(TM) SE Runtime Environment (build 1.8.0_221-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.221-b11, mixed mode)

/root
# vi .bashrc
JAVA_HOME=/opt/jdk1.8;
PATH=.:/JAVA_HOME/bin:$PATH;
CLASSPATH=$JAVA_HOME/lib;
export PATH CLASSPATH JAVA_HOME
```





## 관리자 명령어.

* RPM
* YUM

```
/root/file
# rpm -e gedit-3.8.3* -> gedit 지우기
# cp /run/media/root/CentOS\ 7\ x86_64/Packages/gedit-3.8.3-6.el7.x86_64.rpm .
# rpm -Uvh gedit-3.8.3-6.el7.x86_64.rpm  -> rpm으로 설치
# which gedit 
/usr/bin/gedit
```

```
[file]# cp /run/media/root/CentOS\ 7\ x86_64/Packages/mysql-connector-odbc-5.2.5-6.el7.x86_64.rpm .
// # yum remove my mysql
// # yum remove mysql-connector-odbc
// [~]# yum -y install mysql-connector-odbc
// yum remove mysql
[file]# yum localinstall mysql-connector-odbc-5.2.5-6.el7.x86_64.rpm
// package info

localinstall -> 내가 갖고있을때
install -> 서버에 가서 설치
yum remove odbc
```

```
cd
# yum -y groupinstall "MariaDB Database Server"
```

```
[~]# cp anaconda-ks.cfg arcive
# mkdir atest
# cp arcive atest/archive
[atest]# cp archive archive1
[atest]# cp archive archive2
[atest]# cp archive archive3
[atest]# cp archive archive4
# xz archive1
# bzip2 archive2
# gzip archive3
# zip archive4
압축은 하나의 파일만 가능
여러개 파일 불가
# tar cvf atest.tar atest
폴더를 하나로(디렉터리를 묶고)
# xz atest.tar -> 압축

# tar cvfz atest.tar.gzip atest -> 묶고
# rm -r atest
# tar xvfz atest.tar.gzip -> gzip 압축을 풀고 하나로 묶은걸 푼다
```

```
cd
# find /etc -name *.conf
# find /home -user user1
# find /usr/bin -perm 644
# find /usr/bin -size +10k -size -100k
~
# touch a1
# touch a2
# touch a3
# touch a4
# find ~ -size 0k
# find ~ -size 0k -exec ls -l {} \;
# mkdir temp
# find ~ -size 0k -exec cp {} temp \; -> cp 뭐를 여기에
폴더의 위치 찾을때
# which java
# whereis java
# locate java
```

```
# nmtui 네트워크수정
# firewall-config
포트 - 포트/포트범위:1521 tcp(설정: 런타임,영구적)
```





```
eclipse 설치
# tar xvf eclipse-jee-2019-06-R-linux-gtk-x86_64.tar.gz 
# tar xvf apache-tomcat-9.0.22.tar.gz 

eclipse 폴더를 /etc/eclipse로 복사하고
[etc]# cp -r /root/file/eclipse .
/usr/bin/eclipse 파일을 Symbolic link를 거시오
[~] cd /usr/bin
[bin]# ln -s /etc/eclipse/eclipse eclipse
[bin]# ls -l eclipse 
lrwxrwxrwx 1 root root 20  7월 24 00:37 eclipse -> /etc/eclipse/eclipse

```



```
vi /etc/profile

HOSTNAME=`/usr/bin/hostname 2>/dev/null`
HISTSIZE=1000
if [ "$HISTCONTROL" = "ignorespace" ] ; then
    export HISTCONTROL=ignoreboth
else
    export HISTCONTROL=ignoredups
fi

JAVA_HOME=/etc/jdk1.8
export JAVA_HOME
CLASSPATH=$JAVA_HOME/lib
export CLASSPATH
PATH=.:$JAVA_HOME/bin:$PATH

export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL

reboot
```



```
/root/file/apache-tomcat-9.0.22
# cd conf
# vi server.xml
:set nu 68번 Connector port="80"

/root/file/apache-tomcat-9.0.22/webapps
/root/file/apache-tomcat-9.0.22/bin
실행 : startup.sh
종료 : shutdown.sh

# firewall-config 
서비스 http 체크(런타임, 영구적)

[root@server2 bin]# startup.sh 
Using CATALINA_BASE:   /root/file/apache-tomcat-9.0.22
Using CATALINA_HOME:   /root/file/apache-tomcat-9.0.22
Using CATALINA_TMPDIR: /root/file/apache-tomcat-9.0.22/temp
Using JRE_HOME:        /etc/jdk1.8
Using CLASSPATH:       /root/file/apache-tomcat-9.0.22/bin/bootstrap.jar:/root/file/apache-tomcat-9.0.22/bin/tomcat-juli.jar
Tomcat started.

=> 80포트로 웹서버를 띄움

```







