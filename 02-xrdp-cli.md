---
layout: page
title: R 파이썬 소프트레이어 클라우드 그리고 xwMOOC
subtitle: 클라우드 그래픽 사용자 인터페이스(xrdp)와 명령라인 인터페이스(cli) 사용
minutes: 10
---
> ## 학습 목표
>
> *   클라우드 리눅스에 그래픽 사용자 인터페이스(GUI), xrdp를 실습한다.
> *   클라우드 명령라인 인터페이스 (CLI)를 실습한다.


## 클라우드 환경 그래픽 사용자 인터페이스(GUI), xrdp

클라우드에 가상컴퓨터를 생성한 뒤에 데스크탑 환경을 설치하고 VNC 원격 데스크탑 혹은 [XRDP](http://www.xrdp.org/)를 사용해서 윈도우와 마찬가지로 그래픽 사용자 인터페이스를 이용한다.

~~~ {.input}
sudo apt-get install ubuntu-desktop
sudo apt-get install xrdp
sudo /etc/init.d/xrdp start
~~~

원격 컴퓨터에 우분투 데스크탑 환경을 설치하고 나서 XRDP도 이어서 설치한다.
설치가 완료되면 XRP 응용프로그램을 실행한다.

~~~ {.input}
sudo adduser gildong sudo
~~~

윈도우 `원격 데스크톱연결`을 사용하기 전에 `root`가 아닌 사용자를 `gildong`으로 생성하고 `sudo` 권한도 부여한다.

~~~ {.input}
su gildong
echo "gnome-session --session=ubuntu-2d" > .xsession
~~~

`gildong` 사용자 세션 환경을 지정한다. `nano` 편집기를 사용해서 내용을 `.xsession` 파일에 저장해도 된다.

윈도우에서 클라우드 우분투 가상컴퓨터에 로그인 하기 이해서 `시작` -> `실행`을 클릭하고 `mstsc`를 입력하여 
`원격 데스크톱연결`을 실행한다.

![윈도우 원격 데스크톱 연결 화면](fig/02-xrdp-mstsc.png)

마지막으로 XRDP 로그인 화면이 나오고 비밀번호를 넣게 되면 그래픽 사용자 인터페이스를 원격 컴퓨터에서 이용하게 된다.

![XRDP 로그인 화면](fig/02-xrdp.png)

## 클라우드 소프트레이어 파이썬 명령라인 인터페이스 (CLI) 설치

파이썬에서 명령라인 인터페이스를 사용하기 위해서는 먼저 `pip`를 통해 `softlayer` CLI를 설치하기 때문에 만약 `pip` 명령어가 실행되지 않으면 `python-pip`를 설치한 후 `pip`를 통해서 `softlayer` CLI를 설치한다.
파이썬을 사용하기 위해 `python-softlayer`도 설치한다.

~~~ {.input}
$ apt-get install python-pip

$ pip install softlayer

$ sudo apt-get install python-softlayer
~~~

> ## 명령라인 인터페이스 {.callout}
>
> 지금 대부분의 사람들이 사용하는 그래픽 사용자 인터페이스(GUI, graphical user interface)과 구별하기 위해서 
> 명령라인 인터페이스(CLI, command-line interface)라고 한다. 
> CLI의 핵심은 읽기-평가-출력(REPL,read-evaluate-print loop이다. 사용자가 명령어를 타이핑하고 엔터(enter)/반환(return)키를 입력하면, 
> 컴퓨터가 일고, 실행하고, 결과를 출력한다. 그러면 사용자는 다른 명령를 타이핑하는 것을 로그 오프할때까지 계속한다.

정상적으로 소프트레이어 CLI를 설치하면 `slcli` 명령어를 입력하고, 버젼확인 인자를 넣게 되면 출력결과가 다음과 같이 나온다.

~~~ {.input}
root@shiny:~# slcli --version
slcli (SoftLayer Command-line), version 4.0.2
~~~

### 소프트레이어 파이썬 CLI 초기설정

이제 `slcli`를 초기 설정하자.
사용자 이름, API Key 혹은 비밀번호, 종점(Endpoint) 및 타임아웃(Timeout) 정보를 입력한다.

~~~ {.input}
root@shiny:~# slcli setup
Username []: SL......
API Key or Password []:
Endpoint (public|private|custom) [public]: public
Timeout [0]: 30
:..............:..................................................................:
:         Name : Value                                                            :
:..............:..................................................................:
:     Username : SL......                                                         :
:      API Key : 7c.............................                                  :
: Endpoint URL : https://api.softlayer.com/xxxx/xxx/                              :
:      Timeout : 30                                                               :
:..............:..................................................................:
Are you sure you want to write settings to "/root/.softlayer"? [Y/n]: Y
Configuration Updated Successfully

root@shiny:~# slcli config show
~~~

> ## API키 정보 {.callout}
> API키는 [고객 포털](https://control.softlayer.com/)에 로그인한 후,
> 상단 사용자 이름을 클릭하면, `Edit User Profile` 화면으로 넘어간다.
> 스크롤을 내려 `API Access Information` API키 정보를 확인한다.


### 소프트레이어 파이썬 CLI 맛보기

이제 `slcli` `call-api`를 통해 [고객포털](http://control.softlayer.com/)에 
로그인한 후 클릭 등 하지 않고도 계정정보를 확인할 수 있다.

~~~ {.input}
root@shiny:~# slcli call-api Account getObject
:.............................:...........................:
:                        Name : Value                     :
:.............................:...........................:
:                    lastName : Lee                       :
:                        city : Seoul                     :
:                  postalCode : xxx                       :
:                  modifyDate : 2014-11-11T17:55:18-06:00 :
:       lateFeeProtectionFlag :                           :
:                   firstName : xxxxxxxxx                 :
:                 companyName : xwMOOC                    :
:                    address1 : 17 xxxx xx                :
: accountManagedResourcesFlag : False                     :
:             accountStatusId : xxxx                      :
:                  statusDate :                           :
:                     brandId : xx                        :
:                       email : xxxxxxxxx.xxx.x@gmail.com :
:                       state : OT                        :
:      allowedPptpVpnQuantity : 1                         :
:                     country : KR                        :
:                          id : xxxxxx                    :
:                 officePhone : 82.xx.xxxx.xxxx           :
:                  isReseller : 0                         :
:                  createDate : 2014-07-23T19:29:22-06:00 :
:      claimedTaxExemptTxFlag : False                     :
:.............................:...........................:
~~~

`slcli vs list` 명령어를 통해서 현재 운영중인 가상 컴퓨터 목록을 확인할 수 있다.
[고객 포털](https://control.softlayer.com/) 그래픽 사용자 인터페이스를 통해서 
`Device` -> `Device List`를 클릭한 화면과 동일하다.

~~~ {.input}
root@shiny:~# slcli vs list
:.........:...............:.................:................:............:........:
:    id   :    hostname   :    primary_ip   :   backend_ip   : datacenter : action :
:.........:...............:.................:................:............:........:
: 827.... :      d...     : 161............ : 10............ :   tok02    :   -    :
: 622.... : elasti...arch :  119........... : 10............ :   hkg02    :   -    :
: 766.... :     py...n    :  119........... : 10............ :   hkg02    :   -    :
: 882.... :    rst...o    :  169........... : 10............ :   dal09    :   -    :
: 826.... :    rur...e    : 161............ : 10............ :   tok02    :   -    :
: 899.... :     sh...     :  169........... : 10............ :   dal09    :   -    :
: 899.... :      s...     :  158........... :  10........... :   sjc01    :   -    :
: 899.... :     sw...     :  119........... : 10............ :   hkg02    :   -    :
: 623.... :     we...     :  119........... : 10............ :   hkg02    :   -    :
: 937.... :    win...s    :  158........... :  10........... :   sjc01    :   -    :
: 773.... :      w...     :  119........... : 10............ :   hkg02    :   -    :
:.........:...............:.................:................:............:........:
~~~

