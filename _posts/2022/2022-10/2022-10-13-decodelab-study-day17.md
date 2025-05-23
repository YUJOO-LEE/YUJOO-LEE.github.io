---
layout: post
title:  리액트 라우터 (Router) 버전 이슈, useEffect
date:   2022-10-13 16:13:14 +0900
comments : true
categories: Note
tags: [decodelab, react, router]
---

학원 수업 Day17

### Router

리액트로 만들어진 페이지는 하나의 페이지에 각각의 컴포넌트 조각을 상황에 맞게 끼워서 보여준다.

그러나 마치 서브 페이지가 있는 것처럼, 링크 태그의 동작을 위해, 링크를 타고 들어가면 해당 경로에 맞는 컴포넌트를 띄워주는 것을 라우터가 해준다.

<br>

#### 리액트 라우터 돔 설치

```npm
npm install react-router-dom
// 설치

npm uninstall router-react-dom
// 삭제
```

<br>

#### 버전 이슈

현재 *22년 10월* 기준 Router v6의 기능이 v5에 비해 상당히 많이 바뀌었는데, 현재는 v5 기준으로 학습하므로 v5로 재설치했다.

v6이 나온지 얼마 되지 않아 검색해도 대부분 코드가 v5 기준으로 나온다..

*package.json* 파일의 *react-router-dom* 부분에서 router-dom 버전을 수정한 후 `npm i`로 인스톨한다.

```npm
"react-router-dom": "^5.3.0",
```

v6에서 수정된 기능은 아래 링크에서 추후에 다시 학습하기로 한다.

[React Router](https://reactrouter.com/en/main)

<br>

### Switch

Switch 는 같은 경로의 라우터 연결 시 구체적인 라우터 하나만 적용한다.

한개의 컴포넌트로 경로 별 출력 내용을 구분하기 위해 prop으로 구분 값을 넘긴다.

```javascript
<Switch>
  <Route exact path='/'>
    <Header type={'main'} />
  </Route>

  <Route path='/'>
    <Header type={'sub'} />
  </Route>
</Switch>
```

`exact` 정확하게 그 경로일 때만 불러오기.

없으면 path로 시작하는 모든 경로에 불러온다.

<br>

### Link, NavLink

모두 HTML a태그 대신 페이지 링크 기능을 하는 태그이다.

둘의 가장 큰 차이는 active 상태 반환이 가능하다는 것이다.

NavLink 는 activeStyle, activeClassName 등을 사용 가능하다.

- 라우터 버전 6부터는 active@ 가 삭제되고 style, className 만 사용 가능한데, 값에서 isActive 조건문으로 동일한 기능 구현이 가능하다.

<br>

### Browser vs Hash

#### hashRouter

- 주소에 #가 붙는다. 

- 새로고침을 해도 화면이 유지된다. 따라서 서버가 주소의 이동(페이지의 존재 유무)를 모른다.

- 검색 엔진에서 접근할 수 없다는 단점이 있다.

- 정적인 페이지에 적합하다.

- 깃허브 배포가 비교적 편리하기 때문에 포트폴리오용으로 사용률이 높다.

<br>

#### browserRouter

- 주소가 깔끔한 대신에 새로고침시 서버에 요청이 들어간다.

- 새로고침 문제는 서버에서 세팅을 해주면 해결할 수 있다고 한다.

- 서버에서 데이터를 가공해서 전달받는 동적인 웹페이지에 적합하다.

- SEO 최적화 때문에 browserRouter 사용률이 월등히 높다.

<br>

### useEffect()

아래와 같은 특수한 상황에서 작업을 실행시키기 위해 사용한다.

- 컴포넌트가 마운트 되었을 때    
-- 처음 나타났을 때    
-- props로 받은 값을 컴포넌트의 로컬 상태로 설정할 때    
-- 외부 API요청이 있을 때    
-- setInterval, setTimeout 통해 작업이 예약될 때

- 컴포넌트가 언마운트 되었을 때    
-- 사라질 때    
-- clearInterval, clearTimeout 되었을 때    
-- 라이브러리 인스턴스가 제거 되었을 때    

- 컴포넌트가 업데이트 되었을 때    
-- 특정 props가 바뀔 때

<br>

#### 사용방법

```javascript
import { useEffect } from "react";

useEffect(callback, [deps]);
```

<br>

#### callback

- 랜더링 이후 실행 할 함수.

<br>

#### [deps]

배열의 형태로, 특정한 값이 변경될 때 callback 함수를 실행하고 싶다면 그 식별자를 넣어둔다.

빈 배열을 입력하면 컴포넌트가 마운트 될 때에만 실행되며, 배열도 없이 비워두면 마운트 뿐만이 아니라 업데이트가 될 때에도 실행 된다.

<br>

#### cleanup

callback 함수 내에서 return 함수를 실행하면 컴포넌트가 언마운트 될 때 해당 함수가 실행된다.

`useEffect()` 의 뒷정리 *-이벤트 삭제 등-* 을 해 주어야 할 때 사용한다.

<br>

```javascript
const frame = useRef(null);
useEffect(()=>{
  // 마운트 되었을 때
  frame.current.classList.add('on');
  console.log('on');

  return ()=>{
    // 언마운트 되었을 때 특수경우 아니면 잘 쓰지 않음
    frame.current.classList.remove('on');
    console.log('off');
  }
}, []);
// useEffect(콜백함수, 뎁스: 함수 반복시 사용);
```

<br><br>
<hr>
<br><br>

### [기본 레이아웃](/layout_practice/)

<br>

<iframe src='/layout_practice/' frameborder='0' width='100%' height='500px'></iframe>

<br>

기본 레이아웃으로 연습중!

서브 페이지들을 생성하고 헤더, 푸터 외에도 서브 페이지 각각의 내용과 프레임으로 컴포넌트를 분리했다.

```jsx
// 개별 서브페이지 내 JSX
<Layout name='community'>
  <p>Community contents</p>
</Layout>
```

```jsx
// Layout frame 내 JSX
<section className={`content ${props.name}`}>
  <figure></figure>
  <div className='inner'>
    <h1>{props.name}</h1>
    {props.children}
  </div>
</section>
```

props로 받은 name 과 children 부분만 개별 분리 되어있고, 나머지 그 외 Layout 부분은 공통으로 사용한다.

`props.children` 은 해당 요소의 `innerHTML` 을 가져오는 듯.

<br>

- JSX내 문자열 + js 동시 사용(백틱 사용) 시 중괄호로 백틱을 감싸준다.

```jsx
<section className={`content ${props.name}`}>
```

<br>
<br>