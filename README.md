## 소유권과 관련 명령어

```bash
명령어 ls -l : 파일 속성을 표시

- rw-rw-r-- 1 wlstmd wlstmd 53 2024-05-19 18:40 TEST

속성 값 의미

- rw-rw-r-- : 파일 허가권 및 파일 유형과 접근 권한으로 구성
1 : 물리적 파일의 개수
wlstmd : 파일 소유자명
wlstmd : 파일 소유 그룹명
53 : 파일 크기 (바이트 단위)
2024-05-19 : 파일이 마지막으로 변경된 시간
TEST : 파일명
```

- 파일의 허가권이나 소유권을 설정하는 명령어는 chmod chown chgrp umask

```bash
명령어 chown : 파일과 디렉터리의 사용자 소유권과 그룹 소유권을 변경

# chown [option] 소유자[:그룹명] 파일명
-R 하위 디렉터리를 포함하여 디렉터리 내부의 모든 파일의 소유권을 변경


디렉터리 wlstmd의 모든 파일과 하위 디렉터리의 소유권을 root로 변경
ex) chown -R root wlstmd

파일 TEST의 그룹 소유권을 root로 변경
ex) chown :root TEST
```

```bash
명령어 chgrp : change group을 줄인 명령어로 파일이나 디렉터리의 그룹 소유권을 변경

# chgrp [option] 그룹명 파일명

파일 TEST의 그룹 소유권을 wlstmd로 변경
ex) chgrp wlstmd TEST
```

## 허가권과 관련 명령어

```bash
명령어 ls -l로 파일 유형과 허가권을 알 수 있다.

ls -l
- rw-rw-r-- 1 wlstmd wlstmd 53 2024-05-19 18:40 TEST

파일은 일반 파일, 디렉터리 파일, 특수 파일로 나뉜다.

명령어 ls -l /dev 를 통해 파일 유형이 'b', 'c'인 파일들을 확인 할 수 있다.

기호와 파일 유형
- : 일반 파일
d : 디렉터리 파일
<특수 파일>
b : 블록 단위로 읽고 쓰는 블록 장치 특수 파일 (블록 장치 : 하드 디스크, 플로피디스크, CD/DVD 등)
c :  문자 단위로 읽고 쓰는 문자 장치 특수 파일 (문자 장치 : 마우스, 키보드, 프린터 등)
l : 기호적 링크로 바로가기 아이콘 역할 수행, 연결되어 있는 파일과 실제 파일은 다른 곳에 존재
p : 파이프
s : 소켓

파일 사용자는 파일 소유자, 그룹 소유자, 기타 사용자로 나뉨
파일 권한은 읽기, 쓰기, 실행이 있다.

기호 모드 : r w x
8진수 숫자 모드 : 4 2 1

TEST.txt파일의 권한이 rw-rw-r-- 이라면 사용자는 읽고 쓰기, 그룹 사용자들도 읽고 쓰기 이며 기타 사용자들은 읽기만 가능하다.
```

```bash
명령어 chmod : 파일이나 디렉터리의 접근 허가권을 변경하는 명령어

권한 변경은 '숫자 모드' 또는 '기호 모드'가 있다.

<숫자 모드>
rwx : 읽기, 쓰기, 실행 권한 모두 가짐
rw- : 읽기, 쓰기 권한을 가짐
r-x : 읽기, 실행 권한을 가짐
r-- : 읽기 권한만을 가짐
-wx : 쓰기와 실행 권한을 가짐
-w- : 쓰기 권한만을 가짐
--x : 실행 권한만을 가짐
--- : 아무런 권한이 없음

<기호 모드>
ex) chmod -R o+w wlstmd
<사용자>
u : 사용자
g : 그룹
o : 기타 사용자
a : 모든 사용자
<연산자>
+ : 허가권 부여
- : 허가권 제거
= : 특정 사용자에게 허가권 지정
<적용 예제>
# chmod u+w : 소유자에게 쓰기 권한 부여
# chmod u-x : 소유자에게 실행 권한 제거
# chmod o=rx : 기타 사용자에게 읽기와 실행 권한 부여
```

