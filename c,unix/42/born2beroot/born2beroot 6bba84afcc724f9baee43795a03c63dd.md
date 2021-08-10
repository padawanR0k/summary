
# born2beroot

## 요약

- VM기술을 활용해서 여러 요구사항들을 만족시켜라
- [평가지](https://github.com/wshloic/born2beroot_correction/blob/master/correction_born2beroot.pdf)


## 정리

### Virtual Machine이란?

- 하나의 물리 서버를 보다 효율적으로 사용하기 위해 탄생함
- virtual machine으로 할 수 있는것
    - 맥북에 리눅스 배포판을 깐 후, 사용할 수 있음
    - 맥북에 윈도우를 깔아서 공공기관 사이트들을 사용할 수 있음
- 물리적 하드웨어 시스템에 CPU, 메모리, 네트워크 인터페이스 및 스토리지를 갖추고 가상으로 컴퓨터 시스템을 동하는 가상환경 리소스 가상화 프로그램
- 호스트-게스트로 구성되어있다
    - 호스트 (호스트 머신, 호스트 컴퓨터, 호스트 운영체제)
        - 하이퍼바이저가 탑재된 물리적 머신임
    - 게스트 (게스트 머신, 게스트 컴퓨터, 게스트 운영체제)
        - 하이퍼바이저로 인해 리소스를 사용하는 머신임
- 하이퍼바이저?
    - 호스트 컴퓨터에서 다수의 운영체제를 동시에 운영하기 위한 논리적 플랫폼
        - ex) vmware, virtual box
    - 호스트 시스템에서 다수의 게스트를 구동할 수 있게 해주는 소프트웨어 (호스트와 게스트를 이어준다)
    - 유형
        1. 네이티브 or 하이퍼바이저형

            `Xen, 마이크로소프트 Hyper-V, KVM`

            ![born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled.png](born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled.png)

            - 하이퍼바이저가 하드웨어를 직접 제어해서 효율적임
            - 여러 하드웨어 드라이버를 세팅해야해서 설치가 어려움
        2. 호스트형

            `VMware server, VMware Workstation, Virtual box`

            ![born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%201.png](born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%201.png)

            - 일반적인 소프트웨어 처럼 호스트OS위에서 실행됨
            - VM내부의 게스트 OS에 하드웨어자원을 에뮬레이트하는 방식
            - 게스트OS에 대한 제약이 없다
- 부가적인 지식
    - 컨테이너 방식의 가상화 Docker

        ![born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%202.png](born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%202.png)

        - 기존의 하이퍼바이저는 OS를 포함하고 있어 기능의 중복이 있고 상대적으로 무거웠음
        - 컨테이너 기반 가상화는 기능의 중복을 줄이고 가벼운 가상화를 위해 탄생함. 도커는 그중에 하나
    - 컨테이너 개념; 실행에 필요한 라이브러리와 바이너리, 기타 구성파일을 `이미지`단위로 빌드하고 배포함
        - 이런 이미지들을 docker hub에서 공유하기도 함. git clone 하듯이 이미지를 가져와 바로 사용할 수 있다.
        - mysql db서버를 구동시키기 위한 도커이미지도 있고, 마인크래프트의 멀티를 위한 도커이미지도 있다
    - 하이퍼바이저보다 훨씬 가벼움(MB단위)

### CentOs 와 Debian의 차이?

- centos
    - 레드햇이라는 회사에서 상용으로 배포한 리눅스를 가져와 RHEL을 완벽에 가깝게 반영하는 것으로 만들어짐 (유료화에 대한 반발)
    - 오픈소스이다보니 업데이트가 느림. 기술지원도..
    - 그래서 인력이 충분한곳은 CentOS를 선호, 안정성이 중요한 금융권은 RHEL 선호
    - 주로 Yum을 통해 소프트웨어를 업데이트할 수 있으며 up2date도 지원한다.

- debian
    - 온라인 커뮤니티에서 제작하여 배포됨.
    - 초반에는 레드햇계열에 비해 사후지원과 유틸성능이 뒤쳐졌으나 현재는 뒤쳐지지않으며 넓은 유저층을 가짐
    - 데비안의 특징은 패키지 설치 및 업그레이드의 단순함에 있다. 일단 인스톨을 한 후 패키지 매니저인 **APT 업데이트 방식**을 이용하면 소프트웨어의 설치나 업데이트에서 다른 패키지와의 의존성 확인, 보안관련 업데이트 등을 **자동으로 설정 및 설치**해준다.

### LVM (Logical Volume Manage)

![born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%203.png](born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%203.png)

