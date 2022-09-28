---
layout: post
title:  코어 자바스크립트 인프런 2. Execution context
date:   2022-09-28 21:31:53 +0900
comments : true
categories: Review
tags: [inflearn, javascript]
---


인프런 강의 보면서 정리해두기.

눈이 트이는 느낌이다.

<br>

### Execution context

실행 컨택스트는 코드를 실행하는 데에 필요한 배경이 되는 조건/환경이다.

<br>

전역공간 : 자바스크립트가 실행될 때 생성되어 종료될때 종료되어 함수로 볼 수 있음

module : import되는 순간 내부 컨텍스트가 생성되어 모듈코드가 종료될 때 종료되어 함수로 볼 수 있음

결국 실행 컨텍스트는 **함수**를 실행할때 필요한 조건 또는 환경정보이며,

전역공간, 모듈 또는 함수로 묶인 내부에서는 같은 환경 안에 있는 것이 됨.

*if/for/switch/while문은 블록스코프로 let, const에 대해 독립된 공간으로의 역할은 하지만 별도의 실행컨텍스트가 생성되는 것은 아님.*

<br>

```javascript
function outer(){
  코드1
  function inner(){
    코드2
  }
  inner();
  코드3
}
outer();
코드4
```

<br>

위 코드는 전역, outer, inner 순으로 컨텍스트가 생성되어(stack),

1, 2, 3, 4 순서로 코드가 실행된다.

<br>

### call stack

함수의 동작 순서를 제어하는 자료구조

컨텍스트가 전역, outer, inner순으로 쌓인 후 마지막에 들어온 inner부터 실행하여 종료되면 outer, 종료되면 전역 컨텍스트가 실행된다.

<br>

### 환경정보

- 현재 환경 관련 식별자

#### **VariableEnvironment** : 식별자 정보 수집    

최초 식별자 정보를 담고 있음.    
값에 변화가 생겨도 실시간 반영 안됨.

#### **LexicalEnvironment** : 식별자의 데이터 추적    

실행컨텍스트를 구성하는 환경정보가 담겨있는 사전처럼 구성한 객체
값에 변화가 생기면 실시간 반영됨.    

- **environmentRecord** : 현재 컨텍스트의 내부 식별자 정보    

-- 호이스팅(hoisting) : 현재 식별자 정보를 environmentRecord에 담는 과정을 이해하기 쉽게 말하는 허구의 개념    
식별자 정보를 최상위로 끌어올림 (함수식별자는 함수 그대로 끌어올려짐)

- **outerEnvironmentReference** : 현재 문맥에 관련 있는 외부 식별자 정보    

-- 아까 코드에서 inner의 경우 outerEnvironmentReference는 outer의 LexicalEnvironment를 참조하고,
outer는 전역의 LexicalEnvironment에서 참조한다.

-- scope chain 현상 : 변수의 유효 범위. outerEnvironmentReference에 의해 만들어짐    
가장 가까운 자신부터 점점 멀리 있는 scope로 찾아나가는 것.    
inner내부에서는 참조한 outer나 전역에서 선언한 변수를 사용할 수 있지만,
outer에서는 inner에서 선언한 변수를 사용할 수 없음.    
식별자가 동일한 경우 가장 먼저 찾은 식별자를 사용.

<br>

#### This

실행컨텍스트가 생성될 때 = 함수가 호출될 때 this가 생성된다.

즉, 함수가 어떻게 호출되느냐에 따라서 this가 결정된다.

- **전역 공간에서** : 최초 this = window(브라우저) / global(node.js)

<br>

- **함수 호출 시** : window / global

함수a를 호출한 대상이 전역객체 이므로 this는 window/global이 됨

```javascript
function a() {
  console.log(this);
  function b(){
    console.log(this);
  }
  b();  // this = window or global
}
a();  // this = window or global
```

<br>

- **메서드 호출 시** : 메서드를 호출한 주체

```javascript
let a = {
  b: function() {
    console.log(this);
  },
}
a.b();  // this = a
a[b]();  // this = a
```

메서드 : 객체와 관련된 동작을 하게되면 그 객체의 메서드 라고 함

```javascript
let a = 10;
let obj = {
  a: 20,
  b: function() {
    console.log(this.a);  // 20

    function c() {
      console.log(this.a);  // 10
    }
    c();  // this = window/global

    let self = this;  // 우회법
    function d() {
      console.log(self.a);  //20
    }
    d();  // this = window/global
  },
}
obj.b();  // this = obj
```

this 변경 : call, apply 등 this 바인딩 명령 사용 or 내부함수 상위에서 this를 별도 변수에 담아서 내부함부에서 사용

arrow function : this를 호출 시 동적으로 바인딩하지 않음. 선언 시 정적으로 결정

<br>

- **callback 호출 시** : 기본적으로는 함수 내부에서와 동일

제어권을 가진 함수가 콜백의 this를 지정해둔 경우도 있다.

```javascript
document.querySelector('a').addEventListener('click', function() {
  console.log(this); // this = DOM Element 'a'
})
```

this를 지정하고 싶으면 콜백함수 호출할때 bind, call등 사용

```javascript
함수.call(this로 넘길 객체, 매개변수1, 매개변수2, 매개변수3);
함수.apply(this로 넘길 객체, [매개변수들이 담긴 배열]);

let 변수 = 함수.bind(this로 넘길 객체);
변수(매개변수1, 매개변수2, 매개변수3);
// 아래와 같이도 가능
let 변수 = 함수.bind(this로 넘길 객체, 매개변수1, 매개변수2);
변수(매개변수3);
```

콜백함수를 호출하는 대상이 지정하는 바에 따라 this 호출

<br>

- **생성자 함수 호출 시** : 인스턴스 객체

```javascript
function Person(n, a) {
  this.name = n;
  this.age = a;
}
let a = Person('이름', 20); // this = window/global
console.log(window.name, window.age); // 이름, 20

let a = new Person('이름', 20); // this = a
console.log(a.name, a.age); // 이름, 20
```

<br>
