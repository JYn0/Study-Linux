# Linux

### + Eclipse 

```
[~]# /etc/init.d/oracle-xe status  -> 리눅스가 잘 돌고 있는지
[~]# ps -ef | grep oracle

# ps -ef | grep tomcat
root      9303  6817  0 11:06 pts/0    00:00:00 grep --color=auto tomcat
=> tomcat이 떠있는지 확인 안떠있음
[apache-tomcat-9.0.22]# cd webapps/
[webapps]# ls
ROOT  docs  examples  host-manager  manager
[webapps]# cp ~/file/*.war . => war 가져오기
[webapps]# ls
ROOT  docs  examples  host-manager  manager  testa.war 
[bin]# startup.sh 
[webapps]# ls
ROOT  docs  examples  host-manager  manager  testa  testa.war
192.168.111.111/testa

new dynamic web project -> Context root:/
root는 한개만

[webapps]# cp ~/file/testa2.war .
[webapps]# ls
ROOT  examples      manager  testa.war  testa2.war
docs  host-manager  testa    testa2
# mv ROOT ROOT_BACK => 기존에 있던 ROOT를 지우고
[webapps]# ls
ROOT_BACK  examples      manager  testa.war  testa2.war
docs       host-manager  testa    testa2
=> 톰캣이 돌고있어서 testa2가 자동으로 전개됨

[webapps]# mv testa2 ROOT => testa2 이름을 ROOT로 변경해줘야됨

[root@server1 webapps]# ls
ROOT       docs      host-manager  testa      testa2.war
ROOT_BACK  examples  manager       testa.war

```



```
package oracledb;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class Test {

	public static void main(String[] args) 
	throws Exception{
		String id = "db"; // 스키마 name
		String pwd = "db";
		String url = "jdbc:oracle:thin:@192.168.111.111:1521:XE";
		
		Class.forName("oracle.jdbc.OracleDriver");
		
		Connection con = DriverManager.getConnection(url, id, pwd);
		
		String sql = "SELECT * FROM DEPT";
		
		PreparedStatement pstmt = con.prepareStatement(sql);
		
		ResultSet rset = pstmt.executeQuery();
		
		while(rset.next()) {
			int deptno = rset.getInt("DEPTNO");
			String dname = rset.getString("DNAME");
			String loc = rset.getString("LOC");
			System.out.println(deptno+" "+dname+" "+loc);
		}
	}

}

```





----





### 관리자 명령어



##### | 파이프, grep 필터, 리다이렉션

```
| : 2개의 프로그램을 연결해주는 연결 통로
[~]# ls -l /etc | more 
# ps
# ps -ef

grep -> 필터링
# ps -ef | grep oracle
[~]# ps -ef | grep java
root     14799
# kill 14799
# kill -9 강제종료
# ls -la /usr/bin | grep java

리다이렉션 : 화면에 출력된 내용을 파일로 만든다.
# ls -al /etc > 20190725.txt -> 터미널에서 실행된 내용들을 파일로 만들기
# sort < 20190725.txt 
# sort < 20190725.txt > sort20190705.txt -> sort된 결과를 다시 파일로 저장

```



##### 프로세스, 데몬, 서비스

* 포그라운드프로세스(Foreground Process) : 실행하면 화면에 나타나서 사용자와 상호작용을 하는 프로세스, 화면에서 실행되는 것이 보이는 프로세스
* 백그라운드프로세스(Background Process) : 화면에 나타나지 않고 뒤에서 실행되는 프로세스, 톰캣, 오라클



```
ps : 현재 프로세스의 상태를 확인하는 명령어
# gedit & => &은 백그라운드에서 돌아라

```





---------



### Maria DB (Mysql) 다운

<http://ftp.hosteurope.de/mirror/archive.mariadb.org/mariadb-10.0.15/yum/centos7-amd64/rpms/>

client, common, server



