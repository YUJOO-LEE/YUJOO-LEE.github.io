---
layout: post
title:  코어 자바스크립트 인프런 7. class
date:   2022-10-05 23:57:02 +0900
comments : true
categories: Review
tags: [inflearn, javascript]
---


[인프런 강의](https://www.inflearn.com/course/%ED%95%B5%EC%8B%AC%EA%B0%9C%EB%85%90-javascript-flow) 보면서 정리해두기.

해당 코드는 ES6 이전에 사용하던 방법으로, ES6에서는 class가 생겨서 직접 함수처리 하지 않아도 됨. 

<br>

### class

공통적인 속성들을 모은 추상적인 개념. 정의하는 객체.

- superclass > subclass

<br>

### instance

공통의 속성을 지니는 구체적인 대상. 정의된 구체적인 데이터를 지닌 실체.

상위 클래스가 정의되어야 하위 클래스를 지정할 수 있고, 인스턴스 또한 만들 수 있다.

<br>

### static

new 생성자 없이 함수(객체)로써 호출할 때에만 의미가 있다.

해당 클래스 소속의  인스턴스들의 개별적 동작이 아닌, 소속여부 등의 공동체적인 판단을 필요하는 경우.

<br>

#### static methods

prototype이 아닌, 생성자 함수에 직접 지정된 메서드.

`Array.from()`, `Array.isArray()`, `Array.of()` 등.

instance로 직접적으로 접근할 수 없음.

constructor를 통해서는 우회가 가능하나 this는 별도 처리 필요하다.

<br>

#### static properties

prototype이 아닌, 생성자 함수에 직접 지정된 프로퍼티.

`Array.arguments`, `Array.length`, `Array.name` 등.

<br>

### prototype methods

prototype에 정의된 메서드. prototype을 생략하고 methods로만 말하는 경우가 많다.

instance에서 [[Prototype]]을 통해 접근 가능하다.

<br>

```javascript
function 생성자(a, b) {...}
생성자.Static메서드 = function(instance) {...}
생성자.prototype.메서드 = function() {...}

const 변수 = new 생성자('a','b');

변수.메서드(); // O

변수.Static메서드(변수);  // X
생성자.Static메서드(변수);  // O
```

<br>

### 클래스 상속

클래스A와 클래스B가 있을 때 공통되는 메서드가 많으면 하나로 합쳐서 상위 클래스를 만든 후, 하위 클래스로 상속이 되도록 만들 수 있다.

클래스A가 상위 클래스, 클래스B가 하위 클래스라고 한다면 코드 상으로는 `클래스B.prototype = new 클래스A()`로 두 클래스가 연결되도록 표현할 수 있다.

다만 이렇게 될 경우 클래스B의 prototype은 완전히 클래스A의 인스턴스가 되므로 본래 가지고 있던 기능을 다시 돌려줘야 한다.

자바스크립트는 prototype 객체에 자동으로 constructor를 생성해 주니 이 생성자 함수를 활용한다.

<br>

```javascript
클래스B.prototype = new 클래스A;
클래스B.prototype.constructor = 클래스B;
```

<br>

이렇게 되면 클래스A의 인스턴스가 클래스B의 prototype과 연결되어 클래스A의 메서드를 사용할 수 있다.

다만 클래스B의 입장에서는 클래스A의 메서드만 필요할 뿐 프로퍼티는 필요가 없으므로, 메서드만을 가지고 있는 Bridge를 만들어 사용하는 것이 바람직하다.

<br>

```javascript
function Bridge() {} // 빈 생성자 생성
Bridge.prototype = 클래스A.prototype;
클래스B.prototype = new Bridge;
클래스B.prototype.constructor = 클래스B;
```

<br>

또한 Douglas Crockford는 이를 함수화 해서 사용할 것을 권장했다.

<br>

```javascript
const extendClass = (function() {
  function Bridge() {}
  return function(Parent, Child) {
    Bridge.prototype = Parent.prototype;
    Child.prototype = new Bridge();
    Child.prototype.constructor = Child;
  }
})(); // 한번만 구현

extendClass(클래스A, 클래스B);
extendClass(클래스C, 클래스D);
// 계속적 활용
```

<br>

`Bridge()`라는 함수는 prototype 상속을 위한 연결 외에는 필요하지 않으므로, 클로저를 이용해서 생성자는 단 한번만 생성하고 super, sub클래스를 매개변수로 받아 계속 사용한다.

여기에 추가로 super클래스와 sub클래스의 인스턴스가 똑같은 프로퍼티를 가진다면 `Child.prototype.superClass = Parent`를 추가한다면 쉽게 활용할 수 있다.

<br>

#### 최종 코드

```javascript
const extendClass = (function() {
  function Bridge() {}
  return function(Parent, Child) {
    Bridge.prototype = Parent.prototype;
    Child.prototype = new Bridge();
    Child.prototype.constructor = Child;
    Child.prototype.superClass = Parent;
  }
})();

function 클래스A(a) {
  this.a = a;
}
클래스A.prototype.getA = function() {
  return this.a;
}

function 클래스B(a, b) {
  this.superClass(a); // 클래스A 생성자 호출
  this.b = b;
}

extendClass(클래스A, 클래스B);  // 클래스 상속

클래스B.메서드B = function(instance) { // 클래스B 전용 메서드
  return this.b;
}

const 변수 = new 클래스B('a', 'b');
```

<br>

### ES6 class

```javascript
class 클래스A { // super 클래스 생성
  constructor(a) {
    this.a = a;
  }
  getA() {
    return this.a;
  }
}

class 클래스B extends 클래스A { // 클래스 상속
  constructor(a, b) {
    super(a);
    this.b = b;
  }
  getB() {
    return this.b;
  }
}
```

<br>

자바스크립트는 발전하고 있다.

자바스크립트 만세!

<br>