- 논리 볼륨 관리자
- 물리적인 디스크를 논리적인 디스크로 할당하여 유연하게 관리할 수 있게해줌
    - 운영체제 설치시 파티션을 지정해줘야하는데, 기존에 이 파티션의 크기를 바꾸는 방법은 재설치하여 해결함
    - LVM은 재설치없이 크기를 조정
- 비슷한 것으로는 (RAID)가 있음
- 용어 설명
    - 파티션(Partition)
        - 하나의 하드디스크에 대해 영역(구역)을 나누는 것을 말한다. fdisk로 파티션 설정 가능.
    - 물리볼륨(PV, Physical Volume)
        - 물리볼륨은 각각의 파티션을 LVM으로 사용하기 위해 형식을 변환시킨 것이다.(/dev/ hda1, /dev/hda2 등)
    - 논리볼륨(LV, Logical Volume)
        - 사용자가 다루게 되는 부분이며 마운터 포인터로 사용할 실질적인 파티션이다. 크기를 확장 및 축소 시킬 수 있다.
    - 볼륨그룹(VG, Volume Group)
        - PV로 되어 있는 파티션을 그룹으로 설정한다. /dev/sda1 을 하나의 그룹으로 만들 수도 있고, /dev/sda1 + /dev/sda2처럼 파티션 두 개를 하나의 그룹으로 만들 수 있다.
    - 물리적 범위(PE, Physical Extent)
        - PE는 LVM이 물리적 저장공간(PV)을 가리키는 단위이다. 기본 단위는 4MB이다.
    - 논리적 범위(LE, Logical Extent)
        - LE는 LVM이 논리적 저장공간(LV)을 가리키는 단위이다. 기본 단위는 물리적 범위와 동일합니다.
    - VGDA(Volume Group Descriptor Area)
        - 볼륨그룹의 모든 정보가 기록되는 부분. VG의 이름, 상태, 속해있는 PV, LV, PE, LE들의 할당 상태 등 을 저장한다. VGDA는 각 물리볼륨의 처음부분에 저장된다.

### SSH (Secure Shell)

- 원격 호스트에 접속하기 위해 사용되는 보안 프로토콜
- 기존에 사용하던 텔넷이 암호화를 제공하지 않아서 보안상에 취약했어서 나오게됨
- 22번 포트 사용
- 작동원리
    - 클라이언트와 호스트가 각각 키를 보유하고 이를 활용함
