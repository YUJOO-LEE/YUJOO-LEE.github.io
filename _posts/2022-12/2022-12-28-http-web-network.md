---
layout: post
title:  모든 개발자를 위한 HTTP 웹 기본 지식 6. 캐시와 조건부 요청
date:   2022-12-28 18:16:05 +0900
comments : true
categories: Review
tags: [web, http]
---

인프런 강의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC) 수강내용 정리

<br>

### 캐시

아래와 같이 요청하면 응답 결과를 캐시에 저장한다.

응답 시 서버에서 `max-age` 로 유효 시간(초)을 지정한다.

> 예) cache-control: max-age=60

지정한 시간 내 동일한 파일을 재요청한다면 네트워크를 사용하지 않고 캐시에 저장된 데이터를 사용한다.

캐시 덕분에 일정 시간동안 네트워크를 사용하지 않아도 된다.

만약 캐시가 없으면 데이터가 변경되지 않아도 매번 데이터를 다운로드 받아야 하기 때문에 브라우저 로딩 속도가 느려져 사용자 경험 또한 느릴 것이다.

<br>

한번 다운로드 받은 파일의 캐시 만료 후에 재요청 하게 되면 파일을 처음부터 새로 다운받게 된다.

하지만 캐시 만료 후 데이터 변경이 없다면 다운로드 받을 필요가 없을 것이다.

검증 헤더 추가와 조건부 요청으로 불필요한 다운로드를 하지 않고 저장된 캐시를 재활용 하도록 할 수 있다.

<br>

#### 검증/조건부요청

검증 헤더와 조건부 요청 헤더는 짝꿍으로 캐시 데이터와 서버의 현재 데이터가 동일한지 검증하기 위해 사용한다.

검증 헤더는 서버에서 다운로드 시, 조건부 요청은 클라이언트에서 캐시 만료 데이터 재요청 시 사용한다.

<br>

데이터 최초 다운로드 시 검증 헤더가 있었다면, 캐시 만료 후 클라이언트에서 재요청 할 때 조건부 요청 헤더로 마지막 수정 시간을 넘겨서 수정 여부를 묻는다.

요청받은 서버에서 데이터의 현재 정보와 비교 후 수정되었다면 **200** 으로 응답해 새로운 데이터를 전송하고, 동일한 데이터라면 응답 시 **304(Not Modified)** 로 응답한다.

304 응답에는 헤더 메타 정보만 있고, 바디는 없이 전송하기 때문에 최소한의 용량으로 전송한다.

해당 응답을 받는다면 클라이언트에서 이미 만료되었던 해당 데이터를 다시 캐시로 세팅해서 사용한다. 

<br>

- Last-Modified (검증 헤더)    
- If-modified-since (조건부 요청) 

마지막 수정 날짜 및 시간으로 수정 여부를 확인한다.

1초 미만 단위로 캐시 조정이 불가하다.

날짜 기반의 로직 사용으로, 실제 데이터 컨텐츠가 바뀌지 않았어도 수정 날짜만 바뀌면 새로 다운받는다.

<br>

- ETag (검증 헤더)    
- If-None-Match (조건부 요청)     

캐시용 데이터에 임의의 고유한 버전 이름을 지정한다.

데이터가 변경되어 새로 다운받아야 한다면 이 이름을 변경한다.

If-None-Match 로 요청 해 이름이 다르면(수정되었다면) 새로 다운받고, 이름이 같으면 304 응답으로 캐시 데이터를 유지한다.

캐시 제어 로직은 서버에서 관리되고 클라이언트에서는 단순히 해당 값을 서버에 제공하기만 한다.

<br>

#### Cache-Control

- max-age    
캐시 유효 시간을 초 단위로 지정한다.

- no-cache    
데이터는 캐시해도 되지만, 유효 시간에 상관 없이 항상 원(origin) 서버에 검증하고 사용하도록 한다.    
원 서버에 접근이 불가하면 서버 설정에 따라서 프록시 캐시 서버의 데이터를 반환할 수도 있다.

- no-store    
데이터에 민감한 정보가 있으므로 저장하지 않도록 한다.    
클라이언트는 해당 데이터를 메모리에서 사용하고 최대한 빨리 삭제한다.

- must-revalidate    
캐시 유효 시간 내라면 캐시를 사용하고, 캐시 만료 후 최초 조회시에 원 서버에 검증하도록 한다.    
원 서버에 접근이 불가하면 504(Gateway Timeout) 오류를 발생시킨다.

- public    
프록시 캐시 관련 헤더    
응답이 public 캐시에 저장되는 것을 허용한다.

- private    
클라이언트 개인용 정보이므로 public 에 저장하지 않고 private 에 저장하도록 한다.    
별도로 지정하지 않으면 private 가 기본값으로 지정된다.    

- s-maxage    
프록시 캐시에 max-age 를 지정한다.

<br>

#### Pragma

- no-cache    
HTTP 1.0 하위 버전에서 `Cache-Control: no-cache` 와 동일한 기능 호환을 위해 사용한다.

<br>

#### Expires

캐시 만료일을 정확한 날짜로 지정한다.

HTTP 1.0 부터 사용 가능하다.

`Cache-Control: max-age` 사용을 권장하며, `Cache-Control: max-age` 와 함께 사용하면 `Expires` 는 무시된다.

<br>

### 프록시 캐시

- 원 서버 : 실제 자원이 있는 서버

- 프록시 캐시 서버 : 캐시 데이터를 저장해 둔 서버

원 서버가 해외에 있어서 직접 연결하면 응답 속도가 느리므로 클라이언트 근처의 프록시 서버로 연결해서 요청하도록 한다.

프록시 캐시 서버에 있는 캐시 데이터를 public 캐시라고 하고, 클라이언트 로컬에 저장된 캐시를 private 데이터라고 한다.

<br>

#### Age

- 예) Age: 60

원 서버에서 응답 후 캐시 내에 머문 시간을 초단위로 나타낸다.

<br>

### 캐시 무효화

기본적으로 웹 브라우저에서 자동으로 데이터를 캐시한다.

확실하게 캐시를 하지 않으려면 아래와 같이 응답해야 한다.

> Cache-Control: no-cache, no-store, must-revalidate
> Pragma: no-cache *(HTTP 1.0 하위 호환)*

<br>
