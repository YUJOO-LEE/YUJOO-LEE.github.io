---
layout: post
title:  모던 자바스크립트 Deep Dive 스터디 인프런 12
date:   2022-10-13 23:17:02 +0900
comments : true
categories: Review
tags: [inflearn, javascript]
---

[인프런 - 모던 자바스크립트 딥다이브 스터디](https://www.inflearn.com/course/%EB%AA%A8%EB%8D%98-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%94%A5%EB%8B%A4%EC%9D%B4%EB%B8%8C) 보면서 몰랐던, 헷갈렸던, 확실하게는 알지 못했던 것만 정리해두기

<br><br>
<hr>
<br><br>

### 12. 함수

함수 선언문은 표현식이 아닌 문이다.

표현식이 아닌 문은 변수에 할당할 수 없다.

```javascript
const func = function () {};
```

위와 같이 변수에 할당되는 것은 함수를 리터럴 표현식으로 해석할 수 있기 때문이다.

함수 선언문과 함수 리터럴은 그 모양이 서로 같은데, 함수 선언문은 이름을 생략할 수 없다는 점만 다르다.

따라서 문맥에 따라 함수 리터럴을 단독으로 사용하면 선언문으로 해석하고, 변수에 사용하거나 피연산자로 사용하는 경우에는 리터럴 표현식으로 해석한다.

<br>

#### 함수 생성 시점과 호이스팅

```javascript
// 함수 참조
console.dir(a); // f a(x)
console.dir(b); // undefined

// 함수 호출
console.log(a(1));  // 1
console.log(b(1));  // Type Error

function a(x) {
  return x;
}

var b = function (y) {
  return y;
}
```

이 경우 호이스팅은 함수 a와 변수 b가 되고, 변수 b에 함수를 할당하는 것은 하단에서 실행된다.

따라서 할당문의 상단에서 함수를 호출 할 경우 호출이 될 수 없다.

<br>

#### function 생성자

```javascript
new Function(파라미터, 실행내용);
```

위와같이 생성자 함수로 함수를 행성할 수 있으나, 일반적이지 않으며 바람직하지도 않다.

생성자로 생성된 함수는 클로저를 생성하지 않는다.

클로저처럼 사용하려고 하면 RefereneError가 뜬다.

<br>

#### 화살표 함수

```javascript
const func = (x) => {
  return x;
}
```

중괄호 안에 `return` 하나만 있다면 중괄호와 `return` 을 생략할 수 있다.

```javascript
const func = (x) => x;
```

파라미터가 하나라면 괄호마저도 생략할 수 있다.

```javascript
const func = x => x;
```

<br>

**화살표 함수와 기존 함수의 차이점**

- 기존 함수에서 동작을 간략화 해서 함수로만 사용할 수 있다.

- prototype을 갖지 않는다.

- arguments 객체를 생성하지 않는다.

- `new func()` 와 같이 생성자 함수로 사용할 수 없다.

- 기존 함수와 this 바인딩 방식이 달라서 외부 this를 사용할 수 있다.

<br>

#### 매개변수와 인수

```javascript
function func(x) {  // x는 매개변수
  return x;
}
func(1, 2); // 1, 2는 인수
```

함수는 실행 시 인수로 넣은 값들을 매개변수로 받아와 스코프 내부에서 선언해서 사용한다.

선언되지 않고 초과된 인수(위 코드에서 2)는 arguments 객체의 프로퍼티로 보관된다.


```javascript
function func(x, ...y) {
  return x;
}
func(1, 2);
```

굳이 arguments를 쓰지 않고 위와 같이 나머지들을 별도로 선언할수도 있다.

<br>

- 자바스크립트 함수는 매개변수와 인수의 개수가 일치하는지 확인하지 않는다.

- 자바스크립트는 동적 타입 언어이다. 따라서 자바스크립트 함수는 매개변수의 타입을 사전에 지정할 수 없다.

- 런타임 에러 발생을 방지하기 위해 함수 내에서 typeof 조건문을 통해 인수의 타입을 제어할 수 있다.

- 하나의 함수는 가급적 작게 만들어서 한가지 일만 해야 한다.

- 매개변수는 최대 3개 이상을 넘지 않는 것이 바람직하다.

<br>

#### 반환문

함수 호출은 표현식이다. 

함수 호출 표현식은 `return` 키워드가 반환한 값으로 평가된다. 

return 뒤의 식을 지정하지 않으면 undefined 가 반환된다.

반환문은 함수의 실행을 중단하고 함수를 빠져나간다. 

따라서 반환문 뒤의 코드는 무시된다.

반환문은 함수 내부에서만 실행되므로 전역에서는 사용할 수 없다.

<br>

#### 참조와 외부상태

인수로 객체를 전달받으면 함수 내부에서 해당 파라미터는 객체를 참조한 것이기 때문에 그대로 사용 시 외부에 있는 원본 객체를 변경시킬 수 있다.

외부 상태를 변경하지 않고 의존하지 않는 함수를 순수 함수라고 한다.

순수 함수를 통해 부수 효과를 최대한 억제해서 오류를 피하고 프로그램의 안정성을 높이려는 패러다임을 **함수형 프로그래밍**이라고 한다.

<br>

#### 즉시 실행 함수

```javascript
(function () {
  console.log('Hello Function!');
}());
```

```javascript
(function () {
  console.log('Hello Function!');
})();
```

```javascript
!function () {
  console.log('Hello Function!');
}();
```

```javascript
+function () {
  console.log('Hello Function!');
}();
```

즉시 실행 함수는 익명이든 기명이든 상관 없지만 기명이어도 해당 함수는 한번 실행하고 사라지기 때문에 다시 실행할 수 없다.

일반적으로는 첫번째와 같이 그룹 연산자로 감싸서 함수를 리터럴로 평가해 객체화 해서 실행시킨다.

함수 리터럴을 평가해서 함수 객체를 생성할 수 있다면 위와 같이 다양한 연산자를 사용해서 즉시 실행되게 할 수 있다.

즉시 실행 함수도 값을 반환하거나 인수를 전달할 수 있다.

<br>

#### 재귀 함수

```javascript
function countdown(n) {
  for (let i = n; i >= 0; i--) console.log(i);
}
```

이 함수는 아래와 같이 재귀 함수로 만들 수 있다.

```javascript
function countdown(n) {
  if (n < 0) return;
  console.log(n);
  countdown(n - 1); // 재귀 호출
}
```

<br>

#### 콜백 함수

함수의 매개 변수를 통해 다른 함수의 내부로 전달되는 함수를 콜백 함수라고 한다.

이때 전달받은 함수는 고차 함수라고 한다.

콜백 함수는 고차 함수에 의해서 호출 시점과 인수를 정해 호출되므로 콜백 함수를 전달할 때에는 함수를 호출하지 않고 함수 자체를 전달해야 한다.

<br>

#### 순수 함수

```javascript
let count = 0;
function increase(n) {
  return ++n;
}
count = increase(count);
```

위의 함수는 **순수 함수**로 외부의 값을 직접적으로 변경하지 않기 때문에 count 는 변화하지 않는다.

따라서 함수를 호출 할 때 마다 count의 변화를 추적할 수 있다.

```javascript
let count = 0;
function increase(n) {
  return ++count;
}

increase();
```

위의 함수는 **비순수 함수**로 외부의 값을 직접적으로 변경하기 때문에 count의 상태를 변화시킨다.

<br>

