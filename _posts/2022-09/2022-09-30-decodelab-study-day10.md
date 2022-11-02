---
layout: post
title:  data 속성, 무한 루프 슬라이드, 멀티 슬라이드, 스크롤 이벤트
date:   2022-09-30 18:02:23 +0900
comments : true
categories: Note
tags: [decodelab, javascript]
---

학원 수업 내용 Day10

### DOM 구조 변경

문서와 기존 구조를 js를 통해 변경

- 부모 요소명.append(자식요소)    
-- 부모요소 안쪽 가장 뒤에 자식요소 삽입

- 부모요소명.prepend(자식요소)    
-- 부모요소 안쪽의 사장 앞쪽에 자식요소 삽입

<br>

### data-속성

**data-식별자**

- HTML5부터 적용

- 브라우저는 data 속성에 대해 관여하지 않음

- 개발자가 특수하게 사용할 용도

- **css에서 사용** `요소[data-식별자="값"]`

- **js에서 사용** `요소.dataset.식별자 = '값'`

<br><br>
<hr>
<br><br>

### [무한 루프 슬라이드](/decodelab/220930/slider_loop/)

<br>

<iframe src='/decodelab/220930/slider_loop/' frameborder='0' width='100%' height='500px'></iframe>

<br>

#### 기본원리

- 프레임을 기준으로 양쪽에 적어도 한 개 이상의 패널 li가 있어야 함

- 즉, li 적어도 3개 이상 필요

- nth-of-type은 li 순서에 따라 스타일을 적용하므로 loop slider같은 li순서가 바뀌는 경우에는 적용할 수 없음.

1. 초기 ul 위치값 설정

2. 슬라이드 기본 모션    
- prev 버튼 클릭 : ul left: -100% => 0%로 이동    
- next 버튼 클릭 : ul left: -100% => -200%로 이동

3. 이동이 끝난 뒤 쌓인 li를 ul 안이나 뒤로 재배치 필요

4. ul의 초기 위치 left 값 -100%으로 초기화

<br><br>
<hr>
<br><br>

### [멀티 슬라이드](/decodelab/220930/multi_slider/)

<br>

<iframe src='/decodelab/220930/multi_slider/' frameborder='0' width='100%' height='500px'></iframe>

<br>

**동일한 기능을 복수개 이상 사용시 함수로 호출**

- 큰 틀을 파라미터로 넘겨서 지역변수 사용

<br><br>
<hr>
<br><br>

### [스크롤 이벤트 예제](/decodelab/220930/scroll/)

<br>

<iframe src='/decodelab/220930/scroll/' frameborder='0' width='100%' height='500px'></iframe>

<br>

- scrollY, pageYOffset : 문서 수직으로 얼마나 스크롤 되었는지 px 단위로 반환

- scrollY : 익스 9 이전 적용안됨

- offsetTop : document 최상단에서부터 요소 시작점까지의 값

<br>

```javascript
const top배열 = [];
for (let 요소 of 요소배열) top배열.push(요소.offsetTop);

let 현재_세로_스크롤_위치 = window.scrollY || window.pageYOffset;
```

<br>

위와 같이 받아와서 class도 on/off하고 개별 요소에 left값도 줘서 스크롤값 비례해서 움직이기도 했는데, 너무 재밌고 숫자계산 하느라 머리아팠다..

<br><br>

