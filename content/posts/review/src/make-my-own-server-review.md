---
title: "안 쓰는 컴퓨터로 나만의 서버 만들기"
description: "안 쓰는 컴퓨터를 이용해 Ubuntu Server로 나만의 서버 구축하고 보안 초기 설정하기"
date: 2022-06-27T14:29:40+09:00
hero: /images/hugo.png
theme: Toha
menu:
  sidebar:
    name: "안 쓰는 컴퓨터로 나만의 서버 만들기"
    identifier: make-my-own-server-review
    parent: review
    weight: 2
tags:
  [
    "서버",
    "개인서버",
    "NAS",
    "Ubuntu",
    "Ubuntu server",
    "데스크탑",
    "컴퓨터",
    "중고",
    "fail2ban",
    "ufw",
    "방화벽",
    "unified firewall",
  ]
categories: []
draft: false
---

<br>

## 이 글에서는...

<br>

안 쓰는 일반 데스크탑 컴퓨터를 이용하여 개발 공부, NAS, 홈 자동화 등 다용도로 사용할 수 있는 나만의 서버를 만듭니다.<br>
Ubuntu Server를 설치한 뒤, 보안 및 네트워크 초기 설정을 합니다.<br>

<br>

---

<br>

## 하드웨어와 OS 선택하기

<br>

#### 클라우드, 온프레미스?

<br>

> 온프레미스란 서버를 클라우드 같은 원격 환경에서 운영하는 방식이 아닌, 자체적으로 보유한 서버에 직접 설치해 운영하는 방식을 의미합니다.

<br>

인정한다.<br>
돈을 조금만 내면 아래 과정들을 거치지 않아도 AWS, GCP 등 대형 클라우드 회사로부터 나만의 서버를 대여할 수 있다.<br>
돈을 내고 클라우드 서비스를 이용하는 것이 안정성도 있고 앞으로 여러모로 신경쓸 일을 많이 줄여주는 것이 사실이다.<br>
그럼에도 클라우드 서비스 대신 나만의 온프레미스 서버를 사용해야겠다고 생각한 이유는 다음과 같다.<br>

1. 서버를 직접 운용하며 네트워크 및 보안 지식을 쌓고 싶었다.<br>
2. 하드웨어부터 소프트웨어에 이르기까지 다른 곳에 의존하지 않고 웹사이트나 NAS같은 서비스를 최대한 내가 직접 만들어보고, 문제가 생겼을 경우에도 직접 대처를 해보고 싶었다.<br>
3. 웹사이트 등 개발 공부, 개인용 NAS, 홈 오토메이션 등 다양한 목적으로 사용하고 싶었다. 홈 오토메이션의 경우 서버가 집 네트워크에 속해 있는 것이 유리했다.<br>
4. <small>내 방 한편에서 돌아가는 서버를 보면 심리적 안정감을 느낄 수 있을 것 같았다.</small><br>

<br>

#### 상용 NAS 기기

<br>

처음에 나만의 온프레미스 서버를 만들기 위해서 맨 처음으로 고려했던 것은 상용 NAS 기기였다.<br>

