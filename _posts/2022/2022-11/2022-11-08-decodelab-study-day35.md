---
layout: post
title:  firebase 활용 회원가입, 로그인 구현
date:   2022-11-08 10:33:25 +0900
comments : true
categories: Note
tags: [decodelab, firebase, terminal]
---

학원 수업 Day35

웹사이트 규모가 작을 경우 가입된 회원의 데이터 관리는  자체 DB에서 하는 것보다 firebase를 이용해 구글에서 관리하게 하는 것이 보안 등의 측면에서 편리하다.

<br>

### firebase

firebase 를 react 폴더에서 설치한다.

```terminal
npm i firebase
```

firebase 를 사용하기 위해 src 폴더에 firebase.js 파일을 생성한다.

```javascript
//firebase추가
import firebase from 'firebase/compat/app';
//firebase auth 추가
import 'firebase/compat/auth';

const firebaseConfig = {
  // APP_인증_정보
};

//firebase 초기화
firebase.initializeApp(firebaseConfig);

//firebase 내보내기
export default firebase;
```

<br><br>
<hr>
<br><br>


### redux-toolkit

로그인 상태를 redux-toolkit 으로 관리하기 위해 react 폴더에서 npm 명령어를 이용해 redux-toolkit 과 react-redux 를 설치한다.

```terminal
npm i react-redux @reduxjs/tookit
```

<br>

Slice 파일을 생성해서 `createSlice()` 메서드로 초기값과 리듀서를 생성한다.

```javascript
import { createSlice } from '@reduxjs/toolkit';
```

먼저 `createSlice` 를 `import` 시킨다.

```javascript
const userSlice = createSlice({
  name: 'user',
  initialState: {
    displayName: '',
    uid: '',
    accessToken: ''
  },
  reducers: {
    loginUser: (state, action)=>{
      state.displayName = action.payload.displayName;
      state.uid = action.payload.uid;
      state.accessToken = action.payload.accessToken;
    },
    logoutUser: state=>{
      state.displayName = '';
      state.uid = '';
      state.accessToken = '';
    }
  }
});
```

createSlice() 로 인스턴스의 이름, 초기값, reducers 를 작성한다.

데이터를 통신으로 받아와서 사용하는 것이 아닌 고정값을 사용할 것이기 때문에 extraReducers가 아닌 일반적인 reducers로 작성한다.

로그인 시 정보를 저장할 메서드 loginUser 와 로그아웃 시 정보를 비울 logoutUser 메서드를 생성했다.

<br>

store 내의 정보를 받아오기 위해 index.js 에서 `configureStore()` 메서드로 스토어를 생성해 Provider 로 App 을 감싸 store 를 전송한다.

```javascript
import { Provider } from 'react-redux';
import { configureStore } from '@reduxjs/toolkit';
import userSlice from '만들어둔 slice 파일경로';

const store = configureStore({
	reducer: {
		user: userSlice
	},
	middleware: (getDefaultMiddleware)=>getDefaultMiddleware({serializableCheck: false})
})
```

```jsx
<Provider store={store}>
  <App />
</Provider>
```

<br><br>
<hr>
<br><br>

### Join

1차적으로 구글에 실제 비밀번호와 가입 정보를 저장한 후, 게시글 작성에 필요한 최소한의 정보를 MongoDB에 저장한다.

먼저 유저 별 고유번호를 생성하기 위해 DB 의 counter document 에서 userNum 을 초기값 1로 하여 추가한다.

<br>

#### back-end

node 폴더의 index.js 에서 user 라우터를 지정한다.

```javascript
app.use('/api/user', require('./router/userRouter.js'))
```

기존에 생성해두었던 counterSchema.js 에서 userNum 의 데이터 타입을 Number 로 정의한다.

```javascript
const counterSchema = new mongoose.Schema({
  name: String,
  communityNum: Number,
  userNum: Number,  // userNum 추가
}, { collection: 'Counter'});
```

node/router 폴더에 userRouter.js 를 생성한다.

저장하는 로직은 [게시글 작성](/note/2022/11/04/decodelab-study-day33.html)과 같다.

DB에 직접 추가한 userNum 값을 받아와서 프론트로부터 넘겨받은 데이터에 추가한 뒤, counter 의 userNum 값을 1 더하고 저장한 결과값을 프론트로 넘겨준다.

