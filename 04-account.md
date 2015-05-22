---
layout: page
title: R 파이썬 소프트레이어 클라우드 그리고 xwMOOC
subtitle: 클라우드 계정관리
minutes: 10
---
> ## 학습 목표
>
> *   클라우드 계정 보안수준을 높인다.
> *   
> *   


## 클라우드 보안 강화

다른 클라우드 서비스도 마찬가지지만, SoftLayer에서는 `이중인증(Two-Factor Authentification)`을 지원한다.
기본적으로 비밀번호만 사용하게 되면 상대적으로 비밀번호가 강력하더라도 보안에 취약할 수가 있다. 따라서, 알고 있는 것과 가지고 있는 것을 동시에 확인해서 보안을 강화할 수 있다. 마치 은행에서 예금을 인출할 때 알고 있는 것은 비밀번호, 갖고 있는 것은 체크카드를 가지고 본인인증을 강화해서 금융사고에 대비하는 것도 동일하다.

SoftLayer에서는 계정정보에서 **Two-Factor Authentication** 메뉴를 클릭하며 `구글 인증(Google Authentication)` 통해 알고 있는 SoftLayer 계정 비밀번호와 함께 스마트폰에 *Google Authenticator app* 설치가 필요하다. 구글 앱스트로어 Play로 들어가서 `Google OTP`를 설치한다. 스마트폰 바탕화면에 `OTP`가 생성된다. 이제 SoftLayer 계정정보에 나타나는 QR 코드를 스캔하고 확인을 누르게, 스마트폰을 이중인증 과정에 등록하면 전체적인 인증등록 과정이 완료된다. 

![클라우드 이중인증 보안 설정](fig/two-factor-auth.png)

만약 `Google Authentification is:`를 활성화(Active) 상태로 두었다면, 이제부터 SoftLayer 로그인 [https://control.softlayer.com/](https://control.softlayer.com/)하게 되면, 비밀번호 외에 `일회용 비밀번호(One-Time Password, OTP)`를 요청한다. 당황하지 말고, 스마트폰을 꺼내 `Google OTP` 앱을 열어 비밀번호를 입력하자. 이제부터 좀더 강화된 클라우드 보안서비스를 마음껏 누려보자.

> ## 이중 인증(Two-factor Authentification) {.callout}
> 
> 2가지 인증 방법을 조합 적용하는 안전성을 향상시키는 인증. 인증은 다음과 같은 3가지 요소로 구분할 수 있다. 
> 바로 자신이 알고 있는 것(what you know : 패스워드, PIN 등), 자신이 소유한 것(what you have : 스마트 카드, 토큰, 키 등), 
> 자신 그 자체(what you are : 지문 등 생체 정보)이다. 이들 중 하나의 요소만 이용하는 단일 인증은 보안에 매우 취약한 편이다. 
> 패스워드 등 자신이 아는 정보만을 사용할 경우 분실 여부를 인지하기 어려우며, 토큰, 키 등 자신이 소유한 것만을 사용할 경우 
> 분실 시 습득자의 즉각적인 사용이 가능하다. 따라서 이러한 단일 인증의 보안 취약성을 보강하기 위하여 이들 중 서로 다른 2개의 
> 인증을 조합하여 채택한 방식이 이중 인증이다. 예를 들어 은행의 현금 자동 인출기(ATM)처럼 카드와 개인 식별 번호(PIN)를 
> 조합하여 사용함으로써 보안성을 높일 수 있다.  
> 출처 : [네이버 지식백과, 이중 인증 (two-factor authentication), IT용어사전, 한국정보통신기술협회](http://terms.naver.com/entry.nhn?docId=859932&cid=42346&categoryId=42346)





## 클라우드 청구비용 조회

클라우드 서비스를 통해서 컴퓨터 자원을 마음대로 사용할 수 있지만, 
종량제(Pay-as-you-go)에 따라 컴퓨터 사용 청구비용이 높아지기 때문에 원하는 시점에 사용량을 확인하자.
REST API를 사용해서 `https://ID:API-KEY@api.softlayer....` 방식에 맞춰 ID와 비밀번호를 입력하고 현재 계정에 부과된 요금을 확인할 수 있다. SLXXXXXX 계정에 10.77 달러가 청구된 것을 확인할 수 있다.

~~~ {.input}
root@shiny:~# curl -s https://SLXXXXXX:7c7xxxxxx@api.softlayer.com/rest/v3/SoftLayer_Account/getBalance.xml
<?xml version="1.0"?>
<root>10.77</root>
~~~


