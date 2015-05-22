---
layout: page
title: R 파이썬 소프트레이어 클라우드 그리고 xwMOOC
subtitle: 명령라인 인터페이스 활용
minutes: 10
---
> ## 학습 목표
>
> *   명령라인 인터페이스를 사용하여 클라우드를 활용한다.
> *   가상 컴퓨터를 조회, 생성, 갱신, 삭제한다.
> *   스케일업과 스케일다운으로 명령라인 인터페이스로 수행한다. 

마치 데이터베이스의 CRUD 연산과 유사하게 명령라인 인터페이스를 통해서 
가상 컴퓨터를 생성(Create), 조회(Read), 갱신(Update), 삭제(Delete)할 수 있다.

## 들어가며

클라우드에 운영중인 컴퓨터가 다양한 이유로 인해서 변경이 된다. 과거 컴퓨터가 한대라면 모든 것이 명확하지만, 사용자의 목적에 맞게 빅데이터 분석용, 앱서비스 운영용, IoT 배포용, 테스트 개발용 등 다양한 컴퓨터를 수십 수백대 운영한다면 그 특정 컴퓨터를 찾아서 뭔가 조치를 취하는 것도 쉬운 일은 아니다. 

우선 다음과 같은 전자우편으로 클라우드 서비스 운영업체에서 받았다고 가정하자. 특정 웹서버스를 운영중인 가상컴퓨터에 우분투 10.04가 설치되어 있는데 더이상 지원을 하지 못한다고 한다. 만약 적절한 조치를 취하지 않는다면 우분투 10.04 버젼을 계속 운영하면 발생되는 모든 문제는 사용자 문제로 귀책된다. 

~~~ {.output}
Customer Identification: xwMOOC (XXXXXX)
Start Date: Wednesday 15-Apr-2015 00:00 UTC
End Date: Saturday 20-Jun-2015 23:59 UTC
Duration: 2 months
Event Type: Planned Event
Subject: Event 17577385 - Ubuntu Linux 10.04 LTS End of Sale/End of Support Notice

=================================================================
/ Event Description /
Dear Customer,

This notification is to inform you that on 4/15/2015 we will discontinue sales of Ubuntu Linux 10.04 LTS, and on 06/20/2015 we will discontinue support.  One or more of your servers will be impacted by this process.

In order to continue using a supported version of Ubuntu Linux, we recommend upgrading to Ubuntu Linux 12.04 Precise Pangolin or Ubuntu Linux 14.04 Trusty Tahr, both available at no charge through an OS Reload.

Should you have any questions or concerns, please reach out to a member of the Sales team by opening a ticket or initiating a chat.

Best Regards,

Your Sales Team


/ Items Associated With This Event /
82XXXX | rur-ple.xwmooc.net | XXX.XXX.XXX.XXX | XX.XXX.XX.XXX | Virtual Server
~~~

## 전체 컴퓨터 혹은 디바이스 개괄하기

전자우편을 통해서 클라우드 제공업체가 영향을 받는 클라우드 서버 컴퓨터를 특정해서 알려줬다. 하지만, 현재 운영중인 클라우드 가상 컴퓨터와 디바이스가 어떤 것이 있는지 자세한 세부정보는 어떻게 되는지 확인해보자. 

~~~ {.input}
root@shiny:~# slcli vs list
~~~

~~~ {.output}
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

`id`, `hostname`, `primary_ip`, `backend_ip`, `datacenter`, `action`이 나와있다.
가상컴퓨터 세부적인 사항을 조회하는 방법은 다음 가상 컴퓨터 생성을 한후에 좀더 구체적으로 다룬다.

## 가상 컴퓨터 생성(Create)

소프트레이어 명령라인에서 가상컴퓨터(Virual Machine)을 주문하자. 먼저 가능한 선택사항에 대해서 확인해보자.
`slcli vs create-options` 을 입력하면 선택가능한 컴퓨터 사양이 출력된다.

