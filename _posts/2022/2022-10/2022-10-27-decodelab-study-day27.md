---
layout: post
title:  redux 설치, 사용방법, useSelector, useDispatch
date:   2022-10-27 14:15:21 +0900
comments : true
categories: Note
tags: [decodelab, react, redux, terminal, github]
---

학원 수업 Day27

### git Clone

```linux
git clone 레포지토리주소
```

원하는 경로에서 터미널을 열어서 해당 명령어를 사용하면 해당 경로에 레포지토리 내용이 복제된다.

<br>

### Redux

리액트같은 싱글 페이지 어플리케이션에서 상태 관리를 쉽게 할 수 있도록 도와주는 라이브러리이다.

자바스크립트 라이브러리이기 때문에 리액트 뿐 아니라 뷰, 앵귤러 등 여러 라이브러리 또는 프레임워크에서 사용할 수 있다.

그동안 리액트를 사용하면서 특정 컴포넌트에서 axios 등을 이용해 데이터를 불러오면 그 데이터를 다른 컴포넌트로 넘겨주기 위해서는 받는 쪽이 하위 컴포넌트 일 경우 props로 넘기거나, 받는 쪽이 상위 컴포넌트 일 경우 forwardRef 등으로 넘겨야 해서 상당히 불편했다.

그러나 리덕스를 사용하면 데이터를 먼저 store라는 공간에 불러온 후 그 데이터를 전역에서 편리하게 사용할 수 있게 된다.

<br>

#### 사용 방법

먼저 react-redux를 설치 해 준다.

```npm
npm i redux react-redux
```

설치가 완료되면 데이터를 다룰때 필요한 파일들을 scr/redux 에 생성한다.

<br>

#### action.js

```javascript
export const set데이터 = (받아온 데이터)=>{
  return {
    type: 'SET_데이터',
    payload: 받아온 데이터
  }
}
```

action.js 파일은 데이터를 수정 등 가공이 필요할 때 사용 할 예정이다.

내부에는 데이터가 필요한 컴포넌트에서 사용할 함수를 작성한다.

컴포넌트에서는 여기서 작성한 `set데이터()` 함수를 사용해서 데이터를 가공하도록 요청한다.

action.js 내의 함수 자체는 마치 요청서와 같아서 받아온 데이터를 가공할 카테고리와 데이터를 정리해서 객체 형태로 `return` 한다.

`return` 한 객체는 추후 `useDispatch()` 메서드에 의해 reducer.js 에서 넘겨받아서 가공을 처리한다.

<br>


#### reducer.js

```javascript

// 초기 데이터를 state 에 저장했다가 action 객체가 전달되면 type에 따라서 기존 데이터 변경
const 데이터Reducer = (state={데이터: []}, action)=>{
  switch(action.type){
    case 'SET_데이터':
      return { state: action.payload }
    
    default:
      return state;
  }
}
```

reducer.js 에는 action.js 에서 작성한 요청서를 실제로 처리하는 함수를 작성한다.

데이터Reducer로 이름지어진 해당 함수는 기존 데이터와 action.js 에서 `return` 한 객체를 파라미터로 받아서 데이터 가공을 처리하게 된다.

보통 기존 데이터는 `state`, action.js에서 `return` 한 객체는 `action` 이라고 받아온다.

기존 데이터가 없을 시 빈 데이터 값도 넣을 데이터 형식과 같은 형식으로 지정해주면 syntax error를 방지할 수 있을 것이다.

그리고 받아온 객체의 type 값에 따라 case 를 나눠 데이터를 처리한다.

만약 받아온 type이 미리 설정해둔 case에 해당되지 않는다면 기존 데이터를 변경 없이 반환하는 default 값도 만들어준다.

type에 맞는 case가 있을 경우, 데이터를 action에 담긴 데이터로 바꾸어서 `return` 한다.

```javascript
// 최상단
import { combineReducers } from 'redux';

// 최하단
const reducers = combineReducers({ 데이터Reducer });
export default reducers;
```

이 데이터 가공 함수들은 데이터 카테고리, 가짓수에 따라 여러 개 만들 수 있다.

하지만 결과적으로 저장되는 공간, 그릇은 하나이므로 함수들을 하나의 객체로 합쳐야 한다.

그때 하나로 합쳐주는 hook이 `combineReducers()` 이다.

객체로 합쳐지면 마지막에는 그 값을 export 해 준다.

reducer.js 은 결국 여러 종류의 데이터를 받고 그걸 일부 가공한 후 묶어서 내보내는 역할을 하는 파일이다.

<br>

#### store.js

```javascript
import { createStore } from 'redux';
import reducers from './reducer';

const store = createStore(reducers);

export default store;
```

store.js 는 reducer.js에서 반환한 함수와 데이터를 담기 위해 `createStore()` 메서드를 통해 공간을 만들고 그 공간을 채워서 최종적으로 내보내는 역할을 한다.

이제 데이터를 담아오고, 내보낼 준비를 끝냈다.

<br>

#### store 사용

```javascript
import { Provider } from 'react-redux';
import store from './redux/store';
```

```jsx
<Provider store={store}>
  <App />
</Provider>
```

store에 담긴 데이터들을 전역에서 사용하기 위해, index.js 에 redux 관련 내용을 추가한다.

App을 Provider로 둘러 주어야 App 내, 전역에서 store에 접근할 수 있다.

<br>

#### useSelector()

`useSelector()` 는 저장된 데이터를 컴포넌트에 불러오는 메서드이다.

```javascript
import { useSelector } from 'react-redux';
...

const Members = useSelector((store)=> store.memberReducer.members);
```

일단 최상단에 import 해준 후, 함수 내에서 사용하면 store 공간에 저장된 모든 데이터를 받아올 수 있는데, 원하는 특정 데이터를 골라서 변수에 담고 사용하면 된다.

<br>

#### useDispatch()

값을 불러올 수 있다면, 수정 또한 가능하다.

`useDispatch()` 는 새로운 값을 action.js 에서 만들었던 함수를 이용해서 reducer.js 로 보내 가공하도록 연결시켜준다.

```javascript
import { useDispatch } from 'react-redux';
import { set데이터 } from './redux/action';
```

먼저 `useDispatch()` 메서드와 앞서 action.js 에서 만들어두었던 데이터 요청서 함수를 import 해온다.

만약 전체 데이터를 새로운 값으로 바꾸는 것이 아니라 현재 데이터에서 일부만 수정하고 싶다면 `useSelector()`를 사용해서 기존 데이터를 받아와서 일부 값을 수정한다.

```javascript
const dispatch = useDispatch();

const getMembers = async ()=>{
  const url = `${process.env.PUBLIC_URL}/데이터.json`;
  const result = await axios.get(url);
  dispatch(set데이터(result.data.members));
}
```

먼저 `useDispatch()` 메서드를 dispatch 변수에 담는다.

그리고 원하는 변경 값을 `dispatch(데이터액션(값))`의 형태로 실행한다.

해당 함수를 원하는 이벤트에 삽입하면 데이터가 변경이 되는 것을 확인할 수 있다.

<br>