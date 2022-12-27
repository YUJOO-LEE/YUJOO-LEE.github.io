---
layout: post
title:  모든 개발자를 위한 HTTP 웹 기본 지식 5. HTTP 헤더
date:   2022-12-27 22:13:03 +0900
comments : true
categories: Review
tags: [web, http]
---

인프런 강의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC) 수강내용 정리

<br>

### HTTP 헤더

- **구조**    
`field-name : field-value`    
-- field-name : 대소문자 구분 없음    
-- field-value : 대소문자 구분 있음

- **용도**    
HTTP 전송에 필요한 모든 부가 정보를 포함한다.    
-- 예) body 내용의 타입, 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보 등    
-- 표준 헤더 : [https://en.wikipedia.org/wiki/List_of_HTTP_header_fields](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)    
필요 시 임의의 헤더 추가가 가능하다.

<br>

#### HTTP 표준

- **과거(RFC2616) 헤더 분류**    
-- General : 메세지 전체에 적용되는 정보.    
-- 예) Connection: close    
-- Request : 요청 정보.    
-- 예) Use-Agent: Mozilla/5.0 (Macintosh; ..)      
-- Response : 응답 정보.    
-- 예) Server: Apache    
-- Entity : 엔티티 바디 정보.    
-- 예) Content-Type: text/html, Content-Length: 3423

- **과거(RFC2616) 메세지 바디**    
-- 메세지 바디는 엔티티 바디를 전달하는 데 사용했다.    
-- 엔티티 바디는 요청이나 응답에서 전달할 실제 데이터를 의미한다.    
-- 엔티티 헤더는 엔티티 바디의  데이터를 해석할 수 있는 정보를 제공한다. (데이터 유형이나 길이, 압축 정보 등)

- **RFC723X**    
-- 1999년 'RFC2616'이 폐기되고 2014년 RFC7230~7235로 스펙이 분리되며 개정된다.
-- 엔티티 -> 표현(Representation)으로 변경되었다.    
-- 표현(Representation)은 표현 메타 데이터(Representation Metadata)와 표현 데이터(Representation data)가 합쳐진 말이다.

- **최신(RFC7230) 메세지 바디**    
-- 메세지 바디를 통해 표현 데이터를 전달한다.    
-- 표현은 요청이나 응답에서 전달할 실제 데이터를 의미한다.    
-- 표현 헤더는 표현 데이터를 해석할 수 있는 정보를 제공한다. (데이터 유형이나 길이, 압축 정보 등)

<br>

#### 표현 헤더

표현 헤더는 전송, 응답 모두 사용한다.

- **Content-Type** : 표현 데이터의 형식(JSON, HTML 등)    
-- 미디어 타입이나 문자 인코딩 정보    
-- 예1) text/html; charset=utf-8    
-- 예2) application/json    
-- 예3) image/png

- **Content-Encoding** : 표현 데이터의 압축 방식        
-- 표현 데이터를 압축하기 위해 사용한다.    
-- 데이터를 전달하는 쪽에서 압축 후 인코딩 헤더 정보를 보내면 전달받는 쪽에서 헤더 정보로 압축을 해제한다.    
-- 예1) gzip    
-- 예2) deflate    
-- 예3) identity(압축 안한다는 뜻)

- **Content-Language** : 표현 데이터의 자연 언어(한국어, 영어 등)    
-- 다국어를 지원할 때 언어 선택 시 활용한다.    
-- 예1) ko    
-- 예2) en    
-- 예3) en-US

- **Content-Length** : 표현 데이터의 길이 (명확하게 분류하자면 payload 헤더)    
-- 바이트 단위로 표현한다.    
-- 전송 코딩(Transfer-Encoding)을 사용하면 `Content-Length` 사용이 불가하다.    

<br>

#### 협상(컨텐츠 네고시에이션) 헤더

클라이언트가 선호하는 표현을 요청할때 사용한다.