```bash
명령어 umask : 새로 생성되는 파일이나 디렉터리의 "기본 허가권 값"을 지정

파일의 기본 권한은 666, 디렉터리의 기본 권한은 777이다. umask는 이 기본 권한값을 변경한다.

# umask [option][value]
-S umask 값을 문자로 표기

파일이나 디렉터리 생성 시 디폴트 권한 값에서 설정한 umask 값을 뺀 값을 기본 허가권으로 지정한다.
ex) umask가 022인 경우, 파일 권한은 666-022 = 644권한이 되고, 디렉터리 권한은 777-022 = 755권한이 된다.

umask 적용 방법
umask -> 보수로 취한 -> AND 연산으로 권한이 설정

- umask가 754인 경우 -> 보수로 취환 (111 101 100 -> 000 010 011)
<파일 권한>
  000 010 011
& 110 110 110
--------------
  000 010 010
권한 표시 : (--- -w- -w-)
```

```bash
특수 권한
SetUID와 SetGID
프로세스가 실행되는 동안 해당 프로세스의 root권한을 임시로 가져오는 기능이다.
SetUID의 경우 사용자가 사용할때만 소유자 권한으로 파일을 실행시키고, SetGID의 경우 사용자가 사용할때만 그룹 권한으로 파일을 실행한다.

SetGID 권한이 명시된 디렉터리에 생성되는 모든 하위 디렉터리나 파일도 SetGID 권한을 가진다.

특수 권한의 절대적인 표기 방법은 4자리로 나타낸다.
☐           코드 절대값         설명
SetUID       s  4000 프로세스 실행 당시 UID로 설정
SetGID       s  2000 프로세스 실행 당시 GID로 설정
Sticky bit   t  1000 실행 후에도 메모리를 점유 하도록 설정
```

```
Sticky bit

- 일반적으로 공용 디렉터리를 사용할때 sticky bit를 설정하여 사용한다.
- sticky biy가 설정되어 있는 디렉터리 안의 내용은 해당 파일의 소유자나 root만이 변경이 가능하게 하여 공용디렉터리라도 권한에 제약을 두어 다른 사용자들의 파일을 보호 하기 위한 목적으로 만든 것 이다.

- 사용자 권한을 지정하기 어려운 프로그램들이 일시적으로 특정 디렉터리에 파일을 생성하고 삭제하도록 이용한다.

- 특정 응용 프로그램이 다른 응용프로그램에서 생성한 파일을 삭제하지 못하는 권한 설정을 한다.
- 설정된 디렉터리에는 누구든 접근 가능하고 파일을 생성할 수 있다.
- 생성된 sticky bit 파일을 삭제 시에는 소유자(파일 생성자)와 관리자만 지울 수 있다. 다른 사용자는 자신의 소유가 아닌 파일을 삭제할 수 없다.
- sticky bit가 적용된 디렉터리 내에 파일을 생성하는 것은 누구나 가능하지만, 삭제는 생성자 본인과 관리자만 가능하다.
- 일반적으로 sticky bit가 설정되는 디렉터리는 /tmp 안에 생성한다.

ex) # chmod 1777 ClassA
    # ls -l ClassA
    drwxrwxrwt. root root 6 5월 13:41 ClassA
```

