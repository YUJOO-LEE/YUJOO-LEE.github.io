---
layout: post
title:  useHistory, useNavigate, useLocation, props 넘기기
date:   2022-11-04 09:02:02 +0900
comments : true
categories: Note
tags: [react, hook]
---


### Router-DOM V5

#### useHistory()

JSX에서 라우터를 이동할 때 라우터의 Link 를 사용하지만 js에서 이동을 시키고 싶을 때는 `useHistory()` 를 사용한다.

react 로부터 useHistory 를 `import` 하고 history 로 정의해 사용한다.

```javascript
const history = useHistory();
```

<br>

#### history.push()

실제로 페이지를 이동할 때는 `history.push(경로)` 형태로 사용한다.

인자로 문자열만 넣으면 단순하게 해당 경로로 이동하지만, 객체 형태로 넣으면 경로 뿐만이 아니라 다른 데이터도 같이 보낼 수 있다.

```javascript
history.push({
  pathname:`/이동할경로`,
  state:{
    키1 : [{
        키1_1: '값1-1',
        키1_2: '값1-2'
    }],
    키2 : '값2-1'
  }
})
```

<br>

#### history.replace()

`push()` 를 하면 이동 전의 URL이 history stack 에 쌓여서 이동한 기록이 남지만, `replace()` 를 하면 기록이 남지 않는다.

뒤로가기로 현재 페이지로 돌아오지 않아야 할때 사용하면 된다.

<br><br>
<hr>
<br><br>

### Router-DOM V6

#### useNavigate()

라우터 6버전에서는 useHistory 가 useNavigate 로 변경되었다.

useNavigate 를 `import` 해준다.

```javascript
import { useNavigate } from "react-router-dom";
```

사용법은 `useHistory()` 와 유사하다.

```javascript
navigate('이동할 경로', { state: {
  키1: "값",
  키2: "값"
}, replace: false })
```

state 에 이동시 전달하길 원하는 데이터를 넣는다.

replace 를 true로 주면 `history.replace()` 와 같이 현재 URL의 기록이 남지 않는다.

<br><br>
<hr>
<br><br>

### 값 넘겨받기

`<BrowserRouter>` 나 `<HashRouter>` 안의 Route로 감싸진 컴포넌트는 props 안에 history, location, match 가 기본적으로 포함되어 있다.

history.push 이후 받아온 데이터는 location으로 받아온다.

```javascript
export function 컴포넌트({location}) {
  return (
    <h1>{location.state.키1.키1_1}<h1>
  );
}
```

<br>

만약 Route로 감싸진 컴포넌트가 아니라면 `useLocation()` Hook 을 사용한다.

```javascript
import { useLocation } from "react-router-dom";

export function 컴포넌트() {
  const location = useLocation();

  return (
    <div>{location.state.키1.키1_1}</div>
  );
}
```

구조분해 할당으로 바로 정의해서 사용할 수도 있다.

```javascript
import { useLocation } from "react-router-dom";

export function 컴포넌트() {
  const { state } = useLocation();

  return (
    <div>{state.키1.키1_1}</div>
  );
}
```

<br>