```javascript
const express = require('express');
const router = express.Router();
const { User } = require('../model/userSchema');
const { Counter } = require('../model/counterSchema');

router.post('/join', (req, res)=>{

  Counter.findOne({name: 'counter'})
    .exec()
    .then(doc=>{
      const temp = req.body;
      temp.userNum = doc.userNum;
      const userData = new User(temp);

      userData.save()
      .then(()=>{
        Counter.updateOne({name: 'counter'}, {$inc: {userNum: 1}})
          .then(()=>{
            res.json({success: true})
          })
      })
      .catch(err=>{
        console.log(err);
        res.json({success: true})
      })
    })
});

module.exports = router;
```

가입 정보 저장용 스키마파일인 userSchema.js 파일을 생성한다.

```javascript
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  userNum: Number,
  email: String,
  displayName: String,
  uid: String
}, {collection: 'Users'});

const User = mongoose.model('Users', userSchema);
module.exports = { User };
```

userNum 은 자체적으로 관리하기 위한 값이며 uid 는 firebase에서 부여한 고유 id 값이다.

<br>

#### front-end

join.js 파일을 만들어서 회원가입 페이지를 작성한다.

```javascript
import { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import firebase from '../firebase';
```

input 정보를 담을 State 와, 가입 취소 또는 저장 성공 후 페이지 이동을 일으키기 위해 `useNavigate()` 를 활성화 시켜준다.

```javascript
const navigate = useNavigate();
const [ Email, setEmail ] = useState('');
const [ Pwd1, setPwd1 ] = useState('');
const [ Pwd2, setPwd2 ] = useState('');
const [ Name, setName ] = useState('');
```

jsx로 실제 가입 폼을 만든다.

input value 로 State 를 넣어주고, onChange 이벤트로 State 를 변경해준다.

```jsx
<input type='email' value={Email} 
  placeholder='이메일 주소를 입력하세요' 
  onChange={e=> setEmail(e.target.value)}
  autoComplete='email'
/>
<input type='password' value={Pwd1} 
  placeholder='비밀번호를 입력하세요' 
  onChange={e=> setPwd1(e.target.value)}
  autoComplete='new-password'
/>
  <input type='password' value={Pwd2} 
  placeholder='비밀번호를 재입력하세요' 
  onChange={e=> setPwd2(e.target.value)}
  autoComplete='new-password'
/>
<input type='text' value={Name} 
  placeholder='이름을 입력하세요' 
  onChange={e=> setName(e.target.value)}
/>
<button onClick={()=>navigate(-1)}>Cancel</button>
<button onClick={handleJoin}>Join</button>
```

취소버튼 클릭 시 이전 페이지로 이동하고, 가입버튼 클릭시 가입 핸들 이벤트로 연결한다.

그리고 핸들 이벤트를 작성한다.

먼저 입력된 내용이 비어있거나, 비밀번호를 맞게 입력했는지 검증절차를 거친 후 async await 로 firebase 와 통신해서 저장 후 로그인 페이지로 이동시킨다.

```javascript
if (!(Name && Email && Pwd1 && Pwd2)) {
  return alert('모든 양식을 입력하세요.');
}
if (Pwd1 !== Pwd2) {
  return alert('비밀번호를 동일하게 입력하세요.');
}

// 입력 조건 통과 후 저장
// async await 로 firebase 와 통신해서 인증처리
const createdUser = await firebase.auth().createUserWithEmailAndPassword(Email, Pwd1);
await createdUser.user.updateProfile({
  displayName: Name
})
```

Email 과 비밀번호 입력으로 가입정보를 저장하기 위해 firebase 의 `createUserWithEmailAndPassword()` 메서드를 사용해서 해당 값을 넘겨 신규 유저를 생성하고, 그 후에 `updateProfile()` 메서드로 displayName 을 업데이트 한다.

업데이트 직후, 해당 저장값을 다시 객체에 담아서 자체 DB에 저장하고 메세지 출력 후 로그인 페이지로 이동한다.

가입 후 자동 로그인이 안되고 직접 로그인 하도록 유도하기 위해 DB 저장 전에 firebase 및 store 로그인 상태를 초기화한다.

