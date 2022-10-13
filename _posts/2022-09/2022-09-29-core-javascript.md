---
layout: post
title:  코어 자바스크립트 인프런 4. callback function
date:   2022-09-29 23:31:53 +0900
comments : true
categories: Review
tags: [inflearn, javascript]
---


[인프런 강의](https://www.inflearn.com/course/%ED%95%B5%EC%8B%AC%EA%B0%9C%EB%85%90-javascript-flow) 보면서 정리해두기.

3. this는 지난번에 한번에 듣고 2. 실행 컨텍스트에 같이 정리해둬서 3번 없이 4번으로 정리

<br>

### callback function

다른 함수의 인자로 함수를 전달해서 넘기는 대상에게 해당 함수에 대한 제어권을 위임한다.

특별한 요청(bind)가 없으면 미리 정해놓은 방식에 따라 콜백 함수가 호출된다.

*넘기게 되는 제어권 : **실행 시점**, **매개 변수**, **this***    

<br>

콜백함수는 메서드가 아님

```javascript
객체.함수();
```

위의 경우 함수는 메서드로 '호출'되어 실행됨 : **this = 객체**

<br>

```javascript
배열.forEach(객체.함수);
// 함수자체를 '전달' = 호출은 forEach가 함
```

위의 경우 함수는 인자로 '전달'되어 forEach의 방식으로 호출됨 : **this = window**

<br>

#### 실행 시점

```javascript
let 함수 = function() {
  실행 내용;
}

setInterval(함수, 1000);
```

함수를 setInterval에 넘겨서 알아서 실행시키도록 함

<br>

#### 매개 변수

```javascript
let 배열 = [1,2,3,4,5];

배열.forEach(함수(배열 요소, 인덱스){
  실행 내용;
}, thisArg(생략 가능));
```

forEach의 규칙에 따라야 제대로 동작할 수 있음

<br>

#### this

```javascript
함수(x){
  console.log(this); // this = 이벤트 타겟 = 엘리먼트
  console.log(x); // 이벤트 객체
}
엘리먼트.addEventListener('이벤트타입', 함수);
```

this값 이벤트 타겟으로 넘어가도록 정의됨

this값 변경하고 싶으면 `함수.bind(this지정)`

<br><br>

