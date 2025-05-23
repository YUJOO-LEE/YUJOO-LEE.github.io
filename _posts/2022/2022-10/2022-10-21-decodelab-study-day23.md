---
layout: post
title:  동기와 비동기, promise, async/await, 객체지향
date:   2022-10-21 20:03:13 +0900
comments : true
categories: Note
tags: [decodelab, react, scroll]
---

학원 수업 Day23

### 동기, 비동기


동기
 
- 코드 실행이 순차적으로 진행되는 방식이다.    
- 한번에 한가지의 작업만 할 수 있고 그동안 다른 작업은 기다리고 있다가 현재 작업이 완료되어야 차례대로 실행된다.


비동기

- 작업을 호출 한 후 작업의 진행 및 완료 여부와 관계없이 다음 작업을 시작하는 방식이다.    
- Rest API를 호출해서 받은 정보로 코드를 실행하면 호출 결과가 나오기 전에 코드를 실행하면 문제가 생긴다.

<br>

자바스크립트는 코드가 동기식 언어이다.

자바스크립트가 함수를 호출할때, 실행되는 함수가 Callstack으로 차곡차곡 쌓였다가 해당 함수가 실행이 완료되면 Callstack에서 제거되는데, 비동기 함수로 호출하면 Callback Queue 에 쌓였다가 Callstack 이 비면 그때 Callstack 으로 들어와서 실행되게 된다.

비동기로 처리하고 싶다면 `setTimeout()` 과 같은 비동기 함수를 사용해야 한다.

```javascript
function settingTimeout(callback, delay) {
  callback();
}

console.log(1);
settingTimeout(()=>{
  console.log('가짜 셋타임 아웃');
}, 0)
console.log(2);
setTimeout(()=>{
  console.log('셋타임 아웃');
}, 0);
console.log(3);
```

위 코드를 실행하면 콘솔에는 1, '가짜 셋타임 아웃, 2, 3, '셋타임 아웃'의 순서로 찍히게 된다.

`settingTimeout()` 으로 명명한 일반함수는 동기식으로 실행되어 순차에 맞게 실행된다.

하지만 `setTimeout()` 은 아무리 경과시간을 0으로 주어도 일단 callback queue에 들어갔다가 callstack이 비면 그때서야 들어오기 때문에 뒤에 있는 3이 먼저 출력이 된다.

`setTimeout()` 을 두개 만들고 각각 1000ms와 2000ms으로 준다면 시간의 스타트 지점은 거의 동시이기 때문에 1000ms 간격으로 두개의 함수가 실행이 된다.

만약 비동기로 실행된 함수 다음에 실행된 함수를 동기식으로 붙여넣고 싶다면, 예를 들어서 첫번째 함수는 1000ms후에, 두번째 함수는 첫번째 함수 종료 후 2000ms후에 실행하고 싶다면, 두번째 함수를 첫번째 함수의 callback 함수로 넣어서 실행시켜 주어야 한다.

```javascript
function delay(sec, callback) {
  setTimeout(()=>{
    callback(new Date().toISOString());
  }, sec * 1000)
}

delay(1, (result)=>{
  console.log(1, result);

  delay(2,(result)=>{
    console.log(2, result);

    delay(3,(result)=>{
      console.log(3, result);
    })
  })
})
```

위 코드 실행 시 1초 후 첫번째 실행, 첫번째 실행 종료 후 2초뒤 두번째 실행, 두번째 실행 종료 후 3초위 세번째로 실행이 된다.

원하는 대로 앞선 함수의 종료를 기다렸다가 실행하게 된 것이다.

하지만 이어지는 코드가 많아진다면 가독성이 현저히 떨어지게 된다.


<br>

#### Promise / then

```javascript
const delay = () => {
  return new Promise((resolve, reject)=>{
    setTimeout(()=>{
      resolve('완료 ' + new Date().toISOString());
    }, 1000)
  });
};

delay().then((result)=>{
  console.log(1, result);
  return delay();
}).then((result)=>{
  console.log(2, result);
  return delay();
}).then((result)=>{
  console.log(3, result);
})
```

Promise 를 사용하면 비동기로 실행되는 함수를 별도로 순차적으로 처리되도록 동기적으로 만들 수 있다.

then은 앞선 함수가 Promise를 반환한다면 해당 함수의 종료를 기다렸다가 현재 함수를 시작한다는 뜻이다.

callback 방식보다 훨씬 아름답다.

<br>

#### Promise.all, race

```javascript
function timer(time) {
  return new Promise((resolve, reject) => {
    setTimeout(()=>{
      Boolean ? resolve(time) : reject(false);
    }, time)
  })
}

console.time('Promise.all');
const loading = Promise.all([timer(1000), timer(2000), timer(3000)])
  .then((result)=>{
    console.log('결과', result);
    console.timeEnd('Promise.all');
  });

loading
  .then((result)=>{
    // 정상 실행 완료 시
    console.log('로딩 완료');
  })
  .catch((result)=>{
    // 예외, 에러 처리
    console.log('로딩 실패');
  })
  .finally(()=>{
    // 정상, 에러와 관계 없이 마지막에 실행
    console.log('종료');
  })
```