~~~ {.output}
root@shiny:~# slcli vs create-options
:.................:.......................................................:
:            Name : Value                                                 :
:.................:.......................................................:
:      datacenter : ams01,dal01,dal05,dal06,dal09,hkg02,hou02,lon02,mel01,:
:  cpus (private) : 1,2,4,8                                               :
: cpus (standard) : 1,2,4,8,12,16                                         :
:          memory : 1024,2048,4096,6144,8192,12288,16384,32768,49152,65536:
:     os (CENTOS) : CENTOS_5_32                                           :
:                 : CENTOS_5_64                                           :
:                 : CENTOS_6_32                                           :
:                 : CENTOS_6_64                                           :
:                 : CENTOS_7_64                                           :
:                 : CENTOS_LATEST                                         :
:                 : CENTOS_LATEST_32                                      :
:                 : CENTOS_LATEST_64                                      :
: os (CLOUDLINUX) : CLOUDLINUX_5_32                                       :
:                 : CLOUDLINUX_5_64                                       :
:                 : CLOUDLINUX_6_32                                       :
:                 : CLOUDLINUX_6_64                                       :
:                 : CLOUDLINUX_LATEST                                     :
:                 : CLOUDLINUX_LATEST_32                                  :
:                 : CLOUDLINUX_LATEST_64                                  :
:     os (DEBIAN) : DEBIAN_6_32                                           :
:                 : DEBIAN_6_64                                           :
:                 : DEBIAN_7_32                                           :
:                 : DEBIAN_7_64                                           :
:                 : DEBIAN_LATEST                                         :
:                 : DEBIAN_LATEST_32                                      :
:                 : DEBIAN_LATEST_64                                      :
:     os (REDHAT) : REDHAT_5_32                                           :
:                 : REDHAT_5_64                                           :
:                 : REDHAT_6_32                                           :
:                 : REDHAT_6_64                                           :
:                 : REDHAT_LATEST                                         :
:                 : REDHAT_LATEST_32                                      :
:                 : REDHAT_LATEST_64                                      :
:     os (UBUNTU) : UBUNTU_10_32                                          :
:                 : UBUNTU_10_64                                          :
:                 : UBUNTU_12_32                                          :
:                 : UBUNTU_12_64                                          :
:                 : UBUNTU_14_32                                          :
:                 : UBUNTU_14_64                                          :
:                 : UBUNTU_LATEST                                         :
:                 : UBUNTU_LATEST_32                                      :
:                 : UBUNTU_LATEST_64                                      :
:   os (VYATTACE) : VYATTACE_6.5_64                                       :
:                 : VYATTACE_6.6_64                                       :
:                 : VYATTACE_LATEST                                       :
:                 : VYATTACE_LATEST_64                                    :
:        os (WIN) : WIN_2003-DC-SP2-1_32                                  :
:                 : WIN_2003-DC-SP2-1_64                                  :
:                 : WIN_2003-ENT-SP2-5_32                                 :
:                 : WIN_2003-ENT-SP2-5_64                                 :
:                 : WIN_2003-STD-SP2-5_32                                 :
:                 : WIN_2003-STD-SP2-5_64                                 :
:                 : WIN_2008-DC-R2_64                                     :
:                 : WIN_2008-DC-SP2_64                                    :
:                 : WIN_2008-ENT-R2_64                                    :
:                 : WIN_2008-ENT-SP2_32                                   :
:                 : WIN_2008-ENT-SP2_64                                   :
:                 : WIN_2008-STD-R2-SP1_64                                :
:                 : WIN_2008-STD-R2_64                                    :
:                 : WIN_2008-STD-SP2_32                                   :
:                 : WIN_2008-STD-SP2_64                                   :
:                 : WIN_2012-DC_64                                        :
:                 : WIN_2012-STD_64                                       :
:                 : WIN_LATEST                                            :
:                 : WIN_LATEST_32                                         :
:                 : WIN_LATEST_64                                         :
:   local disk(0) : 25,100                                                :
:   local disk(2) : 25,100,150,200,300                                    :
:     san disk(0) : 25,100                                                :
:     san disk(2) : 10,20,25,30,40,50,75,100,125,150,175,200,250,300,350  :
:     san disk(3) : 10,20,25,30,40,50,75,100,125,150,175,200,250,300,350  :
:     san disk(4) : 10,20,25,30,40,50,75,100,125,150,175,200,250,300,350  :
:     san disk(5) : 10,20,25,30,40,50,75,100,125,150,175,200,250,300,350  :
:             nic : 10,100,1000                                           :
:.................:.......................................................:
~~~