```bash
★디스크 쿼터

- 파일 시스템마다 사용자나 그룹이 생성할 수 있는 파일의 용량 및 개수를 제한 하는 것 이다.
- 보통 블록단위의 용량제한과 inode(파일과 디렉터리)의 개수를 제한한다.
- 쿼터는 사용자별, 파일 시스템별로 동작된다.
- 그룹단위로도 용량을 제한 할 수 있으며, 웹호스팅 서비스를 하는 경우에 유용하다.

쿼터 진행 순서

/etc/fstab 수정 -> 재마운팅 또는 리마운팅 -> 쿼터 DB 생성 -> 개인별 쿼터 설정

<디스크 쿼터 관련 명령어>
명령어 quataoff : 쿼터 서비스를 비활성화한다.
옵션      설명
-a : 파티션 정보 출력
-u : 사용자 쿼터 비활성화
-g : 그룹 쿼터 비활성화
-v : 메세지 출력

명령어 quatacheck : 파일 시스템의 디스크 사용 상태를 검색한다.
옵션        설명
-a : 모든 파일 시스템을 체크
-u : 사용자 쿼터 관련 체크
-g : 그룹 쿼터 관련 체크
-m : 재마운트를 생략
-n : 첫번째 검색된 것을 사용
-p : 처리 결과를 출력
-v : 파일 시스템의 상태를 보여줌

명령어 edquota : 편집기를 이용하여 사용자나 그룹에 디스크 사용량을 할당하는 명령어이다.
옵션        설명
-u : 사용자 디스크 할당량 설정
-g : 그룹 디스크 할당량 설정
-t : 디스크 할당량 유예기간 설정
-p : 디스크 할당량 설정을 다른 사용자와 동일하게 설정

명령어 setquota : 편집기가 아닌 명령행에서 직접 사용자나 그룹에 디스크 사용량을 할당하는 명렁어이다.
옵션
-u : 사용자 디스크 할당량 설정
-g : 그룹 디스크 할당량 설정
-a : 해당 시스템의 모든 설정
-t : 유예기간을 초 단위로 설정
```

```
LVM

LV (Logical Volume): 논리 볼륨은 사용자가 실제로 데이터를 저장하는 공간입니다. LVM을 사용하면 LV의 크기를 필요에 따라 쉽게 조정할 수 있으며, 여러 개의 물리적 디스크에 걸쳐 있을 수도 있습니다.

PV (Physical Volume): 물리 볼륨은 LVM에서 사용되는 가장 낮은 수준의 저장공간 단위입니다. 하나 이상의 하드 드라이브 파티션 또는 전체 디스크를 PV로 초기화하여 LVM에서 사용할 수 있게 합니다.

PE (Physical Extent): 물리적 확장은 PV 내에서 LV에 할당될 수 있는 고정된 크기의 데이터 블록입니다. PE는 LVM의 할당 단위로 작동합니다.

VG (Volume Group): 볼륨 그룹은 하나 이상의 PV를 묶어서 관리하는 논리적인 컨테이너입니다. VG 내에서 LV들을 생성하고 관리합니다.
```

```
tar 관련
J=xz
j=bz
z=gz

-c : 파일이나 디렉터리를 묶는것
-C : 디렉터리 변경
```

```
kill 시그널

1 SIGHUP 재시작
2 SIGINT [Ctrl+C]의 시그널 / 프로세스 종료
3 SIGQUIT [Ctrl+\]의 시그널 / 종료
9 SIGKILL 강제 종료
15 SIGTERM 시스템 호출시, 가능하면 정상 종료 시키는 시그널 kill 명령 (기본 시그널)
20 SIGTSTP [Ctrl+Z]의 시그널 / 프로세스를 대기로 전환. 프로세스 중단
```

```

PV : Physical Volume, 디스크를 LVM에서 사용할 수 있게 변환하는 작업
VG : Volume Group, PV가 모여 만들어진 그룹
     VG는 다시 LV로 할당할 수 있는 공간이기도 함.
PE : Physical Extent, PV에서 나누어 사용하는 블록. 4MB 단위
LV : Logical Volume, VG에서 사용자가 필요한 만큼 할당돼서 만들어지는 공간
LE : Logical Extent, LV가 나누어진 일정한 크기의 블록. PE와 서로 1:1 대응
```

```
1) CUPS(CommonUnix Printing System)
- 애플이 개발한 오픈 프린팅 소스 시스템
- 설정 디렉터리: /etc/cups
- 웹기반으로 IPP(Internet Printing Protocol) 사용하여 제어 http/631 port
2) LPRng :
- 버클리 프린팅 시스템.
- BSD계열.
- 프린터 스풀링과 네트워크 프린터 서버를 지원
- 리눅스 초기에 기본으로 사용됐으나 최근에는 CUPS 시스템을 추가로 사용
3)SANE(Scanner Access Now Easy): 스케너등 이미지 관련 하드웨어를 제어하는 API
4)ALSA(Advanced Linux Sound Architechture): 사운드 카드용 장비 드라이버를 제공하기 위한 리눅스 커널요소
```

