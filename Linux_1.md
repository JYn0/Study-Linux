# Linux

#### 설치

Create new virtual

I will ~ check

Linux - Version:CentOS 64-bit

CentOS 64-bit 20190718 / c:\CENTOS

edit - 2GB



ctrl+alt

한국어 - 키보드(한국어+영어(미국))

소프트웨어센서 - 개발및창조를위한~체크

네트워크 켜기

설치대상 - 파티션을 설정합니다 - 표준 파티션 + swap 2GB + /

swap메모리 : 메모리 부족할때

ROOT암호 : 111111 / 성명:centos



HWADDR : HWADDR="00:0C:29:C9:AF:E6"

2 - HWADDR="00:50:56:32:D5:54"

3 - 00:50:56:26:AB:F0





### 가상머신 파일을 복사해서 Clone 만들기

1. C드라이브에 있는 CENTOS 를 복사하여 CENTOS2를 만들고
2. Configuration file의 displayname을 바꾸고
3. VMware에서 CENTOS2 로 들어가서 추가하기.
4. 반드시 moved it 을 선택하여 실행.
5. edit -network Adapter -advanced- Mac addr generate
6. terminal 에서 gedit /etc/sysconfig/network-scripts/ifcfg-ens33 ex)ens33 달라질 수 있음.
7. Mac addr 복사한거 HWADDR 에 붙여넣기 ex)00:50:56:35:05:0C

### 3. 가상 컴퓨터 설정 (매우 중요)

1. YUM 명령어를 이용하여 소프트웨어 업데이트를 할때도 버전에 맞는 소프트웨어만 다운로드하도록 설정
   - cd /etc/yum.repos.d/
   - ls
   - gedit CentOS-Base.repo
   - released updates 부분 삭제
   - gedit CentOS-Sources.repo
   - released updates 삭제
2. CentOS 7.0 (1406) 버전이 계속 설치되도록 터미널에서 설정 변경
   - cd /etc/yum.repos.d/
   - mv CentOS-Base.repo CentOs-Base.repo.bak
   - wget <http://download.hanbit.co.kr/centos/7/CentOS-Base.repo>
   - rm *.repo~
3. IP 주소 고정할당
   - cd /etc/sysconfig/network-scripts/
   - gedit ifcfg-ens33
     - 수정 : BOOTPROTO = "dhcp " --> none
     - 추가 :
       - IPADDR =192.168.111.100
       - NETMASK=255.255.255.0
       - GATEWAY=192.168.111.2
       - DNS1 = 192.168.111.2
   - **systemctl restart network** 이 명령어를 사용하여 항상 바꾼뒤에 변경내용을 Apply 해줘야 함
4. HOSTNAME 변경하기
   - hostnamectl set-hostname server1
   - gedit /etc/hosts
     - 192.168.111.100 server1 저장하고 나오기
   - reboot 한번 하기
5. SELinux 보안 해제

- gedit /etc/sysconfig/selinux
  - SELINUX = disabled 로 바꾸고 저장.

gedit /etc/sysconfig/network-scripts/ifcfg-ens33

00:50:56:35:05:0C

### 4.WinClient 설치해보기

display name: WIN8

HDD: 30G

MEMORY :4G

IP: 192.168.111.200

- N6KXJ - P6YWY - 4C92Q - J7BVB - R6XGM
- Window만 설치 하기로 INSTALL하기.
- 계정 없이 로그인으로 설정
- HOTS file에 IP ADDRESS랑 이름 등록해두기

윈도우에서 다른 컴퓨터 가상머신으로 접근 할 노ㅕㅅ수 있도록 만들어 보기.