상기 가능한 선택사항을 검토하고 가상 컴퓨터를 주문해보자. 주문하는 가상 컴퓨터 사양은 다음과 같다.
컴퓨터/호스트명은 `slcli`, 도메인명은 `xwmooc.net`, 데이터센터는 미국 달라스 소재 `dal09`,
프로세서갯수는 `2`개, 주기억장치는 `1,024 MB`, 운영체제는 우분투, 보조기억장치는 `25 GB`, 
공용 `public`, 요금청구단위는 시간당이 아닌 `monthly` 월단위 정액제로 사양서를 만들어 주문한다.


|   구성항목     |  명칭           |  선택 사양   |
| -------------|:----------------|:-------------|
| 호스명        |  hostname       |  slcli       |
| 도메인명       |  domain         |  xwmooc.net  |
| 데이터센터     |  datacenter     |  dal09       |
| 프로세서갯수   |  cpu            |  2           |
| 주기억장치    |  memory         |  1024        |
| 운영체제      |  os             |  UBUNTU_14_64|
| 보조기억장치  |  disk           |  25          |
| 공용/전용     |  public/private |  public      | 
| 요금 청구단위 |  billing        |  monthly     |


~~~ {.input}
root@shiny:~# slcli vs create --hostname=slcli --domain=xwmooc.net --datacenter
=dal09 --cpu 2 --memory 1024 --os UBUNTU_14_64  --disk 25 --public --billing=mo
nthly
This action will incur charges on your account. Continue? [y/N]: y
:.........:......................................:
:    name : value                                :
:.........:......................................:
:      id : 9493553                              :
: created : 2015-05-20T06:23:55-06:00            :
:    guid : 3ea2de49-ef9c-4bc1-b0b9-8e46f84e87d6 :
:.........:......................................:
~~~

## 가상 컴퓨터 확인(Read) 및 로그인

정상적으로 가상컴퓨터가 생성되었는지 확인해보자. `slcli vs list` 명령어를 다시 입력해서 
정상적으로 가상컴퓨터가 `slcli`이름으로 생성된 것을 확인했다.

~~~ {.input}
root@shiny:~# slcli vs list
:.........:...............:.................:................:............:........:
:    id   :    hostname   :    primary_ip   :   backend_ip   : datacenter : action :
:.........:...............:.................:................:............:........:
: 9493553 :     slcli     :   169.55.37.8   :  10.121.149.8  :   dal09    :   -    :
:.........:...............:.................:................:............:........:
~~~

이제 생성한 컴퓨터 내부를 자세히 들어다보자 `root`의 비밀번호도 표시해 보자.

~~~ {.input}
root@shiny:~# slcli vs detail slcli --passwords
:....................:..............................:
:               Name : Value                        :
:....................:..............................:
:                 id : 9493553                      :
:               guid : 3ea2de49-ef9c-4bc1-b0b9-8e46f:
:           hostname : slcli                        :
:             domain : xwmooc.net                   :
:               fqdn : slcli.xwmooc.net             :
:             status : Active                       :
:              state : Running                      :
: active_transaction : -                            :
:         datacenter : dal09                        :
:                 os : Ubuntu                       :
:         os_version : 14.04-64 Minimal for VSI     :
:              cores : 2                            :
:             memory : 1G                           :
:          public_ip : 169.55.37.8                  :
:         private_ip : 10.121.149.8                 :
:       private_only : False                        :
:        private_cpu : False                        :
:            created : 2015-05-20T06:23:55-06:00    :
:           modified : 2015-05-20T06:27:25-06:00    :
:              owner : SL360466                     :
:              vlans : :.........:........:........::
:                    : :   type  : number :   id   ::
:                    : :.........:........:........::
:                    : : PRIVATE :  2143  : 848615 ::
:                    : :  PUBLIC :  1931  : 848613 ::
:                    : :.........:........:........::
:              users : :..........:..........:      :
:                    : : username : password :      :
:                    : :..........:..........:      :
:                    : :   root   : UwJ8kX8u :      :
:                    : :..........:..........:      :
:....................:..............................:
~~~