`Promise.all()`, `Promise.race()`는 여러개 Promise 의 실행 여부에 따라 결과를 반환한다.

`Promise.all()`    
- 배열에 들어있는 Promise 함수가 모두 실행 완료 되어야 결과 반환

`Promise.race()`    
배열에 들어있는 Promise 함수들 중 가장 빨리 완료되는 것이 있으면 해당 결과만 반환

<br>

#### async, await

```javascript
function delay() {
  return new Promise((resolve, reject)=>{
    setTimeout(()=>{
      resolve('완료 ' + new Date().toISOString());
    }, 1000)
  });
};

async function fistAsync() {
  const result = await delay();
  console.log(result);
}
fistAsync();
```

async, await 는 then 보다 훨씬 가독성 있게 쓸 수 있다.

await는 async와 짝꿍으로 사용되며, Promise 객체를 반환하는 함수를 async이 선언된 함수로 감싸고, 그 함수 안에서 await로 Promise 객체 반환 함수를 호출하면 된다.

await 아래의 코드들은 await로 선언된 함수가 실행이 완료되어야 호출된다.

<br>

#### try, catch

```javascript
function delay(time = 2000, cond = true) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      cond ? resolve('success') : reject('fail');
    }, time);
  });
}

async function test() {
  try{
    const result = await delay(3000, false);
    // 정상 시 아래 실행
    console.log(result);
  } catch(err) {
    // 예외 시 실행
    console.log(err);
  } finally {
    // 무조건 마지막에 실행
    console.log('종료');
  }
}
test();
```

await는 then의 의미이므로 에러 처리를 할 수 없다.

따라서 async 함수 내에서 try, catch로 에러를 별도 지정한다.

try로 해당 함수를 실행시키고, Promise 객체 실행이 정상이면 다음줄 출력, 예외 상황이라면 다음줄은 실행하지 않고 catch 로 넘어간다.

<br><br>
<hr>
<br><br>

### 객체 지향

서로 연관된 변수와 함수를 그룹으로 묶고 이름을 붙여 객체화 하면, 중복되고 복잡한 코드를 깔끔하게 정리할 수 있다.

한번만 객체의 틀(생성자 함수)을 작성해두면, 그 틀에 맞춘 객체(인스턴스)를 바꿔서 몇번이고 새로 생성할 수 있게 된다.

<br>

ES6 버전 이하에서는 아래와 같이 함수와 해당 함수의 prototype을 이용해서 객체화 했다.

```javascript
function FontStyle(el, color, size) {
  this.el = document.querySelector(el);
  this.changeColor(color);
  this.changeSize(size);
}

FontStyle.prototype.changeColor = function(color) {
  this.el.style.color = color;
}
FontStyle.prototype.changeSize = function(size) {
  this.el.style.fontSize = size;
}

const txt1 = new FontStyle('h1','red','14px');
const txt2 = new FontStyle('p','blue','12px');
```

<br>

그리고 ES6부터 class 문법이 도입되면서 좀 더 편하게 객체를 생성할 수 있게 됐다.

```javascript
class FontStyle {
  constructor(el) {
    // 프로퍼티 생성자
    this.el = document.querySelector(el);
  }

  changeColor(color) {
    // 메서드
    this.el.style.color = color;
  }

  changeSize(size) {
    this.el.style.fontSize = size;
  }
}

const txt1 = new FontStyle('h1');
txt1.changeColor('red');
txt1.changeSize('50px');
```

객체간의 상속도 용이해졌다.

```javascript
class BoxStyle extends FontStyle{
  // BoxStyle은 FontStyle을 참조한다.
  constructor(el){
    super(el);
    // FontStyle의 프로퍼티 생성자를 참조해서 사용
  }

  changeBoxSize(wid, ht) {
    this.el.style.width = wid;
    this.el.style.height = ht;
  }

  changeBg(bg) {
    this.el.style.backgroundColor = bg;
  }
}

const box = new BoxStyle('div');
box.changeBoxSize('200px', '100px');
// 자식 객체인 BoxStyle은 본인의 메서드도 사용할 수 있지만
box.changeSize('20px');
// 부모 객체인 FontStyle의 메서드도 사용할 수 있다
```

객체 생성 시 부모 객체를 메인 카테고리, 자식 객체를 하위 카테고리 요소를 생성하는 용도로 사용한다면 객체간의 관계를 보다 쉽게 명시, 정리할 수 있다.

부모 객체 생성자로 기계를 정의한다면, 자식 객체 생성자로 컴퓨터를 정의하는 식으로.

아마도, 그게 바로 객체 지향 패러다임이 아닐까.

나중에 좀 더 깊게 공부해야겠다.

<br>

#### [실습](/decodelab/221021/tabmenu/)

<br>

<iframe src='/decodelab/221021/tabmenu/' frameborder='0' width='100%' height='500px'></iframe>

<br>

탭 메뉴를 생성자 함수를 이용해 구현 해 보았다.

<br>
