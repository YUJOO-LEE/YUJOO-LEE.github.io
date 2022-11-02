---
layout: post
title:  학원 수업 내용 Day28
date:   2022-10-30 02:50:57 +0900
comments : true
categories: Note
tags: [decodelab, react, redux, terminal, generator]
---

### generator


비동기 함수의 동기작업을 처리하기 위해 그동안은 `promise`, `then`, `asyn`, `await` 등의 구문을 사용했었다.

위의 구문을 사용하면 앞에서 일어난 상황이 끝난 직후 다음 상황이 실행하게 되는데, 이렇게 즉시 실행하지 않고 원하는 타이밍에 실행하도록 하려면 `generator` 를 사용해야 한다.


#### 사용 방법

먼저 순차적으로 실행하고 싶은 내부 함수들을 작성한다.

```javascript
function 내부함수1() {
  console.log('text1');
  return '리턴값1';
}
function 내부함수2() {
  console.log('text2');
  return '리턴값2';
}
```

그리고 이 내부 함수들을 묶을 함수를 만드는데, 함수 선언 시 `*` 를 붙여 제너레이터 함수로 만든다.

그리고 내부 함수들을 실행을 원하는 순서대로 불러오는데, 앞에 `yield` 라는 구문을 붙여준다.

이렇게 제너레이터로 선언된 함수의 내부 함수들은 호출하기 전까지 실행되지 않고 기다리게 된다.

마지막으로 해당 제너레이터 함수를 실행하기 위해 이 함수를 담을 변수를 만든다.

```javascript
function* 함수() {
  yield 내부함수1();
  yield 내부함수2();
}

const 함수를_담은_변수 = 함수();
```

이제 원하는 타이밍에 순차적으로 불러오기만 하면 된다.

먼저 만든 제너레이터 함수에 `.next()`를 붙이면 함수 내부에 불러온 순서대로 하나씩 실행한다.

```javascript
const 내부함수1_실행결과를_담은_변수 = 함수를_담은_변수.next();
// 내부함수 1의 내용으로 console.log(text1) 이 실행되어 콘솔에 text1이 출력됨
console.log(내부함수1_실행결과를_담은_변수); 
// 내부함수 1의 실행 결과가 콘솔에 출력됨
```

첫번째로 `.next()`를 실행하면 `yield` 가 붙은 내부 함수 중 가장 상위의 함수가 호출된다.

여기서는 내부 함수의 `console.log(text1)` 이 실행되어 콘솔에 text1이 출력된다.

그리고 이 변수 자체를 콘솔에 출력시키면, 내부함수의 실행 결과가 객체 형태로 출력된다.

객체는 내부에 두개의 프로퍼티를 가지고 있는데, 첫번째는 done 으로 제너레이터 함수의 실행완료 여부를 알려주는 값이 담겨있다.

아직 실행이 완료되지 않았기 때문에 이 값은 `false` 가 담겨있다.

그리고 두번째는 내부함수에서 `return` 한 값인 `리턴값1`이 담겨있어서 해당 값을 활용할 수 있게 된다.

```javascript
const 내부함수2_실행결과를_담은_변수 = 함수를_담은_변수.next();
console.log(내부함수2_실행결과를_담은_변수);
```

그리고 두번째로 실행하면, 방금 전 함수의 다음 함수인 두번째 함수를 실행한다.

```javascript
const 내부함수2_실행결과를_담은_변수 = 함수를_담은_변수.next();
console.log(내부함수2_실행결과를_담은_변수);
```

한 번 더 `next()` 를 실행해서 실행한 횟수가 내부함수의 갯수보다 더 많아지면, 에러가 나지는 않고 이미 종료된 함수이기 때문에 결과값으로 done이 `true`값으로 담겨있고 value값은 `undefined`로 비어있는 상태로 출력된다.

<br>

### Redux-saga

Redux-saga는 Redux의 미들웨어이다.