`ssh root@169.55.37.8` 명령어로 직접 컴퓨터에 로그인하자. 비밀번호는 `slcli vs credentials slcli` 통해 확인한다.

~~~ {.input}
root@shiny:~# slcli vs credentials slcli
:..........:..........:
: username : password :
:..........:..........:
:   root   : UwJ8kX8u :
:..........:..........:
~~~

~~~ {.output}
root@shiny:~# ssh root@169.55.37.8
The authenticity of host '169.55.37.8 (169.55.37.8)' can't be established.
ECDSA key fingerprint is bf:67:59:e2:69:7c:07:95:e3:b6:d4:e8:18:d2:f2:17.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '169.55.37.8' (ECDSA) to the list of known hosts.
Password:
Password:
Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.13.0-51-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

root@slcli:~#
~~~

로그인한 컴퓨터의 사양이 주문한 컴퓨터 사양과 동일한지 확인해보자.
`inxi` 소프트웨어를 설치해서 좀더 확인하기 좋게 하자. 간단히는 우분투에서 지원하는
`sudo lshw` 명령어도 좋다. `sudo apt-get install inxi` 명령어로 설치하고 `inxi -F`로 전체 사양을 살펴보자.

~~~ {.output}
root@slcli:~# sudo apt-get install inxi
root@slcli:~# inxi -F
System:    Host: slcli Kernel: 3.13.0-51-generic x86_64 (64 bit) Console: tty 0 Distro: Ubuntu 14.04 trusty
Machine:   No /sys/class/dmi, using dmidecode: no machine data available
CPU:       Single core Intel Xeon CPU E5-2650 v2 (-HT-) cache: 20480 KB flags: (lm nx sse sse2 sse3 sse4_1 sse4_2 ssse3)
           Clock Speeds: 1: 2600.050 MHz 2: 2600.050 MHz
Graphics:  Card: Failed to Detect Video Card! X-Vendor: N/A driver: N/A tty size: 80x60 Advanced Dat
a: N/A for root out of X
Network:   Card: Failed to Detect Network Card!
Drives:    HDD Total Size: 29.0GB (3.5% used) 1: id: /dev/xvda model: N/A size: 26.8GB 2: id: /dev/xvdb model: N/A size: 2.1GB
Partition: ID: / size: 25G used: 955M (5%) fs: ext3 ID: /boot size: 232M used: 18M (9%) fs: ext3
           ID: swap-1 size: 2.15GB used: 0.00GB (0%) fs: swap
RAID:      No RAID devices detected - /proc/mdstat and md_mod kernel raid module present
Sensors:   None detected - is lm-sensors installed and configured?
Info:      Processes: 76 Uptime: 16 min Memory: 68.5/989.8MB Runlevel: 2 Client: Shell (bash) inxi:1.9.17
~~~

## 가상 컴퓨터 업그레이드(Update)

사용자가 많아지고 가상 컴퓨터가 해야할 일이 많아지면, 크게 두가지 방식으로 부하를 분산해야 한다.
한 방법은 컴퓨터의 사양을 높여서 동일한 컴퓨터에서 더 많은 작업을 처리하게 하는 방식이고, 
다른 한가지 방법은 컴퓨터의 대수를 늘리는 방식이다. 전자를 **스케일업(Scale-Up)**, 후자를 **스케일아웃(Scale-Out)**이라고 한다.

먼저 스케일업방식을 통해서 더 많은 작업을 수행하도록 가상 컴퓨터를 업그레이드하자. 
`slcli vs upgrade --help` 도움말을 통해서 업그레이드 가능한 항목을 볼 수 있다.
하지만, 간단한 파이썬 프로그램을 통해서 스케일업(Scale-Up)과 스케일다운(Scale-Down)을 실행할 수도 있다.