```
vi 옵션

set ai : 자동 들여쓰기 옵션 윗 라인에 맞춰 같이 자동으로 들여쓰기
set ic : 검색 패턴 사용 시 대소문자 구별 X
set sm : 소스 코딩 작성 중 괄호를 닫을 때 어디에 있는 열기 괄호와 연관 되어 있는지 표시
set list : 눈에 보이지 않는 특수문자를 표시합니다
```

```
1) make: 소프트웨어를 컴파일하는 유틸리티. configure에 의해 변경된 내용을 반영함.
2) cmake: make의 대체 프로그램. make 과정을 수행하지 않고, 지정한 운영체제에 맞는 make 파일 생성이 목적.
3) configure: 소스 프로그램의 환경 설정을 하는 스크립트
4) dnf: rpm의 의존성 문제를 해결해 주는 패키지 설치 명령어
```

```
2) edquota [option] [사용자명 또는 그룹명]
- 편집기를 이용하여 사용자나 그룹에 쿼터를 설정할 때 사용함

3) repquota [마운트경로]
- 파일 시스템에 설정된 쿼터 정보를 출력하는 명령어

4) setquota [옵션][사용자명 또는 그룹명]
- 명령행에서 직접 사용자나 그룹에 디스크 사용량을 할당하는 명령어
```

```
1) ksh: sBourne Shell 가 호환되며 C Shell 의 많은 기능을 포함,  Unix 계열에서 많이 사용됨
2) bash: 리눅스에서 가장많이 사용되는 셸로 Bourne 셀을 토대로 C셸과 Korn Shell 의 기능들을 통합시켜 개발됨
3) dash: 본쉘을 기반으로 개발됨. POSIX 표준을 준수하여 작은 크기로 만들어지게 됨
4) tcsh: C Shell 에 명령 행 완성 과 명령 행 편집 기능을 추가
```

```
/etc/fstab 파일에 설정하는 각 필드 값
[ 장치명 ] [ 마운트 포인트 ] [ 파일 시스템 종류 ] [ 마운트 옵션 ] [ Dump 값 ] [ 무결성 검사 우선순위 값 ]

마운트 옵션 종류

default - rw, nouser, auto, exec, suid 옵션이 설정된 기본 옵션
auto - 부팅 시 자동으로 mount한다
noauto - auto의 반대, 부팅 시 자동으로 mount하지 않는다
exec - 실행 파일이 실행되는 것을 허용함
noexec - exec의 반대, 실행 파일이 실행되는 것을 허용하지 않음
suid - Set-UID, Set-GID를 설정할 수 있음
nosuid - suid의 반대, Set-UID, Set-GID를 설정할 수 없음
ro - Read-Only, 읽기 전용 파일 시스템
rw - Read and Write, 읽고 쓰기가 가능한 파일 시스템으로 설정
user - 일반 사용자가 mount 할 수 있음
nouser - user의 반대, 일반 사용자가 mount 할 수 없으며 root 유저만 mount 할 수 있다.
quota - quota 설정 가능
noquota - quota의 반대, quota 설정 불가능
```

```
ftp-data 20
ftp 21
ssh 22
telnet 23
smtp 25
dns 53
http 80
POP3 110
IMAP 143
HTTPS 443
POP 110
```

```
프로토콜 기본 요소

구문 : 데이터의 구조나 형식을 하는 것으로 부호화 신호레벨 등을 규정하며
의미 : 전송의 조작이나 오류 제어를 위한 제어 정보에 대한 규정
순서 : 속도 일치 및 순서 제어이다.
```

```
1) /etc/resolv.conf: 사용하고자 하는 네임서버를 지정하는 파일
2) /etc/services: 리눅스 서버에서 사용하는 모든 포트들에 대한 정의를 설정하는 파일
3) /etc/sysconfig/network-scripts: 리눅스 ip 주소를 설정하는 파일
4) /etc/hosts: 특정 URL 주소에 접속할 때, DNS 서버에 질의하지 않고 지정된 IP 주소로 연결해 주는 기능을 하는 파일
```