```javascript
// firebase로 부터 인증 정보값을 받아 객체에 담기
const item = {
  email: createdUser.user.multiFactor.user.email,
  displayName: createdUser.user.multiFactor.user.displayName,
  uid: createdUser.user.multiFactor.user.uid,
}

firebase.auth().signOut();
dispatch(logoutUser());

axios.post('/api/user/join', item).then(res=>{
  if (res.data.success) {
    alert('회원가입에 성공했습니다.');
    navigate('/login');
  } else {
    return alert('회원가입에 실패했습니다.');
  }
})
```

<br><br>
<hr>
<br><br>

### Login

```javascript
const navigate = useNavigate();
const [ Email, setEmail ] = useState('');
const [ Pwd, setPwd ] = useState('');
const [ Err, setErr ] = useState('');
```

정보 전달을 위한 State 와 페이지 이동을 위한 `useNavigate()` 를 `import` 해준 후 초기값을 생성한다.

그리고 jsx로 input 및 button 을 생성하고 이벤트를 부여한다.

에러 메세지를 출력하기 위한 요소도 추가한다.

```jsx
<input type='email' value={Email} 
  placeholder='이메일 주소를 입력하세요.' 
  onChange={e=>setEmail(e.target.value)}
  autoComplete='email'
/>
<input type='password' value={Pwd} 
  placeholder='비밀번호를 입력하세요.' 
  onChange={e=>setPwd(e.target.value)}
  autoComplete='current-password'
/>
<button onClick={handleLogin}>Login</button>
<button onClick={()=>navigate('/join')}>Join</button>
{Err && 
  <p>
    {Err}
  </p>
}
```

로그인 이벤트 핸들을 작성한다.

공백 여부를 확인 후 firebase 의 `signInWithEmailAndPassword()` 메서드로 인증한 후 메인페이지로 이동시킨다.

에러메세지 출력을 위해 `catch` 문으로 넘어오는 `err.code` 에 따른 메세지를 State 에 담는다.

```javascript
const handleLogin = async ()=>{
  if (!(Email && Pwd)) {
    return alert('이메일과 비밀번호를 입력하세요.');
  }

  try{
    await firebase.auth().signInWithEmailAndPassword(Email, Pwd);
    navigate('/');
  } catch (err) {
    console.log(err.code);
    if (err.code === 'auth/invalid-email') {
      setErr('이메일 형식을 확인하세요.')
    } else if (err.code === 'auth/user-not-found') {
      setErr('존재하지 않는 이메일 입니다.')
    } else if (err.code === 'auth/wrong-password') {
      setErr('비밀번호 정보가 일치하지 않습니다.')
    } else {
      setErr('로그인에 실패했습니다.')
    }
  }
}
```

<br><br>
<hr>
<br><br>

### login 출력, logout 처리

Header에서 로그인 시 로그인/회원가입 버튼을 지우고 로그인 상태 및 로그아웃 버튼을 출력하기 위해 아래 메서드들을 `import` 한다.

```javascript
import { useDispatch, useSelector } from 'react-redux';
import { logoutUser } from '../redux/userSlice';
import firebase from '../firebase';
```

store 에 저장된 유저 정보를 받아온다.

```javascript
const User = useSelector(store=> store.user);
```

store 에 저장된 유저 정보의 유무에 따른 조건으로 화면을 출력한다.

로그아웃 버튼에는 로그아웃 이벤튼 핸들을 연결한다.

```jsx
{User.accessToken 
? 
  <>
    {User.displayName} 님 환영합니다. 
    <Link onClick={handleLogout}>Logout</Link>
  </>
:
  <>
    <NavLink to='/login'
      style={({isActive})=> isActive ? activeStyle : null}>
      Login
    </NavLink>
    <NavLink to='/join'
      style={({isActive})=> isActive ? activeStyle : null}>
      Join
    </NavLink>
  </>
}
```

로그아웃 핸들 이벤트를 작성한다.

a 태그를 사용해서 `e.preventDefault()` 를 해주었는데, 다른 태그라면 불필요할 것이다.

firebase의 `signOut()` 메서드로 로그아웃 처리를 하고 dispatch() 로 logoutUser() 함수를 호출해서 store를 비워준다.

```javascript
const handleLogout = (e)=>{
  e.preventDefault();
  firebase.auth().signOut();
  dispatch(logoutUser());
  alert('로그아웃 되었습니다.');
  navigate('/');
}
```

로그아웃 메세지 출력 후 페이지를 이동시킨다.

<br>
