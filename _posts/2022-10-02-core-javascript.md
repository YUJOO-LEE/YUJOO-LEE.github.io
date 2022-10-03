---
layout: post
title:  코어 자바스크립트 인프런 5. closure
date:   2022-10-02 23:27:53 +0900
comments : true
categories: Review
tags: [inflearn, javascript]
---


[인프런 강의](https://www.inflearn.com/course/%ED%95%B5%EC%8B%AC%EA%B0%9C%EB%85%90-javascript-flow) 보면서 정리해두기.

<br>

### closure

A의 LexicalEnvironment와 내부 함수 B의 조합에서 나타나는 특별한 현상

컨텍스트 A에서 선언한 변수를 내부함수 B에서 참조할 경우에 발생하는 특별한 현상

<br>

```javascript
const outer = function() {
  let a = 1;
  const inner = function() {
    return ++a;
  }
  return inner;
}
const outer2 = outer();
console.log(outer2());
console.log(outer2());
```

<br>

outer 내에서 선언된 변수가 inner 내에서 참조가 되고 있다면 outer 함수가 종료 되었음에도 불구하고 변수가 살아있는 현상.

<br>

#### 클로저의 활용

1. 이를 통해 함수 종료 후에도 값을 유지하는 변수를 만들어 사용

2. 외부로부터 내부 변수 보호 (캡슐화)    
- function내 return을 객체로 해서 get, set 프로퍼티를 이용해 내부 변수를 보호할 수 있다.

<br>

#### getter, setter

```javascript
let obj = {
  get propName() {
    // getter : obj.propName 시 실행
  },
 
  set propName(value) {
    // setter : obj.propName = value 시 실행
  }
};
```

<br>

#### 클로저와 캡슐화

```javascript
function func(_d){
  b = 'B입니다';
  c = 'C입니다';
  return {
    c
    get d() { return _d },
  }
}
const a = func('D입니다');
```

위의 코드를 실행 후에는 b 값을 전역에서 불러오고 싶어도 불러올 수가 없다.

함수가 종료되면서 내부에 선언된 지역변수가 사라졌기 때문이다.

<br>

하지만 c는 return 값으로 참조하였으므로 a.c는 함수 종료 후에도 여전히 사용할 수 있다.

수정 또한 가능하다.

```javascript
console.log(a.c); // 'C입니다'
a.c = 'Z로 바뀌었습니다';
console.log(a.c); // 'Z로 바뀌었습니다' : 바뀜
```

<br>

추가적으로 getter를 활용하면 해당 값을 보호할 수 있다. (캡슐화)

```javascript
console.log(a.d); // 'D입니다'
a.d = 'A입니다';
console.log(a.d); // 'D입니다' : 바뀌지 않음
```

<br>
