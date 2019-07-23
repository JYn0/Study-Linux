# Linux

## 명령어

id@server

```
종료
poweroff
shutdown -P +3

리부팅
shutdown -r now
reboot
init 6
```

```
네트워크 설정
cd /etc/sysconfig/network-scripts/
gedit ifcfg-ens33
```

```
스위치 유저
su root
로그아웃
exit
```

```
history
history -c
touch 가상의파일
man ls
cd - 이전에 작업했던 곳으로
-r 폴더 옵션
-rf 물어보지않고 바로
cat, more
Ctrl+l -- home
```

```
vi편집기
i Insert 왼쪽
a Insert 오른쪽
A($) Insert End
I(^) Insert Home
o Insert 다음줄
O Insert 윗줄
s replace 특정단어 교체
cw 단어 교체
esc 명령모드
: 메뉴로
w 저장
q 나가기
! 강제
:set nu 행 번호 나옴
(숫자)yy (숫자)줄 복사
p 밑줄에 붙여넣기
P 윗줄에 붙여넣기
hL j아래 k위 lR
gg 맨 위로
G 맨 뒤로
숫자G,:숫자 -> 숫자 행으로
x 단어 삭제
(숫자)dd (숫자)줄 삭제
/단어 단어찾기 n next
:%s/이단어를/이걸로 바꿔라
```

```
마운트
/dev/sr0 on -> 물리적인 CD롬
/run/media/root/CentOS 여기에 존재
/dev/sda2 -> 하드디스크
umount /dev/cdrom -> cdrom을 떼어냄

cd /
mkdir mycdrom
mount /dev/cdrom /mycdrom
cd /
umount /mycdrom

```

사용자 관리와 파일 속성

```
cd /etc/
more passwd
사용자 이름:암호:사용자 ID:사용자가 소속된 그룹ID:전체 이름:홈 디렉터리:기본 셸
more shadow

useradd user1
passwd user1
userdel -r user2

groupadd -g 2222 multi
useradd -g multi muser1
passwd muser1

```