- 방식
    - 비대칭키 방식

        ![born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%204.png](born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%204.png)

        Oauth 로그인 구현할 때, 이런 방식의 절차를 봤었다.

        1. 호스트 또는 클라이언트가 1개의 쌍을 가진 키를 생성함
            1. `id_rsa.pub` (public key)
            2. `id_rsa` (private key)
        2. 서버는 public key로 만들어진 랜덤한 값을 생성하고 클라이언트에 보냄 (public key로 랜덤 256bit 문자열을 암호화)
        3. 클라이언트는 랜덤한 값을 private key를 이용해 복호화 (1번에서 생성된 키는 1대1로만 매칭됨
        4. 복호화된 난수값을 통해 해시를 만들고 서버로 전송
        5. 서버는 자신이 가진 해시와 방금 받은 해시와 비교하여 정상적인 사용자인지 체크

### UFW (Uncomplcated FireWall)

- 방화벽
    - 보안확보를 위해 내부 네트워크와 외부통신을 제어하고, 내부 네트워크의 안전을 유지하기 위해 사용하는 기술
- 데비안 및 리눅스에서 작동되고 파이썬으로 개발됨
- 해당 과제의 요구사항중 ufw가 작동해야하며, 4242번 포트만 열려있어야함

    ```bash
    sudo ufw enable #활성화
    sudo cat /etc/ufw/user.rules #rules 조회
    sudo ufw allow 8080/tcp # 특정 TCP 포트 허용
    sudo ufw deny 8080/tcp # 특정 TCP 포트 차단
    ```

### TTY (Teletypewriter)

- 콘솔, 터미널, tty는 깊은 연관을 가지고 있으며, 상호작용을 위한 장비를 뜻함
- 유닉스 시스템의 기본적인 이용방법은 유닉스가 인스톨된 호스트에 네트워크를 통해서 다른컴퓨터가 접근하여 작업을 수행하는것임. 이때 사용자로 부터 명령을 받아 전달하는 역할을 콘솔이 담당
- 콘솔은 컴퓨터를 조작할 때 사용하는 장치
    - 터미널이나 TTY도 그에 속한다고 보면된다.
    - 실제 물리적인 장치가 연결된것이 아니기 때문에, 커널에서 터미널을 에뮬레이션 한다.

### aptitude와 apt (debian선택시)

- 기능
    - 응용프로그램  설치 및 삭제
    - 응용프로그램 최신버전 유지 등
- 차이점
    - apt (Advanced Package Tool)
        - low-level 패키지 매니저
        - 다른 high-level 패키지 매니저에 의해 사용될 수 있음
        - 데비안 패키지를 설치,제거하는데 사용되던 도구(apt-get, apt-cache)들이 생기면서 기능이 흩어지고 문제가 발생하면서, 그를 해결하기 위해 필요한 기능만 넣어 편리하게 만든것
    - aptitude
        - high-level 패키지 매니저
        - 텍스트기반 대화형 UI가 제공되는 debian패키지 관리자
        - aptitude는 설치, 제거, 업데이트 과정에서 충돌이 있는 경우 다른 대안을 제시해줌. apt는 그냥 안 된다고만 함.
- 부가적인 이야기
    - aptitude가 기능이 더 다양하다.
    - brew, yum도 apt처럼 패키지를 관리하기 위해 사용하는것이다. 운영체제마다 사용하는 패키지매니저가 달라서 [nodejs를 설치할 때 운영체제마다 설치방법이 다르다.](https://ooeunz.tistory.com/5)

### APPArmor 란? (debian선택시)

- 노벨에서 만든 보안솔루션==리눅스 보안 모듈로 오픈소스임
- 임의의 애플리케이션에 대한 잠재적인 공격범위를 줄임. 어떻게?
    - 특정 프로그램이나 컨테이너에서 필요한 리눅스 기능, 네트워크 사용, 파일 권한 등에 대한 접근을 허용하는 프로파일로 구성

        ```bash
        # cat /etc/apparmor.d/usr.sbin.tcpdump
        #include <tunables/global>

        /usr/sbin/tcpdump {
          #include <abstractions/base>
          #include <abstractions/nameservice>
          #include <abstractions/user-tmp>

          capability net_raw,
          capability setuid,
          capability setgid,
          capability dac_override,
          network raw,
          network packet,

          # for -D
          capability sys_module,
          @{PROC}/bus/usb/ r,
          @{PROC}/bus/usb/** r,

          # for -F and -w
          audit deny @{HOME}/.* mrwkl,
          audit deny @{HOME}/.*/ rw,
          audit deny @{HOME}/.*/** mrwkl,
          audit deny @{HOME}/bin/ rw,
          audit deny @{HOME}/bin/** mrwkl,
          @{HOME}/ r,
          @{HOME}/** rw,

          /usr/sbin/tcpdump r,
        }
        ```

        - 프로파일의 특성
            - profile은 텍스트 파일로 구성한다.
            - 주석을 지원한다.
            - 파일의 경로를 지정 할 때 glob 패턴(*.log와 같은)을 사용할 수 있다.
            - r(read), w(write), m(memory map), k(file locking), l(create hard link)등 파일에 대한 다양한 접근 제어가 가능하다.
            - 네트워크에 대한 접근 제어
            - capability에 대한 제어
            - #include를 이용해서 외부 프로파일을 사용할 수 있다.
    - 동료평가시 슬랙화면을 공유하려면 맥의 보안 설정에서 값을 바꿔주었듯이, 비슷하게 그 보안에 대한 설정을 텍스트파일로 저장하여 관리하는듯하다.

### cron 이란?

> 요구사항중 특정시간마다 작동하는 쉘이 있다. 그 부분을 작성하기 위한 지식

- cron은 유닉스 계열의 job scheduler이다.
    - job
        - 특정 작업이나 프로세스
    - scheduler
        - 특정한 시간마다 혹은 특정한 이벤트 발생시 job을 자동으로 실행하는 것
- 반복적인 일들을 자동화해주며 많은곳에서 두루 활용된다.
    - 특정시간 마다 DB를 백업할 수 도 있고, 특정시간마다 웹페이지를 스크래핑하는 cron을 만들수도 있다.
    - GitHub action을 활용해서 python [로또 당첨번호를 가져와 gitgist에 업데이트하는 cron](https://github.com/padawanR0k/k-lotto/actions/runs/888393912/workflow)을 만들어본 적이 있었다.
- cron 표현식

    설명을 봐도 바로 작성하기는 쉽지않다. 이를 위해 UI를 통해 표현식을 만들어주는 [사이트](http://www.cronmaker.com/;jsessionid=node0ke8z6wemr09rclfib2ba4o7576409.node0?0)도 있다.

    ![born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%205.png](born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%205.png)

    - **초(Seconds)**
        - 값 범위 : 0 ~ 59
        - 허용 특수문자 : `, - /`
        - 리눅스/유닉스 크론탭에서는 사용되지 않는다.
    - **분(Minutes)**
        - 값 범위 : 0 ~ 59
        - 허용 특수문자 : `, - /`
    - **시(Hours)**
        - 값 범위 : 0 ~ 23
        - 허용 특수문자 : `, - /`
    - **일(Day of month)**
        - 값 범위 : 1 ~ 31
        - 허용 특수문자 : `, - ? L W`
    - **월(Month)**
        - 값 범위 : 1 ~ 12 또는 JAN ~ DEC
        - 허용 특수문자 : `, - /`
    - **주(Day of week)**
        - 값 범위 : 0 ~ 6 또는 SUN ~ SAT
        - 허용 특수문자 : `, - ? L #`
    - **년(Year)**
        - 값 범위 : 생략 또는 1970 ~ 2099
        - 허용 특수문자 : `, - /`
        - 리눅스/유닉스 크론탭에서는 사용되지 않는다.

### 데비안 비밀번호 정책

- 과제 요구사항

    `/etc/login.defs` (기본(default) 비밀번호 항목을 지정하는 파일이다)

    - 30일마다 비밀번호가 만료되야함
        - `PASS_MAX_DAYS 30`
    - 비밀번호가 변경되고 최소 2일 후 비밀번호를 변경할 수 있다.
        - `PASS_MIN_DAYS 2`
    - 비밀번호가 만료되기 7일전 유저는 경고메세지를 받을 수 있어야한다.
        - `PASS_WARN_AGE 7`
    - 비밀번호는 10글자 이상, 대문자와 숫자를 포함해야한다. 또한 동일한 문자가 연속3번 이상 존재하면 안된다.
        - `PASS_MIN_LEN 10`
        - `ucredit=-1` (대문자 1개이상)  (1이면 최소 1개의 영어 대문자가 비밀번호에 존재해야함을 의미)
        - `dcredit=-1`  (숫자 1개 이상)
        - `maxclassrepeat=2` (연속3번)

    libpam-pwquality (패스워드 만료 모듈)

    - 유저의 이름이 포함되면 안된다. (뒤집힌 경우도 검사함)
        - `reject_username`
    - 비밀번호 변경시 적어도 7글자는 이전 비밀번호에 포함되면안된다.
        - `difok=7`
    - 루트 비밀번호도 이 정책을 따라야한다.
        - `enforce_for_root`

- **현재비밀번호!!**
    - QV#)V NX*@2
    - Dgjl1541213!!

### 데비안 sudo 관리

sudo에 대한 강력한 구성을 해야한다.

- 과제 요구사항
    1. 일치하지 않는 비밀번호의 입력은 3회로 제한을 둬야한다.
    2. sudo사용시 에러가 발생하게되면 나오는 메세지를 커스터마이징해라
        - Debian에는 sudo가 없으므로 sudo 패키지를 설치
    3. sudo 를 사용하면서 발생하는 Input/Output들은 모두 기록되어야한다. 위치는 `/var/log/sudo`
        - 해당 폴더를 생성해준다.
    4. 보안적인 이유로 TTY모드는 활성화되어야한다.
    5. 보안적인 이유로, sudo에서 사용할 수 있는 경로를 제한되어야한다.
        - `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin`

- 데비안에서 sudo는 어떻게 관리하는가?
    - `/ect/sudoers`  파일로 관리
        - `visudo` 명령어로 열어야함. 쓰기권한이 없기때문
        - 파일에는 옵션들을 관리하는데  그에 대한 설명은 [man페이지](https://www.sudo.ws/man/1.8.15/sudoers.man.html)에서 확인할 수 있음
- sudo?
    - 일반 계정에서 Root권한이 필요한 경우, 명령어 앞에  sudo라는 명령어를 붙여 root권한을 가지게 함
    - 그러나 관리자가 해당 일반 계정에 sudo 사용 권한을 주지 않는 경우 사용할 수가 없다

### 모니터링 쉘 작성하기

```
#Architecture: Linux wil 4.19.0-16-amd64 #1 SMP Debian 4.19.181-1 (2021-03-19) x86_64 GNU/Linux
#CPU physical : 1
#vCPU : 1
#Memory Usage: 74/987MB (7.50%)
#Disk Usage: 1009/2Gb (39%)
#CPU load: 6.7%
#Last boot: 2021-04-25 14:45
#LVM use: yes
#Connexions TCP : 1 ESTABLISHED
#User log: 1
#Network: IP 10.0.2.15 (08:00:27:51:9b:a5)
#Sudo : 42 cmd
```

- 아래 내용들을 출력하는 쉘을 작성하고 크론으로 주기적으로 실행시켜야함

    ```
    #!/bin/bash

    printf "#Architecture: "
    uname -a

    printf "#CPU physical: "
    nproc --all

    printf "#vCPU :"
    cat /proc/cpuinfo | grep "processor" | wc -l

    printf "#Memory Usage: "
    free -m | grep "Mem" | awk '{printf "%d/%d (%.2f%%)", $3, $2, $3/$2*100}'

    printf "#Disk Usage: "
    top -b -n 1 | grep -Po '[0-9.]+ id' | awk '{print 100-$1}'

    printf "#CPU load: " 6.7%

    printf "#Last boot: " 2021-04-25 14:45

    printf "#LVM use: " yes
    if [ "$(cat /etc/fstab | grep '/dev/mapper/' | wc -l)" -gt 0];
     then
      echo 'yes';
     else
      echoo 'no';
    fi

    printf "#Connexions TCP :"
    ss -t | grep -i ESTAB | wc -l

    printf "#User log: " 1
    printf "#Network: " IP 10.0.2.15 (08:00:27:51:9b:a5)
    printf "#Sudo :" 42 cmd
    ```

    - 현재 OS이름과 커널 버전 ([링크](https://udpark.tistory.com/99))
        - `uname`
    - 물리적으로 설치된 프로세스 갯수 ([링크](https://www.cyberciti.biz/faq/check-how-many-cpus-are-there-in-linux-system/))
        - `nproc --all`
    - 가상으로 설치된 프로세스 갯수 [링크](https://webhostinggeeks.com/howto/how-to-display-the-number-of-processors-vcpu-on-linux-vps/)
        - `cat /proc/cpuinfo | grep processor | wc -l`
    - 사용가능한 램 용량과 이를 %로 표현 ([free 링크](https://www.whatap.io/ko/blog/37/),  [awk 링크](https://recipes4dev.tistory.com/171))
        - `free -m | grep "Mem" | awk '{printf "%d/%dMB (%.2f%%)", ＄3, ＄2, ＄3/＄2 * 100}'`
            - `free`
                - 메모리 사용량 확인
                - `-m`
                    - 옵션으로 메가바이트단위로 표현한다. (과제예시에서 그렇게 보여주고있기때문에)
            - `grep "Mem"`
                - `Mem` 이라는 문자열이 들어간 줄만 필터링
            - `awk`
                - 파일이나 문자열에서 특정 매칭된 정보를 가져올 수 있다.
                - printf를  연계해서 사용할 수 있다.
                - `printf "%d/%dMB (%.2f%%)", ＄3, ＄2, ＄3/＄2 * 100}`
                    - printf 사용법과 동일하다.
                - `$2, $3` 표현은?
                    - awk에서 매칭된 정보들을 컬럼형식으로 가져올수 있는데, 그때 사용하는 필드

                        ![born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%206.png](born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%206.png)

                        ![born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%207.png](born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%207.png)

    - 사용가능한 메모리 용량과 이를 %로 표현 ([df 명령어 링크](https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_%EC%A0%84%EC%B2%B4_%EB%94%94%EC%8A%A4%ED%81%AC_%EC%82%AC%EC%9A%A9%EB%9F%89_%ED%99%95%EC%9D%B8))
        - 남은용량
            - `df -P | grep -v ^Filesystem | awk '{sum += $3} ${sum2 += $2} END { printf "%d/%dGB (%d%%)", sum/1024 ,sum2/1024/1024, sum/sum2}'`
                - `df -P`
                    - `-P`
                        - 한줄 출력 옵션
                - `grep -v ^Filesystem`
                    - `-v ^Filesystem`
                        - `Filesystem`으로 시작하는 줄제외
                - `awk '{sum += $3} ${sum2 += $2} END { printf "%d/%dGB (%d%%)", sum/1024 ,sum2/1024/1024, sum/sum2}`
                    - `{sum += $3} ${sum2 += $2}`
                        - 특정 필드값을 모두 더함
                    - `printf "%d/%dGB (%d%%)", sum/1024 ,sum2/1024/1024, sum/sum2`
                        - `printf`를 활용해 퍼센티지를 표현

    - 프로세서의 사용률 %로 표현 ([top명령어 링크](https://yjshin.tistory.com/entry/Linux-%EB%A6%AC%EB%88%85%EC%8A%A4-CPU-%EC%82%AC%EC%9A%A9%EB%A5%A0-%ED%99%95%EC%9D%B8%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-TOP-%EB%AA%85%EB%A0%B9%EC%96%B4)1, [링크2](https://www.cubrid.com/tutorial/3794195))
        - `top -b -n 1 | grep -Po '[0-9.]+ id' | awk '{print 100-$1}'`
            - `top -b -n 1`
                - 1번만 배치모드로 실행 → 실행시킨 그 타이밍의 값을 가져옴
            - `grep -Po '[0-9.]+ id'`
                - `-P`
                    - 정규표현식으로 필요한 값만 가져옴
                - `-o`
                    - 빈 줄은 무시
    - 마지막 리부트 시각과 날자 ([링크](https://www.cyberciti.biz/faq/unix-linux-getting-current-date-in-bash-ksh-shell-script/) )
        - `who -b | sed 's/system boot //g' |  sed 's/^ *`
            - `sed 's/^ *//g'`
                - 앞공백 삭제
    - LVM 사용여부 ([링크](https://askubuntu.com/questions/202613/how-do-i-check-whether-i-am-using-lvm), [조건문 사용법 링크](https://stackoverflow.com/questions/20612891/is-it-possible-to-pipe-multiple-commands-in-an-if-statement))
        - `if [ "$(cat /etc/fstab | grep '/dev/mapper/' | wc -l)" -gt 0];
         then
          echo 'yes';
         else
          echoo 'no';
        fi`
            - `/etc/fstab`
                - 루트파일 시스템을 볼 수 있으며, lvm파일 시스템은 /dev/mapper로 시작한다.
    - 활성화된 연결의 갯수
        - `ss -t | grep -i ESTAB | wc -l`
        - `ss -t`
            - TCP 연결만 출력
        - `grep -i`
            - case 무시

    - 서버를 사용하는 유저의 수 [링크](https://shaeod.tistory.com/623)
        - `who | wc -l`
    - IPv4주소, MAC 주소
        - `hostname –I`  [링크](https://vitux.com/find-debian-ip-network-address/)
        - `ifconfig -a | grep "ether " | sed 's|ether||g' | sed -E "s/[[:space:]]//g"` (피씬)
    - sudo 명령어로 실행된 명령의 수
        - "grep 'sudo:' /var/log/auth.log | grep 'COMMAND=' | wc -l"
        - [sudo로 실행된 명령들이 기록되는곳](https://unix.stackexchange.com/questions/167935/details-about-sudo-commands-executed-by-all-user)

    ```bash
    #Architecture: Linux wil 4.19.0-16-amd64 #1 SMP Debian 4.19.181-1 (2021-03-19) x86_64 GNU/Linux
    #CPU physical : 1
    #vCPU : 1
    #Memory Usage: 74/987MB (7.50%)
    #Disk Usage: 1009/2Gb (39%)
    #CPU load: 6.7%
    #Last boot: 2021-04-25 14:45
    #LVM use: yes
    #Connexions TCP : 1 ESTABLISHED
    #User log: 1
    #Network: IP 10.0.2.15 (08:00:27:51:9b:a5)
    #Sudo : 42 cmd
    ```

## step by step

1. 이미지 iso파일 다운하기 ([링크](https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/))
2. managed software center 에서 virtual box 다운
3. new 클릭
4. 경로를 백업할 하드로 선택함 (클러스터에서는 매번 사라지기 때문에)
5. os - linux, debian 설정
6. 메모리 1024 설정
7. '새 가상 하드디스크 만들기' , 'VDI' , '동적 할당', 8기가 선택
8. 상단에 `start` 클릭
9. 1에서 받은 iso파일을 불러와 마운트
10. `Install`
11. 나라 설정
    1. other - asia - 한국 찾아서 선택
12. lvm 설정
    1. 비밀번호 Dgjl1541213!!
13. 데비안 아카이브 미러 선택 - 한국 - deb.debian.org
14. HTTP proxy 비움
15. configuring popularity-contest - no
16. software selelction - 기본값만
17. [GRUB boot loader](https://m.blog.naver.com/dudwo567890/130158001734) on a hardest - no
    - 부팅가능하게끔 하기위해 부팅용 디스크?를 설정해야함.
    - 여담이지만, 부팅용 디스크를 HDD를 안쓰고 SSD로 쓰면 부팅이 엄청 빠름
18. Enter device manually
19. 과제에서 요구한 곳을 입력 (/boot)
    1. 부팅 완료. `lsblk` 명령어 입력해서 encryped lvm이 2개 이상인지 확인
20. SSH 서비스를 특정 포트에서 실행되게 하자
    - 설치 [How to Enable SSH on Debian 9 or 10](https://phoenixnap.com/kb/how-to-enable-ssh-on-debian)
    - 포트 변경 [howto-change-ssh-port-on-linux-or-unix-server](https://www.cyberciti.biz/faq/howto-change-ssh-port-on-linux-or-unix-server/)
    - 진행하려는데 sudo가 안먹힌다? → [bash: sudo: command not found](https://unix.stackexchange.com/questions/354928/bash-sudo-command-not-found)
    - `sudo systemctl restart ssh`로 재시작하여 설정 적용
21. UFW을 특정 포트로에서만 작동하게
    - 설치 및 실행 [setup-ufw-firewall-on-ubuntu-and-debian](https://www.tecmint.com/setup-ufw-firewall-on-ubuntu-and-debian/)
    - 기본적으로 접근을 막고, 특정 포트만 허용
        - `sudo ufw default deny`
        - `sudo ufw allow {port번호}`
22. hostname이 조건을 만족하는지 확인
    - `cat /proc/sys/kernel/hostname`
23. 새로운 유저를 만들고 해당 유저를 과제에서 필요로 하는 두 그룹에 참가시키자
    1. 유저생성
        - `adducer username`
    2. [debian sudo user  추가](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=jodi999&logNo=221336259983)
        - `usermode -aG sudo {이름}`
        - `sudo  - {이름}` 으로 새로 만든 사용자로 전환후 `sudo whoami` 로 root 가 뜨는지 확인
    3. [리눅스 그룹 생성/삭제/확인/추가 - groupadd](https://webdir.tistory.com/134)
        - `groupadd 그룹명`
    4. 유저 추가 (tom으로 추가했음, `1001(user42)`그룹에 속해있음)
        - `usermod -aG {그룹명} {이름}`
            - cf) usermod에 -G옵션을 붙이면 명시한 그룹에 사용자가 속하게 되고, 현재 사용자가 속해 있는 그룹이 명시되지가 않았다면 해당 그룹에 속하지 않게 된다.
                - (ex. chanlee라는 사용자가 user42와 home이라는 그룹에 속한 상태에서 "usermod -G sudo,user42 chanlee"를 실행하는 경우 sudo와 user42에만 속하게 된다.)
                - 이때 -a옵션(append)을 붙이게 되면 커맨드에 적어준 그룹에 '추가적으로' 들어가게 되는 효과만 발생한다.
        - 그룹 리스트 확인
            - `cat /etc/group`
            - `cat /etc/group | grep user42`
        - 유저 리스트 확인
            - `cat /etc/passwd`
        - 유저가 그룹에 속하는지 확인
            - `id username`
24. 강력한 패스워드 정책을 설정하자
    1. `su -`,   `sudo vi /etc/login.defs`
        1. `:160` 이동
        2. `PASS_MAX_DAYS 30  PASS_MIN_DAYS 2 PASS_WARN_AGE 7 PASS_MIN_LEN 10` 수정
    2. `sudo apt install libpam-pwquality` 패키지 설치
    3. `sudo vi /etc/pam.d/common-password`  파일 수정
    4. `:25` 이동
    5. `retry=3 minlen=10 difok=7 ucredit=-1 dcredit=-1 reject_username enforce_for_root maxclassrepeat=3` 수정
    6. `passwd -e 사용자명`
        1. root계정과 현존하는 사용자 계정의 암호 변경을 강제한다. 다음 번 로그인시에 암호를 변경하라고 뜨게 된다.
25. sudo 정책을 설정하자
    1. `mkdir /var/log/sudo`
    2. `sudo visudo /etc/sudoers`

        ```
        Defaults	authfail_message="Authentication failed :("
        Defaults	badpass_message="Wrong password :("
        Defaults	log_input
        Defaults	log_output
        Defaults	requiretty
        Defaults	iolog_dir="/var/log/sudo/"
        Defaults	passwd_tries=3
        Defaults	secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
        ```

        - authfail_message
            - 권한 획득 실패 시 출력하는 메세지이다.
            - ctrl+c

                ![born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%208.png](born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%208.png)

        - badpass_message
            - sudo사용시 에러가 발생하게되면 나오는 메세지를 커스터마이징할 메세지

            ![born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%209.png](born2beroot%206bba84afcc724f9baee43795a03c63dd/Untitled%209.png)

        - log_input, log_output
            - sudo 명령어 실행시 입력 명령어, 출력 결과를 로그로 저장
        - requiretty
            - sudo 명령어 실행시 tty강제
                - ex) 쉘스크립트내부에 `sudo` 넣고 실행불가
        - iolog_dir
            - 로그가 저장될 위치지정
            - `/var/log/sudo/00/00` 디렉토리별  설명

                log

                - sudo 실행 시 실행한 위치와 실행한 명령어의 위치가 저장되어 있다.

                stderr

                - sudo로 실행한 명령어가 오류로 인해 실행되지 않았을 시 출력되는 내용이 저장되어 있다.

                stdin

                - sudo로 실행한 명령어가 표준 입력을 받은 내용이 저장되어 있다.

                stdout

                - sudo로 실행한 명령어가 표준 출력으로 결과를 출력한 내용이 저장되어 있다.

                timing

                - session timing file.

                ttyin

                - sudo로 실행한 명령어가 tty로 입력받은 내용이 저장되어 있다.

                ttyout

                - sudo로 실행한 명령어가 tty로 출력한 결과가 저장되어 있다.
        - passwd_tries
            - sudo 시도시 비밀번호 재입력 횟수제한
        - secure_path
            - sudo 명령어를 통해 실행되는 명령어의 경로를 제한
26. monitoring.sh를 작성하자
    1. 윗 부분 참고

- 기타
    - 로그인정보
        - yurlee42
            - login: yurlee
            - password: 내 비밀번호

### 평가시 얻었던 TIP

### 참고

- [리눅스 LVM에 관하여 (Centos 6.6 기준)](https://sgbit.tistory.com/12)
- [ssh-keygen으로 인증키 생성하는 원리와 방법](https://brunch.co.kr/@sangjinkang/52)
- [우분투(Ubuntu) 환경에 방화벽(UFW) 설정하기](https://lindarex.github.io/ubuntu/ubuntu-ufw-setting/)
- [chanlee님 정리](https://tbonelee.tistory.com/m/16)
- [https://dora-guide.com/하이퍼바이저/](https://dora-guide.com/%ED%95%98%EC%9D%B4%ED%8D%BC%EB%B0%94%EC%9D%B4%EC%A0%80/)
- [가상 머신(VM)이란?](https://www.redhat.com/ko/topics/virtualization/what-is-a-virtual-machine)
- [서버 가상화 기술의 진화: VM과 컨테이너](https://library.gabia.com/contents/infrahosting/7426/)
- [https://ko.wikipedia.org/wiki/데비안](https://ko.wikipedia.org/wiki/%EB%8D%B0%EB%B9%84%EC%95%88)
- [https://ko.wikipedia.org/wiki/CentOS](https://ko.wikipedia.org/wiki/CentOS)
- [리눅스의 종류](https://hack-cracker.tistory.com/2)
- [apt-vs-aptitude](https://www.fosslinux.com/43884/apt-vs-aptitude.htm)
- [[리눅스] 데비안 apt install과 apt-get install의 차이점](https://reasley.com/?p=683)
- [AppArmor를 사용하여 리소스에 대한 컨테이너의 접근 제한](https://kubernetes.io/ko/docs/tutorials/clusters/apparmor/)
- [apparmor](https://www.joinc.co.kr/w/man/12/apparmor)
- [cron 표현식](https://madplay.github.io/post/a-guide-to-cron-expression)
- [우분투 PC Virtual Box 설치 및 ISO 이미지 부팅](https://idchowto.com/?p=8058)
- [How to Enable SSH on Debian 9 or 10](https://phoenixnap.com/kb/how-to-enable-ssh-on-debian)
- [bash: sudo: command not found](https://unix.stackexchange.com/questions/354928/bash-sudo-command-not-found)
- [howto-change-ssh-port-on-linux-or-unix-server](https://www.cyberciti.biz/faq/howto-change-ssh-port-on-linux-or-unix-server/)
- [debian sudo user  추가](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=jodi999&logNo=221336259983)
- [리눅스 그룹 생성/삭제/확인/추가 - groupadd](https://webdir.tistory.com/134)
- [[리눅스(Linux)] 비밀번호(패스워드) 정책 설정](https://jmoon417.tistory.com/36)
- [[CentOS 8] pwquality를 이용한 패스워드 규칙 적용하는 방법](https://mpjamong.tistory.com/155)
- [리눅스 보안 정책 설정 몇가지](https://josh0766.blogspot.com/2019/05/blog-post_30.html)
- [https://wariua.github.io/linux-pam-docs-ko/sag-pam_cracklib.html](https://wariua.github.io/linux-pam-docs-ko/sag-pam_cracklib.html)