```
LAN 케이블 별 최대 전송 속도
CAT.5: 100Mbps
CAT.5e: 1Gbps
CAT.6: 1Gbps
CAT.6A: 10Gbps
CAT.7: 10Gbps
```

```
r 옵션 : (append) 파일 내의 기록에 다른 파일 내용을 추가로 묶음
c 옵션 : (create) 새 파일을 만듦
x 옵션 : (extract) 기록에서 파일을 압축 해제
t 옵션 : (list) 압축된 파일 안에 있는 구성 파일 출력

c : tar 아카이브 생성. 기존 아카이브 덮어쓰기
r : tar 아카이브의 마지막에 파일들 추가.
x : tar 아카이브에서 파일 추출(파일풀때)
t : tar 아카이브에 포함된 내용 확인
v : 처리되는 과정을 자세히 나열
f : 대상 tar 아카이브 지정(기본 옵션)
```

```
x 프로토콜
[ xauth ] : X서버에서 X클라이언트 허가를 위해 생성한 키 값을 확인할 때 사용
[ xhost ] : X서버에서 접근 가능한 IP주소 및 호스트명을 확인할 때 사용
```

```
/etc/hosts 는 단순 ip 도메인 매칭이고
/etc/resolve.conf 가 DNS 입니다.

1. /etc/hosts: IP주소와 도메인 주소를 1:1로 등록하여 도메인에 대한 IP주소를 조회하도록 한다.
2. /etc/resolv.conf: 기본적으로 사용할 도메인명과 네임서버를 설정한다.
3. /etc/sysconfig/network: 네트워크 기본정보가 설정되어 있는 파일이다.
4. /etc/sysconfig/network-scripts/ifcfg-ethX: 지정된 네트워크 인터페이스의 네트워크 환경 설정 정보가 저장된다.
```

```
df - 리눅스 시스템 전체 디스크 사용량 확인
du - 특정 디렉토리 또는 파일을 단위로 하여 용량을 확인
```

```
/etc/fstab 개념

첫 번째 필드는 장치명, 볼륨 라벨, UUID 모두 사용이 가능하다.
특정 파티션을 부팅 시에 자동으로 마운트되지 않도록 설정 할 수 있다.
파일 시스템 관련 정보 파일로 mount, umount, fsck 등의 명령어가 수행될 때 이 파일의 정보를 참조한다.
dump 명령을 통한 백업 시 사용주기를 사용안함, 매일 수행, 2일에 한번 수행으로 설정이 가능하다.
- 0 : dump 사용 안함 / 1 : 매일 수행 / 2 : 2일에 한번 수행
```

```
fdisk
기능: 디스크의 파티션 생성, 삭제, 보기 등 파티션을 관리한다.
형식: -b 크기 → 섹터 크기를 지정한다.
      -ㅣ → 파티션 테이블을 출력한다.

내부 명령
a -파티션의 부트 가능 플래그를 전환합니다.
c -파티션의 DOS 호환성 플래그를 전환합니다.
d -파티션을 삭제합니다.
l -fdisk에 알려진 파티션 유형을 나열합니다.
m -모든 사용 가능한 메뉴 목록을 표시합니다.
n -새 파티션을 추가합니다.
p -현재 디스크에 대한 파티션 테이블을 프린트합니다.
q -변경사항을 저장하지 않고 종료합니다.
t -파티션에 대한 파일 시스템 유형을 변경합니다.
u -표시/항목 단위를 변경합니다.
v -파티션 테이블을 검증합니다.
w -디스크에 테이블을 쓰고 종료합니다.
x -다음의 전문가용 추가 기능을 나열합니다.
```

```
histroy 히스토리 보기
histsize 저장 가능한 최대 명령어 수
histfilesize 히스토리 저장 파일 최대 크기
```

