---
layout: post
title:  학원 수업 내용 Day9
date:   2022-09-29 22:47:23 +0900
comments : true
categories: Note
tags: [decodelab, javascript]
---


### String

#### 인덱스 관련

```javascript
let txt = 'Hello World World';

result = txt.indexOf('Wo'); // 지정한 문자열이 시작되는 인덱스
result = txt.indexOf('Wo', 8);  // 지정한 인덱스부터 검색
result = txt.lastIndexOf('Wo');  // 마지막 인덱스부터 (뒤부터) 검색

let txt2 = 'banana, apple, melon';

let result2 = txt2.slice(8, 13);  // 9부터 13인덱스까지 반환
result2 = txt2.substring(8, 13);  // 9부터 13인덱스까지 반환
result2 = txt2.substr(8, 5);  // 9부터 5개 문자 반환
```

<br>

#### 문자열 변경

```javascript
result2 = txt2.replace('World', 'Everyone');
// 처음 만나는 'World' => 'Everyone'으로 대체해줌

result2 = txt2.toUpperCase(); // 대문자로 변경
result2 = txt2.toLowerCase(); // 소문자로 변경
```

<br>

#### 문자열 병합

```javascript
let txt3 = '안녕하세요';
let userName = 'YUJOO';
let result3 = txt3.concat('! ', userName, '님');
```

여러개의 문자열 병합 = 결과물 : 문자열

<br>

#### 문자열 분리

```javascript
let fruits = 'banana,apple,melon';
let result4 = fruits.split(',');
```

','을 기준으로 분리 = 결과물 : 배열

<br><br>
<hr>
<br><br>

### 콜백함수

함수에 매개변수로 들어가는 함수

함수의 매개변수 자리에 함수를 넣은 것 (단점 : 코드가 길어지고 가독성이 떨어짐)

- addEventListener 또한 파라미터를 받는 콜백함수이다.    
ex) 버튼을 클릭했을 때, 반드시 그 다음순서로 콜백함수 순차적으로 실행하라는 뜻

#### Promise, async/await

순차적인 실행, 비동기 방식 처리

자바스크립트는 호이스팅으로 모든 함수를 위로 끌어올리기 때문에 순차적이지 않음

#### setTimeout()

setTimeout(함수, 시간, 함수로 넘길 파라미터)

함수를 지정한 시간 이후에 실행

<br><br>
<hr>
<br><br>

### Style 관련 

스타일 값을 가지고 오는 방법

1. getComputedStyle() 메서드    
외부파일의 스타일 값

2. Element.style 속성    
HTML 자체 지정된 스타일

<br><br>
<hr>
<br><br>

### [슬라이드 예제](/decodelab/220929/slide_basic/)

<br>

<iframe src='/decodelab/220929/slide_basic/' frameborder='0' width='100%' height='500px'></iframe>

<br>

**함수 패키징**    
- 이벤트 함수 분리    
- 자주 사용하고 의존성이 있는 코드끼리 묶기

<br><br>
<hr>
<br><br>


### [탭 메뉴](/decodelab/220929/tab_menu/)

<br>

<iframe src='/decodelab/220929/tab_menu/' frameborder='0' width='100%' height='500px'></iframe>

<br><br>
<hr>
<br><br>

### [탭 메뉴 (웹접근성)](/decodelab/220929/tab_menu2/)

<br>

<iframe src='/decodelab/220929/tab_menu2/' frameborder='0' width='100%' height='500px'></iframe>

<br>

**`addEventListener('focusin', 함수)`** 해당 요소에 포커스(tab)되면 실행

<br>
