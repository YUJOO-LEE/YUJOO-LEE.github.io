---
layout: post
title:  모던 자바스크립트 Deep Dive 스터디 인프런 10, 11
date:   2022-10-12 23:24:02 +0900
comments : true
categories: Review
tags: [inflearn, javascript]
---

[인프런 - 모던 자바스크립트 딥다이브 스터디](https://www.inflearn.com/course/%EB%AA%A8%EB%8D%98-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%94%A5%EB%8B%A4%EC%9D%B4%EB%B8%8C) 보면서 몰랐던, 헷갈렸던, 확실하게는 알지 못했던 것만 정리해두기

<br><br>
<hr>
<br><br>

### 10. 객체 리터럴

함수는 일급객체이다. 

객체 리터럴은 객체를 생성하기 위한 표기법으로, 중괄호 내에 0개 이상의 프로퍼티를 정의한다.

```javascript
const a = {
  b: 'b',
  c: function() {
    console.log(`this is ${this.b}`);
  }
}
```

여기서 a는 객체, b는 프로퍼티, c는 메서드이다.

`a.c()` 호출 시 콘솔창에는 `this is b` 가 출력된다.

호출 시 a의 메서드로서 호출했기 때문이다.

이 때, 함수로 호출하고 싶다면 `a.c.call()` 이라고 하면 `this` 가 지정되지 않은 채로 함수로 호출이 된 것이기 때문에 `this` 는 `window` 가 되고, 전역에 b가 없기 때문에 콘솔창에는 `this is ` 만 출력된다.

- 객체 리터럴의 중괄호는 코드 블록을 의미하지 않는다.

<br>


#### 프로퍼티

```javascript
const a = {
  name: 'b',
  name: 'c'
};
```

동일한 프로퍼티를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓴다.

<br>

```javascript
const a = {
  0: 1,
  1: 2
};
```

키를 숫자로 사용하면 따옴표는 붙지 않아도 내부적으로는 문자열으로 취급한다.

`a[0]`, `a['0']` 로 호출 가능하다.

<br>

#### 메서드

```javascript
const a = {
  b: 'b',
  c: function () {
    return this.b;
  }
};
```

프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해서 메서드라고 한다.

다만 이는 `a.c()` 와 같이 객체의 내부 함수로 호줄할 때 메서드라고 하며, 콜백함수로 사용 등 그 외의 경우에는 메서드라고 할 수 없다.

<br>

#### 프로퍼티 축약 표현

```javascript
let x = 1, y = 2;

const obj = { x, y }  // => obj = { x: x, y: y }

console.log(obj); // { x = 1,  y = 2}
```

프로퍼티 키와 값이 같을 때 생략 가능하다.

<br>

#### 계산된 프로퍼티 이름

```javascript
let i = 0;

const obj = {
  [`key${++i}`]: i,
  [`key${++i}`]: i,
  [`key${++i}`]: i
}

// obj = {key1: 1, key2: 2, key3: 3}
```

객체 리터럴 내부에서 프로퍼티 키를 동적으로 생성할 수 있다.

<br>

#### 메서드 축약 표현

```javascript
const obj = {
  b: 'b',
  c() {
    console.log(`Hello ${this.b}`);
  }
}
```

`c: function() {}` 의 콜론과 function을 생략할 수 있다.

다만 기존의 프로퍼티에 할당한 함수와 다르게 동작한다.

`c()` 로 직접 입력한 함수는 prototype을 가지지 않는다.

따라서 `obj.c()` 는 생성자 함수(new)로 사용할 수 없다.

메서드로만 사용할 수 있어서 메서드로서의 목적성만을 가지고 사용할 수 있다.

<br>

### 11. 원시 값과 객체의 비교

이 부분은 코어자바스크립트에서 신나게 공부했던 부분이라 개인적으로는 크게 새로운 부분이 없었다.

<br>

#### 유사 배열 객체

배열처럼 인덱스를 사용할 수 있고 `length` 프로퍼티를 가지고 있는 객체이다.

for 문으로 순회또한 할 수 있다.

원시 값을 객체처럼 사용하면 원시 값을 감싸는 래퍼 객체로 자동 변환된다.

<br>