~~~ {.python}
# slcli.py
import SoftLayer
client = SoftLayer.create_client_from_env()
mgr = SoftLayer.VSManager(client)
mgr.upgrade(9493553, cpus=1, memory=2)
~~~

중앙처리장치 갯수는 2개에서 1개로, 주기억장치는 1GB에서 2GB로 변경을 하였다. 
파이썬 프로그램 실행 결과 기존 가상 컴퓨터가 중앙처리장치는 줄고, 주기억장치는 늘어난 것을 확인할 수 있다.

~~~ {.input}
root@shiny:~# python slcli.py
root@shiny:~# slcli vs detail 9493553
~~~

~~~ {.output}
:.............:...............................::.............:...............................:
:        Name : Value                         ::        Name : Value                         :
:.............:...............................::.............:...............................:
:          id : 9493553                       ::          id : 9493553                       :
:        guid : 3ea2de49-ef9c-4bc1-b0b9-8e46f8::        guid : 3ea2de49-ef9c-4bc1-b0b9-8e46f8:
:    hostname : slcli                         ::    hostname : slcli                         :
:      domain : xwmooc.net                    ::      domain : xwmooc.net                    :
:        fqdn : slcli.xwmooc.net              ::        fqdn : slcli.xwmooc.net              :
:      status : Active                        ::      status : Active                        :
:       state : Running                       ::       state : Running                       :
:_transaction : Apply Cloud Instance Upgrades ::_transaction : -                             :
:  datacenter : dal09                         ::  datacenter : dal09                         :
:          os : Ubuntu                        ::          os : Ubuntu                        :
:  os_version : 14.04-64 Minimal for VSI      ::  os_version : 14.04-64 Minimal for VSI      :
:       cores : 2                             ::       cores : 1                             :
:      memory : 1G                            ::      memory : 2G                            :
:   public_ip : 169.55.37.8                   ::   public_ip : 169.55.37.8                   :
:  private_ip : 10.121.149.8                  ::  private_ip : 10.121.149.8                  :
:private_only : False                         ::private_only : False                         :
: private_cpu : False                         :: private_cpu : False                         :
:     created : 2015-05-20T06:23:55-06:00     ::     created : 2015-05-20T06:23:55-06:00     :
:    modified : 2015-05-20T06:57:15-06:00     ::    modified : 2015-05-20T08:45:24-06:00     :
:       owner : SL360466                      ::       owner : SL360466                      :
:       vlans : :.........:........:........: ::       vlans : :.........:........:........: :
:             : :   type  : number :   id   : ::             : :   type  : number :   id   : :
:             : :.........:........:........: ::             : :.........:........:........: :
:             : : PRIVATE :  2143  : 848615 : ::             : : PRIVATE :  2143  : 848615 : :
:             : :  PUBLIC :  1931  : 848613 : ::             : :  PUBLIC :  1931  : 848613 : :
:             : :.........:........:........: ::             : :.........:........:........: :
:.............:...............................::.............:...............................:
~~~


## 가상 컴퓨터 삭제 (Delete)

`slcli vs cancel slcli`와 같이 호스트명 혹은 `id`를 적어두고 GitHub 저장소 삭제하는 것과 마찬가지로 
삭제를 확정하도록 `id`를 한번더 타이핑한다. 그러명 `slcli vs list`에서 삭제되는 것을 확인할 수 있다.

~~~ {.input}
root@shiny:~# slcli vs cancel slcli
This action cannot be undone! Type "9493553" or press Enter to abort: 9493553

root@shiny:~# slcli vs list
:.........:...............:.................:................:............:.........................
:    id   :    hostname   :    primary_ip   :   backend_ip   : datacenter :      action            :
:.........:...............:.................:................:............:........................:
: 9493553 :     slcli     :   169.55.37.8   :  10.121.149.8  :   dal09    : This is a buffer time in
 which the customer may cancel the server :
:.........:...............:.................:................:............:........................:
~~~