[시놀로지 공식 웹사이트](https://www.synology.com/ko-kr)<br>

[QNap 공식 웹사이트](https://www.qnap.com/ko-kr)<br>

NAS 기기는 기본적으로 OS가 기본 설치되어 있을 뿐만 아니라 보안 설정도 기본으로 되어 있고, 애초에 24시간 가동을 염두에 두고 만들어진 물건이기 때문에 발열 및 안정성 등이 보장된다는 장점이 있었다.<br>
하지만 NAS는 Network Attatched **Storage**, 즉 스토리지 기능을 중점으로 두고 있는 기계이다.<br>
데이터베이스 CRUD 작업이나 백업 정도를 수행할 정도의 성능이면 충분하기 때문에, 많은 NAS 기기들이 높은 성능을 가진 하드웨어를 사용하기보다는 저전력, 저소음을 위해 어느 정도의 타협점을 두고 (셀러론, 펜티엄 급) 하드웨어를 사용하고 있었다.<br>
또한, 컴퓨터 공학도가 아닌 일반 사용자들이 손쉽게 NAS를 사용할 수 있도록 하기 위해, 거의 모든 NAS 기기들은 OS와 보안, 그리고 여러 종류의 소프트웨어들을 기본 탑재하고 있다.<br>
그때문에 NAS 기기는 그 갖고 있는 하드웨어 성능보다 더 높은 가격대를 형성하고 있는데, 컴퓨터 공학에 관심 또는 관련 지식이 있다면 굳이 NAS 기기가 없더라도 커버할 수 있겠다는 생각이 들었다.<br>

<br>

#### 데스크탑 PC

<br>

그렇게 마지막으로 선택한 것은 데스크탑 PC였다.<br>
위에서도 언급했었듯이, 나는 내 서버를 동시에 다양한 목적으로 사용하고 싶었고, NAS 기기를 사용한다면 앞으로 더욱 성능이 필요할 때 스케일 업을 하기 힘들 수 있겠다는 생각이 들었다.<br>
또한 네트워크 및 보안 지식을 쌓고 여러 경험을 하기 위해서는, 데스크탑 PC에 내 서버를 돌리는 것이 적합하겠다고 생각했다.<br>

<br>

#### OS 선택하기

<br>

안 쓰는 데스크탑 PC에 서버를 돌리기로 결정한 뒤에는 서버에 올릴 운영체제를 선택해야 했다.<br>
구글링을 통해 다음 대안들을 얻어낼 수 있었다.<br>

- OMV (OpenMediaVault) [(공식 웹사이트)](https://www.openmediavault.org/)<br>

- freenas [(유저 가이드)](https://www.ixsystems.com/documentation/freenas/11.3-U5/freenas.html)<br>

- XigmaNAS (예전 이름: NAS4FREE) [(공식 웹사이트)](https://xigmanas.com/xnaswp/)<br>

- Ubuntu [(공식 웹사이트)](https://ubuntu.com/)<br>

기존에는 위 3개의 운영체제를 고려했었으나, 다용도로 사용하기에는 Ubuntu Server를 사용하는 것이 좋을 것이라고 생각했다.<br>
(CentOS, OpenSuse, Fedora 등 다양한 선택지가 있으나, Ubuntu GUI 버전을 사용한 경험이 있었기 때문에 이후 관리가 쉬울 것이라고 생각해 Ubuntu Server를 선택했다)<br>
따라서 안 쓰는 데스크탑 컴퓨터에, Ubuntu Server를 설치하는 것으로 하기로 최종적으로 결정했다.<br>

<br>

#### 하드웨어 준비하기

<br>

이 글을 읽는 사람들 중 많은 사람들이 집에 안 쓰는 데스크탑을 갖고 있을 것이라는 기대를 하고 이 글의 제목을 '안 쓰는 컴퓨터로 나만의 서버 만들기'로 했지만, 사실 필자는 안 쓰는 데스크탑이 없었다.<br>
따라서 하드웨어를 구하기 위해 처음으로 당근마켓을 이용해 저전력 중고 데스크탑을 구매해보았다.<br>
딩근으로 구매한 데스크탑의 성능은 다음과 같다.<br>

- 프로세서: i3-6100
- 메모리: 8GB
- 외장 VGA: 없음

<br>

---

<br>

## Ubuntu Server 설치하기

<br>

[Ubuntu Server 설치 공식 매뉴얼](https://ubuntu.com/tutorials/install-ubuntu-server#1-overview)

<br>

#### iso 파일 다운로드하기

<br>

[Ubuntu Server 공식 다운로드](https://ubuntu.com/download/server)<br>

> 주의: Ubuntu Server는 대부분 PC에서 사용하는 GUI 버전이 아닌, 텍스트 기반 명령어를 통해 동작하는 CLI 버전입니다.<br>
> GUI 버전을 사용하려면 [Ubuntu Desktop](https://ubuntu.com/download/desktop)을 다운로드해야 합니다.<br>

`Option 2 - Manual server installation`을 선택한 뒤 `Download Ubuntu Server **.** LTS` 버튼을 눌러 iso 파일을 다운로드하면 된다.<br>
(글 쓰는 현재 최신 LTS 버전은 22.04 LTS 버전이고, 필자도 이 버전을 다운로드했다)<br>
LTS(Long term support) 버전의 경우 몇년 간 OS 업데이트를 지원해주기 때문에, 특별한 경우가 아니라면 LTS 버전을 선택하는 것이 좋다.<br>
만약 소프트웨어 호환 등 다른 이유가 있다면 `Alternative downloads`에서 다른 버전을 찾아보자.<br>

<br>

#### 디스크 이미지 만들기

<br>

[balenaEtcher 공식 홈페이지](https://www.balena.io/etcher/)<br>

디스크 이미지 생성 프로그램을 이용해 USB 또는 DVD 등을 bootable drive로 만들어주어야 한다.<br>
이때, bootable drive로 사용할 스토리지는 8GB 이상 용량을 사용할 것을 권장한다.<br>
나는 balenaEtcher라는 소프트웨어를 사용했지만, 기호에 따라 Rufus 등 다른 소프트웨어를 사용해도 무방하다.<br>
balenaEtcher를 사용한다면, `Select Image` 버튼을 눌러 위에서 다운로드받은 iso 이미지를 넣어주고, `Select Drive` 버튼을 눌러 bootable drive로 만들어주고 싶은 USB 또는 DVD 등을 선택해준 뒤 `Flash!` 버튼을 누르기만 하면 된다.<br>
시간이 몇 분 소요된다.<br>

<br>

#### OS 초기 설정하기

<br>

Ubuntu Server를 설치하고자 하는 컴퓨터에 bootable drive를 넣은 뒤, 컴퓨터를 시작하고 잠시 기다리면 초기 설정 화면이 등장한다.<br>

<br>

> 컴퓨터에 이미 설치된 운영체제가 있어 Ubuntu Server 초기 설정 화면이 나오지 않는 경우, `f2`, `f8`, `f12`, `del` 등의 키를 눌러 바이오스에 진입한 뒤 부팅 우선순위를 변경해주어야 한다.<br>
> 메인보드에 따라 바이오스 진입 단축키와 부팅 우선순위 변경 방법이 다르므로, 본인 컴퓨터 메인보드 제조사의 부팅 우선순위 변경 방법을 검색해보자.<br>

> 주의: 이 과정 이후에 원래 설치된 운영체제를 포함해 드라이브의 모든 데이터가 삭제되므로, 필요한 파일이 있다면 미리 백업해두어야 합니다.<br>

<br>

나는 다음과 같이 초기 설정을 했다.<br>

<br>

- Select your language: `English`
- Installer update available: `Continue without updating`
  _(이유는 정확히 모르겠지만, `Update to the new installer`를 선택했을 때 설치가 정상적으로 완료되지 않아 `Continue without updating` 옵션으로 다시 설치했었다)_
- Keyboard Configuration:
  - Layout: `Korean`
  - Variant: `Korean - Korean (101/104 key compatible)`
  - `Done`
    _(한/영 키가 입력되지 않는 문제 등이 발생할 수 있으므로 Variant: Korean이 아닌 위 옵션을 사용하는 것을 권장합니다)_
- Network connections: `Done`
- Configure proxy: `Done`
- Configure Ubuntu archive mirror: `Done`
- Guided storage Configuration:
  - `Custom storage layout`
  - `Done`
    _(전체 디스크를 사용하려면 `Use an entire disk`를, RAID 및 기타 파티션 옵션을 주고 싶다면 `Custom storage layout`을 선택합니다)_
- Confirm destructive action: `Continue`
- Profile setup: 서버 이름, 아이디, 비밀번호, 비밀번호 확인 입력
- SSH Setup:
  - `Install OpenSSH server`
  - `Done`
- Featured Server Snaps:
  - `docker`
  - `Done`
    _(나중에도 설치 가능하기 때문에, 아무 것도 체크하지 않고 넘어가도 무방합니다)_
- Installation Complete: `Reboot`

<br>

재부팅 이후 서버가 켜진다.<br>
`Username:` 과 `Password:`와 같이 입력하라는 프롬프트가 출력되면, 위 Profile setup에서 입력했던 아이디와 비밀번호를 입력하면 된다.<br>

<br>

---

<br>

## 보안 설정하기

<br>

#### 방화벽 설정하기

<br>

##### UFW

<br>

UFW는 unified firewall의 약자로, 다양한 리눅스 환경에서 동작하는 오픈소스 방화벽이다.<br>
기본으로 설치되는 `iptables`를 이용하여 방화벽을 설정할 수도 있지만, UFW를 이용하면 더욱 쉽게 (높은 추상화 수준에서) 방화벽을 관리하고 구성할 수 있다.<br>

`sudo apt-get install ufw` 명령어를 이용해 UFW를 설치할 수 있다.<br>

<br>

##### UFW에서 자주 사용되는 명령어

<br>

자주 사용되는 명령어는 다음과 같다.<br>
`ERROR: You need to be root to run this script`와 같이 관리자 권한이 필요하다는 메시지가 출력되면, 명령어 앞쪽에 `sudo`를 붙이자.<br>

- `ufw status`
  - ufw가 켜져 있는지 상태와, 허용/거부된 포트를 보여준다.
- `ufw status verbose`
  - 위 정보에 더해 로깅, 기본 정책 등 더 많은 정보를 보여준다.
- `ufw enable`
- `ufw disable`
  - ufw를 켜거나 끈다. ufw의 변경 사항을 적용하기 위해서는 disable → enable을 통해 ufw를 재시작해주어야 한다.
- `ufw default [allow|deny] [incoming|outgoing]`
  - ufw의 기본 정책을 변경한다. ufw는 기본값으로, 들어오는 패킷은 모두 거부하고, 나가는 패킷은 모두 허용한다.
- `ufw allow [in|out] <포트 번호>/[tcp|udp]`
- `ufw deny [in|out] <포트 번호>/[tcp|udp]`
  - 주어진 포트 번호를 허용/거부한다.
  - `in` 또는 `out`을 명시하지 않으면 양방향에 대해 적용한다.
  - 프로토콜(`/tcp` 또는 `/udp`)은 선택적 인자로, 적지 않아도 된다.
  - 포트 번호 대신 어플리케이션 이름을 적어도 된다. (예: `ufw allow OpenSSH`)
  - 어플리케이션 목록은 `ufw app list`로 확인할 수 있다.
- `ufw delete allow [in|out] <포트 번호>/[tcp|udp]`
- `ufw delete deny [in|out] <포트 번호>/[tcp|udp]`
  - 이미 설정한 포트 설정/허용 정책을 지운다.
- `ufw reset`
  - 설정된 모든 정책을 지우고 기본값으로 되돌린다.

이외에도 특정 ip로부터 오는 커넥션 거부, 로깅 수준 설정 등 다양한 기능이 있다.<br>
본인이 원하는 방화벽 구성에 따라 설정해주자.<br>

<br>

#### fail2ban 적용하기

<br>

##### fail2ban

<br>

fail2ban은 그 이름에서도 알 수 있다시피, 악의적인 사용자가 접속 시도를 정해진 횟수만큼 실패했을 때 사용자를 차단하는 기능을 가진 소프트웨어이다.<br>
파이썬으로 개발되었으며, 접근 로그 파일을 읽어 접근 실패를 몇 회 이상 하는 경우 사용자를 차단하는 역할을 한다.<br>
서버를 인터넷에 열어놓으면 악의적인 사용자로부터 포트 스캐닝, 브루트포스 공격 등을 받기 쉬운데, fail2ban은 단순한 브루트포스 공격으로부터 서버를 지키는 방어벽이 되어준다.<br>
특히 SSH를 통해 원격으로 서버를 관리하려는 경우, 악의적인 사용자가 열린 SSH 포트를 통해 무작위로 로그인을 시도할 수 있기 때문에 보안 강화를 위해 fail2ban을 필수적으로 설치하는 것을 권장한다.<br>

<br>

fail2ban을 설치하려면 `sudo apt-get install fail2ban` 명령어를 입력한다.<br>
설치가 완료되었다면 `sudo systemctl enable fail2ban`으로 부팅 시 자동으로 시작되도록 설정한다.<br>
`sudo vi /etc/fail2ban/jail.local`을 통해 세부적인 설정을 해줄 수 있다.<br>

<br>

필자의 설정 값은 다음과 같다. (설명을 위해 주석을 더함)<br>

```cmd
[DEFAULT]
# findtime동안 maxretry만큼의 로그인 실패가 있을 시, bantime동안 해당 ip를 밴함.
findtime = 1d (기본 단위: 초)
maxretry = 5 (단위: 회)
# bantime (기본 단위: 초, 기본 설정: 10분, -1=permanant)
bantime = 1w

# 로그 파일 변경을 감지할 방법. pyinotify, gamin, polling, systemd, auto 중 선택
backend = systemd

# 이메일 알림
destemail = yechan24680@gmail.com
sendername = fail2ban@192.168.55.240
mta = sendmail

# ignoreip 에 설정된 IP 는 로그인을 실패해도 차단하지 않음. 기본 설정은 localhost
#ignoreip = 000.000.000.000/00

# service
[sshd]
enabled = true
port = 0 # 본인이 사용할 포트 기입. 기본값은 22
filter = sshd
logpath = /var/log/fail2ban-ssh.log
```

<br>

설정을 완료했다면 `:wq`를 통해 vim 편집기를 빠져나온다.<br>

<br>

##### fail2ban에서 자주 사용되는 명령어

<br>

- `fail2ban-client status`
  - fail2ban의 상태를 보여준다.
- `sudo systemctl enable fail2ban`
  - fail2ban 서비스 시작
- `sudo systemctl reload fail2ban`
  - fail2ban 설정 값 다시 읽기
- `sudo systemctl restart fail2ban`
  - fail2ban 재시작
- `sudo fail2ban-client set <jail 이름> unbanip <ip 주소>`
  - 차단된 ip 주소를 다시 허용시킨다. (예: `sudo fail2ban-client set sshd unbanip 123.210.1.2`)

<br>

---

<br>

## 마치며

<br>

지금까지 데스크탑 컴퓨터에 Ubuntu Server를 설치한 뒤, 기본적인 보안 설정을 해 주었다.<br>
필자는 이렇게 서버를 구성한 뒤 한 달 정도 서버를 운용 중인데, 외부에 서버를 노출시켰음에도 안전하게 서비스 중이다.<br>
이 글이 Ubuntu Server를 기본 설정하는 사람들에게 도움이 되었으면 한다.<br>
서버를 구성하고 보안 설정을 하는 데에 [이 연재글 시리즈](https://nitr0.tistory.com/323)를 보고 많은 도움을 얻었으니, 참고하면 좋을 듯하다.<br>

<br>

필자는 위와 같이 서버를 구성하기까지 몇 시간정도 걸렸다.<br>
개인 NAS 또는 서버를 구축하는 데에 관심이 있는 사람들에게는, Ubuntu로 서버를 구축하는 것이 생각보다 아주 어렵거나 복잡하지 않으니 한 번 남는 PC로 도전해보라고 말해주고 싶다.<br>

<br>