```
[maria]# pwd
/root/file/maria
# yum -y remove mariadb-libs
# yum -y localinstall Maria*
Installed:
  MariaDB-client.x86_64 0:10.0.15-1.el7.centos
  MariaDB-common.x86_64 0:10.0.15-1.el7.centos
  MariaDB-server.x86_64 0:10.0.15-1.el7.centos
Complete!

# systemctl restart mysql
# systemctl status mysql
# chkconfig mysql on => 서비스 상시 가동
# firewall-config
public - 서비스 - mysql 체크(런타임, 영구적)

# mysql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 3
Server version: 10.0.15-MariaDB MariaDB Server
Copyright (c) 2000, 2014, Oracle, SkySQL Ab and others.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
MariaDB [(none)]> exit
Bye

# mysqladmin -u root password '111111'
# mysql -u root -p => root가 pwd를 입력하고 들어가겠다
MariaDB [(none)]> USE mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed

MariaDB [mysql]> SELECT user,host from user;
+------+-----------+
| user | host      |
+------+-----------+
| root | 127.0.0.1 |
| root | ::1       |
|      | localhost |
| root | localhost |
|      | server1   |
| root | server1   |
+------+-----------+
6 rows in set (0.00 sec)

MariaDB [mysql]> GRANT ALL PRIVILEGES ON *.* TO user1@'192.168.111.%' IDENTIFIED BY '111111';
Query OK, 0 rows affected (0.00 sec)
=> 하나의 사용자 만들어짐

MariaDB [mysql]> GRANT ALL PRIVILEGES ON *.* TO user1@'70.12.114.%' IDENTIFIED BY '111111';
Query OK, 0 rows affected (0.00 sec)

MariaDB [mysql]> SELECT user,host from user;
+-------+---------------+
| user  | host          |
+-------+---------------+
| root  | 127.0.0.1     |
| user1 | 192.168.111.% |
| user1 | 70.12.114.%   |
| root  | ::1           |
|       | localhost     |
| root  | localhost     |
|       | server1       |
| root  | server1       |
+-------+---------------+
8 rows in set (0.01 sec)



[root@server1 maria]# mysql -u root -p
Enter password: 

MariaDB [(none)]> USE mysql
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [mysql]> GRANT ALL PRIVILEGES ON *.* TO user1@'localhost' IDENTIFIED BY '111111';
Query OK, 0 rows affected (0.00 sec)

MariaDB [mysql]> SELECT user,host from user;+-------+---------------+
| user  | host          |
+-------+---------------+
| root  | 127.0.0.1     |
| user1 | 192.168.111.% |
| user1 | 70.12.114.%   |
| root  | ::1           |
|       | localhost     |
| root  | localhost     |
| user1 | localhost     |
|       | server1       |
| root  | server1       |
+-------+---------------+
9 rows in set (0.00 sec)


[maria]# mysql -u root -p
Enter password: 

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
+--------------------+
4 rows in set (0.00 sec)

> create database shop;
> use shop;

MariaDB [shop]> CREATE TABLE PRODUCT(
    ->    ID INT PRIMARY KEY,
    ->    NAME NVARCHAR(20) NOT NULL,
    ->    PRICE INT NOT NULL,
    ->    REGDATE DATE 
    -> );
Query OK, 0 rows affected (0.20 sec)

MariaDB [shop]> SELECT * FROM PRODUCT;
+-----+--------+-------+------------+
| ID  | NAME   | PRICE | REGDATE    |
+-----+--------+-------+------------+
| 100 | pants1 | 10000 | 2019-07-25 |
| 101 | pants2 | 20000 | 2019-07-25 |
| 102 | pants3 | 30000 | 2019-07-25 |
| 103 | pants4 | 40000 | 2019-07-25 |
| 104 | pants5 | 50000 | 2019-07-25 |
+-----+--------+-------+------------+
5 rows in set (0.00 sec)

MariaDB [shop]> INSERT INTO PRODUCT VALUES (105, '바지5', 50000, SYSDATE());
Query OK, 1 row affected, 1 warning (0.00 sec)

```

```SQL
CREATE TABLE PRODUCT(
   ID INT PRIMARY KEY,
   NAME NVARCHAR(20) NOT NULL,
   PRICE INT NOT NULL,
   REGDATE DATE 
);

INSERT INTO PRODUCT VALUES (100, 'pants1', 10000, SYSDATE());
INSERT INTO PRODUCT VALUES (101, 'pants2', 20000, SYSDATE());
INSERT INTO PRODUCT VALUES (102, 'pants3', 30000, SYSDATE());
INSERT INTO PRODUCT VALUES (103, 'pants4', 40000, SYSDATE());
INSERT INTO PRODUCT VALUES (104, 'pants5', 50000, SYSDATE());

```





<https://downloads.mariadb.com/Connectors/java/connector-java-2.4.2/>

[mariadb-java-client-2.4.2.jar](https://downloads.mariadb.com/Connectors/java/connector-java-2.4.2/mariadb-java-client-2.4.2.jar) => 다운로드

oracledb - properties - build and path - add external JARs

```java
package oracledb;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class Test2 {

	public static void main(String[] args) 
	throws Exception{
		String id = "user1";
		String pwd = "111111";
		String url = "jdbc:mariadb://192.168.111.111:3306/shop";
		
		Class.forName("org.mariadb.jdbc.Driver");
		
		Connection con = DriverManager.getConnection(url, id, pwd);
		
		String sql = "SELECT * FROM PRODUCT";
		
		PreparedStatement pstmt = con.prepareStatement(sql);
		
		ResultSet rset = pstmt.executeQuery();
		
		while(rset.next()) {
			int iid = rset.getInt("ID");
			String name = rset.getString("NAME");
			int price = rset.getInt("PRICE");
			String regdate = rset.getString("REGDATE");
			System.out.println(iid+" "+name+" "+price+" "+regdate);
		}
	}
}

```



