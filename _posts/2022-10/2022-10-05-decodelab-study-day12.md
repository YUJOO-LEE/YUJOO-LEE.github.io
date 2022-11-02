---
layout: post
title:  flickr API 활용
date:   2022-10-05 22:35:23 +0900
comments : true
categories: Note
tags: [decodelab, javascript, flickr, api]
---

학원 수업 Day12

### [플리커 API 활용](/decodelab/221005/flickr/)

<br>

<iframe src='/decodelab/221005/flickr/' frameborder='0' width='100%' height='500px'></iframe>

<br>

[플리커 API키](https://www.flickr.com/services/api/)를 받아 플리커 사이트 내 정보를 받아올 수 있다.

리스트 디자인을 위해서 [isotope](https://isotope.metafizzy.co/) 플러그인을 활용했다.

오늘은 단순히 API 정보를 받아오는 것 뿐만 아니라, 실제 서비스 같은 페이지 구현이 주 목표였다.

<br>

1. 로딩 모션 이벤트 `addEventListener('load', func)` 추가

2. load error시 대체 이미지 지정 `addEventListener('error', func)`, `setAttribute('src', 'imgUrl')`

3. 용도, 기능 별 함수 패키징

4. 직접 input창에 검색어를 받아 동적으로 리스트를 생성 (3번에서 패키징 된 함수 재사용)

5. 예외 처리 `string.trim()`, `!items.length`

6. 반응형

<br>


확실히 기존 실습에 비해 결과물이 완성도 있어서 더 재밌다.

<br>