```
/etc/passwd

필드 1 : 사용자명
필드 2 : 패스워드
필드 3 : 사용자 uid
필드 4 : 사용자 gid
필드 5 : 사용자 이름
필드 6 : 사용자 홈 디렉토리
필드 7 : 사용자 로그인 쉘
```

```
~/.bashrc: 개인 사용자가 정의하는 alias, function이 있는 파일
~/.bash_profile: 개인 사용자의 시작 프로그램 설정, 환경설정과 관련있는 파일
사용자가 로그인 함 -> ~/.bash_profile 파일을 불러들임

~/.bash_logout: 개인 사용자가 로그아웃을 수행하는 설정을 지정하는 파일
```

```
nice는 프로세스 명을 입력해야되고 renice는 PID값을 입력해야된다.
```

```
vi 편집기는 명령모드, 입력모드, 마지막 행 모드(ex 명령모드)로 총 3가지 모드로 구성되어있습니다.
```

```
rpm 옵션

-qi : 패키지의 정보를 출력
-ql : 파일을 설치한 패키지의 위치 출력
-qa : 전체 패키지 목록 출력
-qf : 파일이 속한 패키지 출력
```

```
패키지 : 패키지이름-버전-릴리스번호-시스템,아키텍처.압축확장자
```

```
- X윈도우 데스크탑 환경 -
1. KDE
2. Xfce
3. LXDE
4. GNOME
```

```
압축률 순위: gzip < bz2 < xz
```

```
ICANN(아이칸) : the Internet Corporation for Assigned Names and Numbers
인터넷 도메인 관리와 정책을 결정하는 비영리 국제관리 기구이다.

EIA : Electronic Industries Association
    : 전자 공업 연맹

ITU : International Telecommunication Union
    : 국제 전기 통신 연합

IEEE : Institute of Electrical and Electronics Engineers
     : 전기 전자 학회
```

```
fdisk 속성 변경 값 조합

Linux 83

Swap 82

LVM 8e

Raid fd
```

```
edquota = 쿼터를 설정한다
quota : 사용자 할당량 지정
quotaon : quota 실행
setquota : vi편집기가 아닌 명령어 커미널에서 쿼터 설정 가능
```

```
mkfs = 파티션한 하드디스크 포맷
fsck = 리눅스 파일 시스템을 검사하고 수리하는 명령
free = 메모리 확인
fdisk = 파티션테이블 관리
```

```
$ : 줄의 끝을 의미
^ : 줄의 시작을 의미
< : 단어의 시작을 의미
> : 단어의 끝을 의미
```

```
크롬 - "구글"
사파리 - "애플"
오페라 - "노르웨이"
파이어폭스 - "모질라재단"
```

```
Arduino : 이탈리아, 싱글 보드
Raspberry Pi : 영국, 교육용, 싱글 보드
Micro Bit: 영국, BBC, ARM 기반
Cubie Board: 중국
```

```
터미널 개발 순서
sh -> csh -> tcsh -> ksh -> bash -> zsh
```

```
RAID0 - 스트라이핑 방식. 디스크를 그대로 이어 붙여 큰 용량 하나로 씀. 백업용이 없으니 고장나면 그대로 날아감.
RAID1 - 미러링 방식. 반반 나눠서 절반은 완전히 복제로 사용. 말 그대로 반은 백업
RAID5 - 패리티 1개.
RAID6 - 패리티 2개.
패리티: 장애가 발생한 뒤에 데이터를 복원하기 위해서 사용되는 부호
```

```
quotacheck → edquota → quotaon → repquota
```

```
1. ICANN : IP 주소 할당 및 도메인을 관리하는 국제기구
2. IEEE : LAN 및 MAN 관련 표준을 제정한 기관
3. ISO : OSI 참조모델(OSI 7계층) 프로토콜을 제정한 국제기구
4. EIA : T568B케이블 배열 표준화기구
```

```
TCP 프로토콜 패킷

1. Client > Server : TCP SYN
2. Server > Client : TCP SYN / ACK
3. Client > Server : TCP ACK
```

```
image

1. GIMP : 이미지 편집기
2. eog : GNOME의 이미지 뷰어
3. evince : PDF 문서를 확인할 때 사용하는 프로그램
4. Gwenview : KDE용 이미지 뷰어
```