- **Accept** : 클라이언트가 선호하는 미디어 타입 전달    
-- 기본적으로 구체적인 것이 우선순위를 갖는다.     
-- 예) text/\*,text/plain,text/plain;format=flowed,\*/*    
-- 예시에서 우선순위는 1. text/plain;format=flowed // 2. text/plain // 3.text/* // 4. \*/* 이다.
-- **Qualith Values(q)** 로 직접 우선 순위를 정해 보낼 수도 있다.    
-- q는 0~1 범위의 숫자로 1에 가까울 수록 높은 우선순위를 가지며, 생략하면 1이다.    

- **Accept-Charset** : 클라이언트가 선호하는 문자 인코딩    

- **Accept-Encoding** : 클라이언트가 선호하는 압촉 인코딩    

- **Accept-Language** : 클라이언트가 선호하는 자연 언어     
-- 예) 한국어 브라우저를 사용 시 헤더에 `Accept-Language: ko` 를 보낸다면 서버에서 가능하다면 `Content-Laguage: ko` 로 응답한다.
-- **Qualith Values(q)** 로 우선 순위를 정해 보낼 수도 있다.    
-- q는 0~1 범위의 숫자로 1에 가까울 수록 높은 우선순위를 가지며, 생략하면 1이다.    
-- 예) ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7

<br>

#### 전송 방식

- **단순 전송**    
`Content-Length` 를 주어 한번에 전송한다.

- **압축 전송**    
데이터를 압축해서 전송한다.    
`Content-Encoding` 를 추가 해 함께 보내야 전달받는 쪽에서 해당 값에 맞춰서 압축 해제를 할 수 있다.

- **분할 전송**    
`Transfer-Encoding: chunked` 헤더와 함께 데이터를 쪼개서 전송한다.    
데이터를 분할해서 분할된 바이트 길이와 데이터를 함께 보낸다.    
마지막에는 `0` `\r\n` 으로 종료를 알린다.    
분할 전송 시 `Content-Langth` 를 보내지 않는다.

- **범위 전송**    
일부 범위만 전달받기 위해 클라이언트에서 Range 헤더로 범위값을 보낸다.    
Range 를 전달받은 서버에서 Content-Range 헤더와 함께 해당 범위의 데이터를 전송한다.    
클라이언트 예) Range: bytes=1001-2000    
서버 예) Content-Range: bytes 1001-2000 / 2000

<br>

#### 일반 정보

단순 정보성 헤더

- From    
-- 요청 시 유저 에이전트의 이메일 정보를 담는다.    
-- 일반적으로 잘 사용되지는 않고, 검색 엔진 등에서 주로 사용한다.    

- Referer    
-- 요청 시 현재 요청의 이전 웹 페이지 주소를 담는다.    
-- 유입 경로 분석이 가능하다.    
-- referrer 의 오타이나 임의로 변경 시 서버에서 식별이 어려워져 그대로 사용한다고 한다...

- User-Agent     
-- 요청 시 클라이언트 애플리케이션의 정보(웹 브라우저 등)를 담는다.    
-- 예) Mozilla/5.0 (Macintosh; Intel Mac Os X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36    
-- 통계 정보로 사용하며, 어떤 종류의 브라우저에서 장애가 발생하는지 파악이 가능하다.    

- Server    
-- 응답 시 요청을 실제로 처리하는 ORIGIN 서버의 소프트웨어 정보를 담는다.    
-- 예1) Apach/2.2.22 (Debian)    
-- 예2) nginx

- Date    
-- 응답 시 해당 메세지가 발생한 날짜와 시간을 담는다.    
-- 예) Tue, 15 Nov 1994 08:12:31 GMT

<br>

#### 특별 정보

- Host    
-- 요청 시 요청한 호스트 도메인 정보를 담는 필수 헤더이다.    
-- 하나의 IP 주소에 여러 도메인이 적용되어 있을 때 구분하기 위한 정보이다.     

- Location    
-- 3XX 응답 시 해당 헤더가 있으면 해당 값의 위치로 자동으로 이동한다.    
-- 201 응답 시 해당 값은 요청에 의해 생성된 리소스의 URI 이다.

- Allow    
-- 허용 가능한 HTTP 메서드를 담는다.    
-- 405(Method Not Allowed) 로 응답 시 해당 헤더를 포함해 어떤 메서드가 사용 가능한지 클라이언트에 알린다.    
-- 예) GET, HEAD, PUT

