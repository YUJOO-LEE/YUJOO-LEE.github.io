---
layout: post
title:  코어 자바스크립트 인프런 6. prototype
date:   2022-10-03 16:28:53 +0900
comments : true
categories: Review
tags: [inflearn, javascript]
---


[인프런 강의](https://www.inflearn.com/course/%ED%95%B5%EC%8B%AC%EA%B0%9C%EB%85%90-javascript-flow) 보면서 정리해두기.

<br>

### prototype

생성자 함수가 있을 때 new 연산자로 instance를 만들면, 그 instance 안에는 constructor의 'prototype'이라고 하는 프로퍼티의 내용이 [[Prototype]]이라고 하는 프로퍼티로 참조를 전달한다.

즉, constructor의 prototype과 instance의 [[Prototype]]는 같은 객체를 바라본다.

이때 [[Prototype]]은 접근 가능한 것이 아니라 정보를 보여주기만해서, 실제 동작상으로는 instance와 동일시 된다.

null, undefined를 제외한 모든 데이터는 기본형, 참조형에 상관 없이 [[Prototype]]을 통해서 prototype의 메서드를 자기것처럼 쓸 수 있게 한다.

<br>

#### 기본형 데이터

기본형 데이터의 숫자, 문자열 등은 객체가 아니지만, 메서드를 쓰려고 하면 자바스크립트가 임시로 각 리터럴에 해당하는 생성자 함수의 instance를 만들어서 prototype을 넘겨줘서 결과 출력 후 instance를 제거한다.

<br>

#### 참조형 데이터

참조형 데이터는 처음부터 인스턴스이기 때문에 prototype을 가지고 있음.

<br>

#### prototype 접근

instance에서 생성자의 prototype으로 접근 할 수 있다.

1. `instance.__proto__`    
- 비추천. 브라우저에서 마음대로 제공하던 것을 ES2015에서 호환상 공식화 해줌

2. `Object.getPrototypeOf(instance)`
- 공식 메서드

<br>

```javascript
function Func(a, b) {
  this.a = a;
  this.b = b;
}
const data = new Func('A입니다', 'B입니다');
const dataClone1 = new data.__proto__.constructor('A1입니다', 'B1입니다');
const dataClone2 = new data.constructor('A2입니다', 'B2입니다');
const dataClone3 = new Object.getPrototypeOf(data).constructor('A3입니다', 'B3입니다');
const dataClone4 = new Func.prototype.constructor('A1입니다', 'B1입니다');
// 모두 같은 것을 가리킴
```

<br>

#### 메서드 상속

함수 하나로 여러 개의 instance를 만들고 각자 같은 메서드를 실행해야 한다면, instance별 메서드를 만드는 것이 아니라 생성자의 prototype으로 만든다.

instance를 생성할 때 각 instance는 고유한 데이터만 가지고 있고, 모든 instance에서 똑같이 필요한 정보는 생성자의 prototype로 보내면 자신의 것처럼 사용할 수 있다.

<br>

#### prototype chaining

생성자의 prototype 역시 객체이기 때문에 object 생성자 함수의 instance이며, object의 prototype과 연결이 되어있다.

따라서 instance는 object의 prototype을 자신의 것처럼 사용할 수 있다.

이렇게 object.prototype, constructor.prototype, instance로 연결되는 것을 '프로토타입 체인'이라고 한다.

<br>

프로토타입은 모두 객체이므로 모두 동일한 구조를 따른다.

모든 데이터타입에 대해 [[Prototype]]으로 연결된 object.prototype에는 자바스크립트 전체를 통괄하는 메서드들이 들어있다.

`hasOwnProperty()`, `toString()`, `valueOf()`, `isPrototypeOf()`등 공통된 메서드는 프로토타입 체인을 통해 접근할 수 있다.

<br>

그렇기 때문에 object의 prototype에는 object 전용 메서드를 정의할 수 없다.

```javascript
obj.freeze(); // X
Object.freeze(obj); // O

obj.keys(); // X
Object.keys(obj) // O
```

프로토타입 체이닝 때문에 모든 데이터 타입에 적용되기 때문이다.

그래서 객체 관련 메서드들 생성자 함수에 적용해야 했다.

<br>

프로토타입 체인을 배열을 통해 알아보자.

```javascript
[1,2,3].toString(); // "1,2,3"
```

Array 생성자에 `toString()` 메서드가 있다.

<br>

```javascript
delete Array.prototype.toString;
[1,2,3].toString(); // "[object Array]"
Object.prototype.toString.call([1,2,3]);  //"[object Array]"
```

Array 생성자의 `toString()` 메서드를 지워도 Object.prototype에 있어서 프로토타입 체이닝을 통해 사용한다.

처음에 메서드를 호출하면 인스턴스 자기 자신에게서 찾고, 없으면 프로토타입을 통해 한단계씩 타고 올라가서 가장 먼저 찾아진 메서드를 사용하기 때문이다.

<br>

```javascript
delete Object.prototype.toString;
[1,2,3].toString(); // Error : [1,2,3].toString is not a function
```

Object.prototype의 메서드까지 지워야 사용할 수 없어진다.

<br>
