---
layout: page
title: R 파이썬 소프트레이어 클라우드 그리고 xwMOOC
subtitle: 데브옵스(DevOps, 개발운영)
minutes: 10
---
> ## 학습 목표
>
> *   GitHub SSH 접속한다.


## GitHub SSH 연결 설정

GitHub에 개발 프로그램과 결과물을 패스워드를 입력하지 않고 저장하여 동기화한다.
먼저 SSH 학습에서 원격 컴퓨터 연결에서 사용한 공개열쇠와 개인열쇠를 사용한다.
다시 열쇠상을 생성해서 사용해도 된다. 

[GitHub](https://github.com/)에 계정을 생성하면 우측상단에 설정 아이콘(기어 모양)을 클릭하면
좌측에 `SSH keys`가 보인다. 클릭해서 들어가면 SSH keys를 등록하는 화면이 나온다.
당황하지 말고, `id_rsa.pub` 파일을 텍스트 편집기로 열어 전체 내용을 `CTRL+A` 키를 눌러 선택하고 복사한다.
[GitHub SSH keys](https://github.com/settings/ssh) 메뉴에서 `Add SSH key`를 눌러 복사한 내용을 붙여넣고 추가하여 저장완료한다.

~~~ {.input}
$ ssh -T git@github.com
Warning: Permanently added the RSA host key for IP address '192.30.XXX.XXX' to t
he list of known hosts.
Hi xwMOOC! You've successfully authenticated, but GitHub does not provide shell access.
~~~

마지막으로 쉘 화면에서 `ssh -T git@github.com` 명령어를 입력하면 설정이 완료된다.
정상적인 작동여부를 테스트하기 위해서 `git push` 명령어를 보내면 아이디와 패스워드를 묻지않고 `ssh` 인증으로 
작업을 완료한다.

~~~ {.input}
$ git push origin gh-pages
~~~