- Retry-After    
-- 503(Service Unavailable) 로 응답 시 서비스가 언제까지 불능인지 알려줄 수 있다.     
-- 예: 날짜 표기) Fri, 31 Dec 1999 23:59:59 GMT     
-- 예: 초단위 표기) 120

<br>

#### 인증

- Authorization    
-- 클라이언트 인증 정보를 서버에 전달한다.

- WWW-Authenticate    
-- 401(Unauthorized) 로 응답 시 해당 헤더값으로 리소스 접근에 필요한 인증 방법을 정의해 클라이언트에 알린다.    

<br>

#### 쿠키 헤더

- Set-Cookie    
-- 서버에서 응답 시 클라이언트로 쿠키를 전달할 때 사용한다.

- Cookie   
-- 클라이언트가 서버에서 받은 쿠키를 저장하고 HTTP 요청 시 서버에 전달할 때 사용한다.

<br>

### 쿠키란?

쿠키는 주로 사용자 로그인 세션을 관리할 때 사용한다.    
로그인이 성공후 데이터를 직접적으로 쿠키로 전달하면 보안에 취약하므로 세션에 저장한 후 해당 세션의 ID 를 쿠키에 저장하도록 한다.

또는 광고 정보를 트래킹 할 때 사용하기도 한다.

쿠키 정보는 브라우저에 저장되며, 클라이언트에서 요청 시 매번 자동으로 헤더에 포함해 전달된다.    
따라서 네트워크 트래픽을 추가로 유발하기 때문에 최소한의 정보만 사용하는 것이 좋다.    
만약 서버에 전송하지 않고 웹 브라우저에 저장만 해놓고 싶다면 웹 스토리지(localStorage, sessionStorage)를 사용하는 것이 좋다.

서버에서 `set-cookie` 를 보낼 때 예시는 아래와 같다.

```
set-cookie: sessionId=abdce1234; expires=Sat, 26-Dec-2020 00:00:00 GMT; path=/; domain=.google.com; Secure
```

<br>

#### expires, max-age

쿠키는 `set-cookie` 시 설정된 `expires` 값에 따라 만료일이 지정된다.     
만료일이 되면 해당 쿠키는 삭제된다. (영속 쿠키)

또는 `max-age` 로 초(sec)를 지정해 만료 시간을 정할 수도 있다.    
해당 값에 0 또는 음수를 지정해서 저장하면 해당 쿠키가 삭제된다.

- 세션 쿠키 : 영속 쿠키에 상반되는 분류로, 만료 날짜를 생략하면 브라우저 종료 까지만 유지된 후 삭제된다.

<br>

#### domain

해당 쿠키를 전송할 도메인을 지정한다.

도메인을 명시하면 명시한 문서 기준한 도메인과 서브 도메인 모두에 적용한다.    
예) `domain=example.org` 를 지정하면 example.org 과 dev.example.org 모두에서 쿠키 접근이 가능하다.

도메인을 생략하면 현재 문서 기준한 도메인만 적용한다.    
예) example.org 에서 쿠키를 생성하고 domain 지정을 하지 않으면 example.org 에서만 접근이 가능하고 dev.example.org 에서는 접근이 불가하다.

<br>

#### path

해당 경로를 포함한 하위 경로 페이지에서만 해당 쿠키 접근이 가능하도록 한다.    

일반적으로 `path=/` 루트로 지정해 전체 사이트에서 사용하도록 한다.

예) path=/home 으로 지정하면 /home 과 /home/level1 그리고 /home/level1/level2 모두에서 접근이 가능하고, /hello 경로에서는 접근이 불가하다.

<br>

#### 보안 관련

- Secure    
-- 기본적으로 쿠키는 http, https 를 구분하지 않고 전송한다.    
-- 하지만 Secure 를 적용하면 https 인 경우에만 전송한다.

- HttpOnly    
-- XSS 공격을 방지한다.    
-- 자바스크립트에서 `document.cookie` 접근이 불가하다.    
-- HTTP 전송에만 사용 가능하다.

- SameSite    
-- XSRF 공격을 방지한다.    
-- 요청 도메인과 쿠키에 설정된 도메인이 같은 경우에만 쿠키를 전송한다.

<br>