```
vi

a = 한 칸 뒤로 넘어가고 편집모드 전환
i = 커서의 해당 위치에서 편집모드 전환
o = 다음 줄로 넘어가고(Enter 키와 역할 같음) 편집모드로 전환
```

```
물리 계층 : 비트(bit)
데이터링크 계층 : 프레임(frame)
네트워크 계층 : 패킷(packet)
전송 계층 : TCP - Segment / UDP - Datagram
```

```
/etc/sysconfig/network-scripts : 네트워크 인터페이스 설정 파일들이 위치하는 디렉토리
/etc/sysconfig/network : 시스템의 전반적인 설정 포함
```

```
1.윈도 매니저 : X-윈도우상에서 윈도우의 배치와 표현을 담당
2.데스크톱 환경 : GUI 환경을 이용하기 위해 사용자에게 제공되는 인터페이스 스타
3.디스플레이 매니저 : : X-윈도우 구성요소 중 사용자 로그인 및 세션 관리 역할 수행 프로그램
4.데스크톱 매니저 :  XDM, GDM, KDM 등이 존재
```

```
최소3개의 저장 장치를 필요로 하고 1개의 패리티를 사용하는건 RAID-5
최소4개의 저장 장치를 필요로 하고 2개의 패리티를 사용하는건 RAID-6
```

```
renice 양수일 경우 n, 음수일 경우 -n을 사용합니다.
nice 양수일 경우 -n, 음수일 경우 --n을 사용합니다.
```

```
A클래스: 1.0.0.0 ~ 126.255.255.255

최초 1바이트가 0으로 시작하지 않음
10진수 예: 10.0.0.0

B클래스: 128.0.0.0 ~ 191.255.255.255

최초 1바이트가 10으로 시작
10진수 예: 128.0.0.0

C클래스: 192.0.0.0 ~ 223.255.255.255

최초 1바이트가 110으로 시작
10진수 예: 192.0.0.0 (11000000.00000000.00000000.00000000)

D클래스: 224.0.0.0 ~ 239.255.255.255

최초 1바이트가 1110으로 시작
멀티캐스트 주소에 사용

E클래스: 240.0.0.0 ~ 255.255.255.255

최초 1바이트가 1111로 시작
향후 사용을 위해 예약된 범위
```

```
192.168.1.0/24 네트워크에 대해 서브넷 갯수와 사용 가능한 IP 갯수

네트워크 주소: 192.168.1.0
서브넷 마스크: /24 or 255.255.255.0


서브넷 갯수 구하기


서브넷 비트 수 = (Ipv4)32 - 24 = 8 비트
서브넷 갯수 = 2^서브넷 비트 수 = 2^8 = 256개


사용 가능한 IP 갯수 구하기


호스트 비트 수 = 32 - 24 = 8비트
사용 가능한 IP 갯수 = 2^호스트 비트 수 - 2
= 2^8 - 2
= 256 - 2
= 254개

따라서 192.168.1.0/24 네트워크에서는

256개의 서브넷을 만들 수 있다.
254개의 IP 주소를 호스트에 할당할 수 있다.

x.x.x.0 : 0~255
x.x.x.128 : 0 ~ 127 / 127 ~ 255
x.x.x.192 : 0 ~ 63 / 64 ~ 127 / 128 ~ 191 / 191 ~ 255
x.x.x.224 : 0 ~ 31 / 32 ~ 63 / 64 ~ 95 / 96 ~ 127 / 128 ~ 159 / 160 ~ 191 / 192 ~ 223 / 224 ~ 255
```

```
IPv4 사설 IP주소 범위(클래스)

IPv4 : 32bit & IPv6 : 128bit
A class : 10.0.0.0 ~ 10.255.255.255 ⇒ 사설 네트워크 : 1개
B class : 172.16.0.0 ~ 172.31.255.255 ⇒ 사설 네트워크 : 16개
C class : 192.168.0.0 ~ 192.168.255.255 ⇒ 사설 네트워크 : 256개
```