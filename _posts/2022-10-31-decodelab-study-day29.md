---
layout: post
title:  학원 수업 내용 Day29
date:   2022-10-31 23:52:16 +0900
comments : true
categories: Note
tags: [decodelab, react, redux, terminal]
---


### redux-toolkit

redux-toolkit은 redux-saga에 비해 간편하게 상태관리를 할 수 있다.

redux 나 redux-saga 를 사용하면서 많은 파일, 함수들을 만들었던 것에 비해 `createAsyncThunk()`, `createSlice()` 만을 사용해서 개별 Slice 파일만 만들면 된다.

설치 또한 간편하다.

```npm
npm i @reduxjs/toolkit
```

한번만 설치하면 redux, redux-thunk, redux-saga가 모두 설치된다.

react 에서 사용을 위해 react-redux 만 별도로 설치하면 된다.

<br>

#### 사용 방법

파일은 호출할 데이터 별 하나만 생성하면 된다.

src/redux 경로에 데이터Slice.js 라는 이름으로 생성했다.

그리고 최상단에 필요한 메서드를 불러온다.

```javascript
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';
import axios from 'axios';
```

<br>

#### createAsyncThunk()

fetch 함수를 만들어서 async 함수를 수행한다.

`createAsyncThunk(액션 타입, 프로미스 반환하는 비동기 함수, 옵션)`

데이터를 createSlice로 보내서 상태를 관리하기 때문에 여기에서는 리듀서를 작성하지 않는다.

```javascript
export const fetch데이터 = createAsyncThunk(
  // 고유 문자값 등록 (내부적으로 actionType 생성 시 활용)
  '데이터/request데이터',
  // 비동기 데이터 호출 함수
  async (파라미터)=>{
    const response = await axios.get(데이터_URL);
    return response.data.데이터;
  }
);
```

<br>

#### createSlice()

리듀서 함수가 있는 객체 'slice' 를 반환한다.

정의된 리듀서를 기반으로 액션 타입 문자열과 액션 생성자 함수를 생성한다.

```javascript
const 데이터Slice = createSlice({
  // 내부적으로 Reducer 생성 시 관리할 데이터가 들어갈 프로퍼티
  name: '데이터',
  initialState: { // 기본값
    data: [],
    isLoading: false,
  },
  extraReducers: {
    [fetch데이터.pending]: (state)=>{
      state.isLoading = true;
    },
    [fetch데이터.fulfilled]: (state, action)=>{
      state.isLoading = false;
      state.data = action.payLoad;
    },
    [fetch데이터.rejected]: (state)=>{
      state.isLoading = false;
    }
  }
});
```

3가지의 상태를 제공한다.

- pending    
데이터 요청 전 상태로, 로딩 상태 등 설정할때 사용한다.

- fulfilled    
데이터 요청 후 데이터를 가져오는데 성공한 상태이다.    
가져온 데이터를 payload 를 통해 store 에 저장한다.

- rejected    
데이터를 요청했으나 가지고 오는데 실패한 상태이다.    
데이터를 저장할 수는 없지만, 요청은 이미 종료되었으므로 로딩 상태는 해제시켜 주도록 한다.

<br>

#### store 생성

```javascript
import { configureStore } from '@reduxjs/toolkit';
import 데이터Reducer from './redux/데이터Slice';

const store = configureStore({
	reducer: {
		데이터: 데이터Reducer,
	}
})
```

리듀서를 store 에 담아 사용하기 위해 Slice 파일으로부터 `export default` 한 리듀서를 index.js 에 불러온다.

```javascript
import { Provider } from 'react-redux';
```

```jsx
<Provider store={store}>
  <App />
</Provider>
```

기존과 마찬가지로 App 컴포넌트를 `Provider` 로 감싸서 store를 전역에서 사용할 수 있도록 한다.

<br>

#### dispatch

기존과 동일한 방식으로 dispatch를 적용하는데 Slice 에서 생성한 fetch함수를 이곳에서 사용한다.

```javascript
import { fetch데이터 } from './redux/데이터Slice';
```

fetch함수를 사용하기 위해 Slice 파일으로부터 `import` 한다.

```javascript
import { useDispatch } from 'react-redux';
import { useEffect } from 'react';
```

마운트 될 때 dispatch 시켜야 하므로 `useDispatch()` 와 `useEffect()` 도 `import` 한다.

```javascript
const dispatch = useDispatch();
useEffect(()=>{
  dispatch(fetch데이터(옵션객체));
}, []);
```

<br>

#### store 불러오기

컴포넌트에서 store를 불러오는 방법 또한 기존과 동일하게 `useSelector()` 를 사용한다.

```javascript
import { useSelector } from 'react-redux';

const 데이터 = useSelector(store=>store.데이터.data);
```

<br>

