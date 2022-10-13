---
layout: post
title:  학원 수업 내용 Day16
date:   2022-10-12 22:02:03 +0900
comments : true
categories: Note
tags: [decodelab, react, hook]
---

### 리액트 랜더링

**SSR (Server Side Rendering)**

- 브라우저가 서버에 각각의 html 파일을 요청해서 화면을 랜더링한다.    
-- 초기 로딩 속도 빠르다.    
-- 페이지 이동시 화면이 깜빡임, 변경될 필요 없는 공통영역까지 모조리 변경한다.

<br>

**CSR (Client Side Rendering)**

- 모든 컴포넌트를 jsx 자바스크립트 파일 형태로 초기 로딩시 모두 불러와서 랜더링한다.    
-- 메뉴 이동 시 부드럽게 실시간으로 서브 컨텐츠를 보여준다. (서버에 요청할 필요가 없음)    
-- 변경될 부분만 실시간으로 바꿔준다. (이미 불러온 컴포넌트로 교체)    
-- 초기 로딩 속도가 비교적 느리다.

<br>

### DOM (Document Object Model)

- html, css 문법들을 브라우저가 해석해서 객체 형태로 변환한다.

<br>

**Real DOM**

- HTML 파일에 입력된 내용을 토대로 화면에 출력된 DOM

<br>

**Virtual DOM**

- JSX 문법을 통해서 실제 브라우저에 real DOM으로 출력되기 전 메모리상에서 빠르게 만들어지는 가상 DOM

<br>

**JSX**

- 자바스크립트의 확장 문법으로 XML과 유사하다.

- Virtual DOM을 쉽게 만들기 위해 사용한다.

<br>

### Component

- 리액트는 통으로 된 하나의 페이지가 아니라, 하나의 페이지를 개별적인 여러 조각으로 나누어 만들고 이를 한 페이지에 불러와서 사용한다.

- 자바스크립트에서 function단위로 쪼개서 사용하는 것과 비슷하다.

- 클래스형 컴포넌트와 함수형 컴포넌트로 만들 수 있는데 일단은 함수형을 배우는 중.

```javascript
function Component(props) {
  return (
    <div>
      <p>Hello React!</p>
    </div>
  );
}
```

부모에서부터 props를 인자로 받아서 JSX로 Virtual DOM을 return해서 부모로 다시 넘겨 사용한다.

<br>

**Props**

- 부모가 자식한테 넘겨주는 값으로 읽기 전용이다.
​
<br>

**State**

- 컴포넌트 자신이 가지고 있는 값으로 `setState()` Hook을 사용해서 변경 가능하다.

<br> 

​
### Hook

함수형 컴포넌트에서 사용할 수 있는 리액트 자체 함수

<br>

#### setState()

```javascript
import { useState } from 'react'; // useState를 react로부터 가져온다 (사용 선언)

function Example() {
  const [count, setCount] = useState(0);
  // [state값 식별자, setter함수 식별자] = useState(기본값);

  return (  // JSX로 작성한 Virtual DOM
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
 );
}
```

클릭 시 `{count}` 자리의 값만 유동적으로 바뀌는 것을 확인할 수 있다.

```javascript
// 변수 선언 부분
const [count, setCount] = useState(0);

// 함수 선언 부분
function plus() {
  setIndex(++count);
  // count++를 하면 아래 console.log 에서 바뀐 값이 바로 출력이 되지 않는다.
  console.log(count);
}

// JSX 부분
<button onClick={() => plus()}>
  Click me
</button>
```

위와 같이 따로 함수로 빼서 함수 내에서 바로 사용하려고 하면 전위 연산자를 사용해야 한다.

후위 연산자를 사용하면 할당 먼저 해버리고 연산을 하기 때문이다.

<br>

#### 전위, 후위 연산자

```javascript
let a = 5;
let result = 0;

//선할당 후증가
result = x++;
console.log(result, x);  // 5,6

//선증가 후할당
result = ++x;
console.log(result, x);  // 7,7

//선할당 후감소
result = x--;
console.log(result, x);  // 7,6

//선감소 후 할당
result = --x;
console.log(result, x);  // 5,5
```

<br>

#### useRef()

React에서 생성한 Virtual DOM 요소를 컴포넌트에서 사용하기 위해 식별자를 지정할 수 있다.

HTML의 ID와 유사하다. React에서 사용하는 전용 ID 이다.

`const frame = useRef(null)`    
- 식별자 선언 (초기값 null)

`<section ref={frame}>`    
- DOM 요소를 할당

`<Btns deg={deg} frame={frame}/>`    
- prop으로 보내서 사용한다.

<br><br>
<hr>
<br><br>

### [Music Player](/music_player/)

<br>

<iframe src='/music_player/' frameborder='0' width='100%' height='500px'></iframe>

<br>

지금까지 수업 때 HTML, CSS, Vanilla JS로 만든 예제들은 모두 한 repository에 커밋했는데, react 예제들은 매번 별도의 repository로 올릴 예정이다.

repo생성할 때 마다 사용할 터미널 명령을 하나에 정리해 놓아야겠다..

위는 articles, header, footer, btns로 각각의 컴포넌트를 만들어 app.js에서 출력하는 첫번째 리액트 예제!

너무 재밌다 빨리 뭔가 동적으로 데이터들을 가지고 더 많이 만들고 싶다.

<br>
