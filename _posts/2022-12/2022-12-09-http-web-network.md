---
layout: post
title:  모든 개발자를 위한 HTTP 웹 기본 지식 1. 네트워크, URI
date:   2022-12-09 03:08:12 +0900
comments : true
categories: Review
tags: [web, http]
---

인프런 강의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC) 수강내용 정리

<br>

### 인터넷 네트워크

컴퓨터가 멀리 떨어져 있는 다른 컴퓨터와 서로 통신하기 위해 컴퓨터 사이를 인터넷 망으로 연결한다.

인터넷 망은 수많은 노드들로 이루어져 있어 보내는 컴퓨터에서 보낸 정보를 받는 컴퓨터로 전달한다.

택배를 보내면 수많은 HUB 를 거쳐 배송지로 배송되는 것과 유사하다.

<br>

#### IP(인터넷 프로토콜)

인터넷은 복잡하게 연결되어 있어 인터넷으로 통신을 하기 위해서는 일정한 규칙이 필요하다.

먼저 모두가 아는 주소를 정하고 규격에 맞게 발신지와 수신지를 기입한다.

편지를 부칠 때 편지봉투에 발수신지를 위치에 맞게 쓰는 것과 유사하다.

이 편지에서 주소에 해당하는 것이 IP 주소이고 보내지는 편지를 패킷이라고 한다.

패킷이라는 단어 자체가 패키지(package)와 버킷(bucket)의 합성어이다.

클라이언트가 이 패킷에 발신 IP 주소와 수신 IP 주소를 기입해서 전송하면 수많은 노드를 지나 수신받는 서버에 도착한다.

데이터를 전달받은 서버에서 클라이언트로 전달되는 과정도 동일하다.

<br>

그러나 IP 프로토콜은 한계점이 있다.

1. 패킷을 받을 대상의 상태를 모르고, 상태에 관계없이 일방적으로 전송한다.

2. 수많은 노드를 거치다 패킷이 유실되어도 알 수 없다.

3. 여러 데이터를 전송하면 제각각 수많은 노드를 거치기 때문에 순차적으로 수신되지 않을 수 있다.

4. 서버에서 같은 IP를 사용하는 프로그램을 여러개라면 구분할 수 없다.

이 한계점을 해결하고자 TCP, UDP 가 나왔다.

<br>

### TCP

TCP 는 클라이언트가 패킷을 전송할때 IP 정보 뿐만이 아닌 TCP 정보를 추가해서 송신하는 방법으로 IP 프로토콜의 문제점을 보완해 주는 '전송을 제어하는 프로토콜'이다.

추가되는 정보로는 발신 PORT, 수신 PORT, 전송 제어, 순서, 검증 정보 등이 있다.

TCP 프로토콜을 통하면 아래와 같은 기능이 가능하다.

- 데이터를 전송하기 전 접속 요청과 요청 수락을 서로 전달하면서 접속 가능 여부를 확인한 후 데이터를 전송한다.

- 데이터를 전송한 후 데이터를 받았는지 수신 확인한다.

- 순서대로 수신하지 않으면 서버에서 클라이언트에 문제가 있는 순서부터 재송신하도록 요청한다.

<br>

#### UDP

TCP 와 같은 계층에 있는 프로토콜이 있으나 TCP 프로토콜과 같이 많은 정보를 포함하지 않고 PORT 정보 정도만 추가된다.

그 외에는 가능한 기능이 많지 않은데 때문에 필요에 따라 어플리케이션에서 추가 작업을 진행한다.

<br>

#### PORT

PORT 정보는 같은 IP 내에서 송수신 받는 어플리케이션을 구분하는 역할을 한다.

패킷을 전송할 때 IP 정보만 있다면 컴퓨터의 주소는 알 수 있지만 어떤 어플리케이션에서 사용하는 지 알 수 없으므로 PORT 정보를 추가한다.

IP 가 아파트 이름이라고 한다면 PORT 는 동호수 정보라고 비유할 수 있다.

<br>

#### DNS

DNS 는 도메인 네임 시스템의 약자로 DNS 서버에 IP 주소와 쉽게 기억할 수 있는 도메인 이름을 가지고 있다가 도메인 이름을 입력하면 IP 주소를 반환하는 전화번호부의 역할을 한다.

IP 는 기억하기 어렵고 변경될 수 있으므로 사용된다.

<br><br>
<hr>
<br><br>

### URI

- Uniform : 통일된 방식

- Resource : 식별할 수 있는 모든 자원

- Identifier : 구분시 필요한 정보

리소스를 식별하는 통합된 방법으로, 구분하는 방법에 따라 URL(Locator) 과 URN(Name) 으로 분류된다.

<br>

#### URN

리소스 자체에 이름을 부여한다.

변하지 않는 값으로 보편화되기 어렵다.

- 예 : 어떤 책의 isbn URN 'urn:isbn:8960777331'

<br>


#### URL

리소스가 있는 위치를 지정한다.

[scheme]://[userinfo@]host[:port][/path][?query][#fragment]

- 예 : 인프런내 강의 접속 'https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/unit/61357?tab=curriculum'

<br>

- **scheme**    
자원 접근 방식을 규정한 프로토콜으로 http, https(http 에 보안 적용), ftp 등이 있다.    

- **userinfo**    
url에 사용자 정보를 포함해야 할 때 사용하지만 거의 사용되지 않는다.    

- **host**    
도메인명 또는 IP 주소를 입력한다.

- **port**    
접속 포트로 http는 80포트, https 는 443포트를 주로 사용하며 포트는 생략이 가능하다.    

- **path**    
리소스가 위치한 경로로 계층적인 구조를 가지고 있다.    

- **query**    
key=value 의 형태로 지정해서 웹서버에 문자형태의 파라미터를 제공한다.    
? 로 시작해서 & 로 여러개의 정보를 추가한다.    
웹 서버에 제공하는 파라미터 정보이기 때문에 query parameter 라고도 하며, 숫자를 입력해도 문자 형태로 전달되기 때문에 query string 이라고도 한다.    

- **fragment**    
서버로 전송되는 정보는 아니며 html 문서 내 북마크 등에 사용한다.    

<br>

### 요청 흐름

웹 브라우저에 URI 를 입력했을 때 브라우저에서의 요청흐름은 아래와 같다.

먼저 요청하기 전 host 정보로 DNS 서버에서 정보를 조회하고 scheme 에 따라 생략된 port 정보를 찾는다.

그 후 아래와 같이 메세지를 전송한다.

1. http 요청 메세지를 생성한다.

> GET [/쿼리정보] [HTTP버전정보]
> Host: [host정보]

2. SOCKET 라이브러리를 통해 서버와 가상으로 연결해 접속 가능 여부를 확인한다.

3. HTTP 정보를 포함한 TCP/IP 패킷을 생성해 전달한다.

4. 서버에서 메세지를 수신해 정보를 해석한 후 데이터를 찾아서 클라이언트로 응답 메세지를 전송한다.

> HTTP/1.1 200 OK
> Content-Type: text/html;charset=UTF-8
> Content-Length: [데이터길이]
>
> [HTML문서데이터]

5. 클라이언트의 브라우저에서 수신받은 HTML 문서 데이터를 렌더링한다.

<br>