데이터의 상태 관리를 할 수 있는 redux의 비동기 처리를 위해 Redux-saga를 사용한다.

액션을 모니터링하고 있다가, 특정 액션이 발생했을 때 원하는 액션으로 dispatch되게 하거나 원하는 js 코드를 실행할 수 있다.

또한 비동기 작업 시 기존 요청의 취소 처리가 가능하며, API 요청이 실패 했을 때 재요청하는 작업도 할 수 있다.

Redux-saga를 사용할 때에는 [Generator문법](#h-generator)을 사용한다.

<br>

#### 설치

```npm
npm i redux-saga
```

redux-saga 를 사용하기 위해서는 redux, react-redux도 설치되어 있어야 한다.

만약 App에 redux가 설치되어 있지 않다면 아래와 같이 한번에 설치할 수도 있다.

```npm
npm i redux react-redux redux-saga
```

<br>

#### 파일 생성

src폴더에 redux 로 처리할 순수 함수들만 모아둘 redux 폴더를 만든다.

그리고 redux 폴더 내에 기능에 따라 아래 파일들을 생성한다.

순수 함수란 함수 외부에 직접적인 영향을 주지 않는, 부수효과를 발생시키지 않는 함수를 말한다.

redux 폴더 내부의 함수들은 DOM 또는 컴포넌트를 직접적으로 변경시키지 않는다.

오로지 프로퍼티, 메서드 등을 export 시킬 뿐, DOM 제어는 외부에서 해당 값들을 활용해 제어하도록 한다.

<br>

생성한 파일은 아래와 같다.

- actionType.js

- api.js

- reducers.js

- saga.js

- store.js

<br>

#### actionType.js

액션 타입을 일일이 문자열로 지정하지 않고 편리하게 사용할 수 있도록 객체로 설정한다.    

```javascript
export const 데이터 = {
  start: '데이터_START',
  success: '데이터_SUCCESS',
  fail: '데이터_FAIL'
}
```

사용 시 types 로 import 해서 `types.데이터.start`와 같은 형태로 사용한다.

<br>

#### api.js

실제 데이터를 요청 함수를 모아둔다.

axios 호출이 이 파일 내에서 이루어진다.

```javascript
import axios from "axios";

export const get데이터 = async ()=>{

  const url = 'API 주소';

  const params = {
    파라미터_키: 파라미터_값,
  };

  const 헤더 = {
    헤더_키: 헤더_값,
  }

  return await axios.get(url, { params, headers: 헤더});
}
```

API 문서에 맞게 aixos 로 호출한 수 export 한다.

<br>

#### reducers.js

action 값을 받아와서 해당 값에 맞는 처리를 하도록 하는 분기점이다.

store 에서 사용할 초기 데이터를 지정할 수 있다.

```javascript
import { combineReducers } from 'redux';
import * as types from './actionType'; 

// 초기 데이터를 state 에 저장했다가 action객체가 전달되면 type에 따라서 기존 데이터 변경
const 데이터Reducer = (state={데이터: []}, action)=>{
  switch(action.type){
		case types.데이터.start:
			return state;

		case types.데이터.success:
			return { ...state, 데이터: action.payload }

		case types.데이터.fail:
			return { ...state, 데이터: action.payload }

		default:
			return state;
  }
}

// 각각의 reducer 데이터를 하나로 합쳐서 반환
const reducers = combineReducers({ 데이터Reducer });

export default reducers;

```

<br>

#### saga.js

모니터링 중인 action 중 가로챌 요청과 실제로 실행할 작업을 설정하는 파일이다.

```javascript
import { takeLatest, all, put, fork, call } from 'redux-saga/effects';
import { getFlickr, getYoutube, getMembers } from './api';
import * as types from './actionType';

function* return데이터({옵션}) {
  try {
    const response = yield call(get데이터, 옵션);
    yield put({type: types.데이터.success, payload: response.data.데이터});
  } catch (error) {
    yield put({type: types.데이터.fail, payload: error});
  }
}

function* call데이터() {
  yield takeLatest(types.데이터.start, return데이터);
}

// store.js 에 의해 reducer에 미들웨어로 적용할 rootSaga 함수 생성
export default function* rootSaga(){
  yield all([fork(call데이터)]);
}
```

아래는 saga에서 사용하는 주요 메서드들이다.

- `takeLatest(액션이름, 함수)`    
첫번째 인자로 입력한 액션에 대해 이미 진행 중인 작업이 있으면 취소하고 가장 마지막으로 실행되었을 때만 두번째 인자의 함수를 실행한다.

- `takeEvery(액션이름, 함수)`    
첫번째 인자로 입력한 모든 액션에 대해 두번째 인자의 함수를 실행한다.

- `all([제너레이터1, 제너레이터2...])`    
`Promise.all()` 과 유사하다.    
인자에 배열로 입력된 제너레이터 함수들이 모두 resolve 될 때까지 기다린다.

- `call(호출함수, 옵션)` 
함수를 동기적으로 실행한다.   
주로 axios 함수를 호출할 때 사용한다.    
위 코드에서도 api.js 에서 생성한 함수를 호출했다.
두번째 인자로 파라미터 등 옵션값을 전달할 수 있다.

- `put({type: 액션, payload: 데이터})`
redux 의 `dispatch()` 와 같은 역할을 한다.

- `fork (saga명령어 실행 함수)`
`call()` 과 유사하게 함수를 실행하는 메서드이다.
`call()` 은 동기적으로 함수를 실행시키는 반면, `fork()` 는 비동기적으로 함수를 실행시킨다.
여러 개 결합 시 결합된 부모가 명령을 종료하면 진행중이던 작업을 취소하고 종료한다.

<br>

#### store.js

store 공간을 생성하고, saga에서 생성한 액션 객체를 reducer에 반영해서 store에 저장하도록 설정한다.

```javascript
import { createStore, applyMiddleware } from 'redux';
import reducers from './reducer';
```

redux 에서 미들웨어 실제 사용을 위해 `applyMiddleware` 메서드를 import 한다.

```javascript
import createSagaMiddleware from '@redux-saga/core';
import rootSaga from './saga';
```

적용할 saga파일 또한 Import한다.

```javascript
const sagaMiddleware = createSagaMiddleware();
const store = createStore(reducers, applyMiddleware(sagaMiddleware));
```

store 공간에 reducers와 함께 saga 미들웨어도 함께 연결해서 생성한다.

```javascript
sagaMiddleware.run(rootSaga);
export default store;
```

`run()` 으로 동적으로 saga를 실행한다.

<br>

#### store 사용

```javascript
import { Provider } from 'react-redux';
```

```jsx
<Provider store={store}>
  <App />
</Provider>
```

index.js 에서 redux 와 동일하게 Provider 로 store 를 전역에서 사용할 수 있도록 뿌려준다.

<br>

```javascript
import { useSelector } from 'react-redux';

const 데이터 = useSelector((store)=>{
  return (store.데이터Reducer.데이터);
});
```

데이터를 불러올 컴포넌트에서 `useSelector()` 로 데이터를 불러와 사용한다.

<br>

```javascript
import { useDispatch } from 'react-redux';
import * as types from './redux/actionType';
```

```javascript
const dispatch = useDispatch();

useEffect(() => {
  dispatch({
    type: types.데이터.start,
    옵션: 옵션객체,
  });
}, [])
```

store 내 데이터 값의 변경이 컴포넌트에서 `useDispatch()` 로 값을 변경한다.

미리 지정해 둔 actionType.js 의 객체도 import 해서 사용하면 오타도 방지되고 가독성도 높일 수 있다.

dispatch 를 App.js 에 적용해두면 전역에서 사용할 데이터의 초기값을 세팅해둘 수 있다.

<br>

