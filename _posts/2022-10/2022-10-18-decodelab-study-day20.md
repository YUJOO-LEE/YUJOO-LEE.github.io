---
layout: post
title:  validation (리액트), useHistory
date:   2022-10-18 21:52:02 +0900
comments : true
categories: Note
tags: [decodelab, react, validation, hook]
---

학원 수업 Day20

### validation

리액트 버전의 validation 을 제작했다.

<br>

#### 초기값 세팅

에러 체크를 위해 각 `input` 의 `value` 값을 저장해야한다.

폼 초기값 객체를 기본값으로 하는 `state` 로 생성한다.

```javascript
const initVal = {
  userId: '',
  email: '',
  pwd1: '',
  pwd2: '',
  gender: null,
  interests: null,
  edu: '',
  comments: '',
};
const [ val, setVal ] = useState(initVal);
```

그리고 에러 메세지와 validation 통과 후 `true` 값으로 전환하기 위한 `state` 도 생성해준다.

```javascript
const [ err, setErr ] = useState({});
const [ submit, setSubmit ] = useState(false);
```

<br>

#### value값 객체에 할당

```javascript
const handleChange = (e) => {
  // input[type=text] 시 value값 객체에 할당
  const { name, value } = e.target;
  setVal({...val, [name]: value});
}

const handleRadio = (e) => {
  // input[type=radio] 선택 시 true값 할당
  const { name } = e.target;
  const isChecked = e.target.checked;
  setVal({...val, [name]: isChecked});
}

const handleCheck = (e) => {
  // input[type=checkbox] 체크 시 true값, 체크 아웃 시 false 할당
  // 생각해보니 라디오도 그냥 이걸 쓰면 되지 않을까?
  const { name } = e.target;
  let isChecked = false;
  isChecked = e.target.checked ? true : false;
  setVal({...val, [name]: isChecked});
}

const handleSelect = (e) => {
  // select 선택 시 value값 객체에 할당
  const { name } = e.target;
  const isSelected = e.target.value;
  setVal({...val, [name]: isSelected});
}
```

**주의할 점** : `key` 값을 변수로 지정할때 대괄호로 묶는다.

<br>

#### 값 체크

값을 받아 에러 메세지를 담은 객체를 리턴한다.

```javascript
const checkVal = (value) => {
  const errs = {};

  if (value.userId.length < 5) {
    // input[type=text] 길이 확인
    errs.userId = '아이디를 5글자 이상 입력하세요.';
  }

  if (value.email.length < 8 || !/@/.test(value.email)) {
    errs.email = '이메일 형식에 맞게 8글자 이상 입력하세요.';
  }

  const testPwd = (val) => {
    const errType = [];
    // 에러 메세지 담을 빈배열 생성
    const reg = {
      '영문자': /[a-zA-z]/,
      '숫자': /[0-9]/,
      '특수문자': /[!@#$%^&*()_+]/,
    }
    // 에러 조건 정규식 지정
    if (val.length < 8) errType.push('8글자 이상');
    for (const curKey of Object.keys(reg)) {
      if (!reg[curKey].test(val)) errType.push(curKey);
    }
    // 에러 상황 발생시 메세지를 배열에 추가
    return errType.join(', ');
    // 에러 배열 합쳐서 문자열로 반환
  }
  if (testPwd(value.pwd1)) {
    // input[type=password] 조건이 길어서 별도 함수 처리
    errs.pwd1 = '형식에 맞게 입력하세요. (' + testPwd(value.pwd1) + ')';
  }

  if (value.pwd2 < 3 || value.pwd1 !== value.pwd2) {
    errs.pwd2 = '비밀번호를 동일하게 입력하세요.';
  }

  if (!value.gender) {
    // input[type=radio] 선택 여부 확인
    errs.gender = '성별을 선택하세요.';
  }

  // 기타 등등...
}
```

`submit` 이벤트로 객체에 저장된 값들을 체크해서 반환되는 에러 메세지를 객체로 넣어준다.

```javascript
const handleSubmit = (e) => {
  e.preventDefault();
  setErr(checkVal(val));
  setSubmit(true);
}
```

마지막으로 `submit` 에 `true` 값을 넣어준다.

```javascript
useEffect(() => {
  const len = Object.keys(err).length;
  if (!len && submit) {
    alert('회원 가입이 완료되었습니다. 메인 페이지로 이동합니다');
    history.push('/youtube');
  };
}, [])
```

에러 메세지 객체가 비어있고 `submit` 상태가 `true` 라면 성공 메세지를 띄우고 페이지를 이동시킨다.

지금은 만들어진 페이지가 youtube 페이지 밖에 없다.....

<br>

#### JSX 부분

```jsx
<input type="text"
  placeholder='아이디를 입력하세요'
  name="userId" id="userId"
  value={val.userId}
  onChange={handleChange}
/>
{err.userId && <span className='err'>{err.userId}</span>}
```

`input` 의 `value` 에 저장된 객체 내의 값을 적용시킴과 동시에 `onChange` 이벤트로 객체 내에 할당해줘서 실시간으로 이어서 수정될 수 있도록 한다.

에러 객체 내 해당하는 에러 메세지가 있다면 에러 메세지를 표시한다.

<br>

#### reset 버튼

```jsx
<button type='reset' onClick={handleReset}>RESET</button>
```

```javascript
const handleReset = () => {
  setVal(initVal);
  setErr({});
  setSubmit(false);
}
```

기존에 저장되었던 `state` 를 모두 초기값으로 돌려준다.

<br>

### useHistory()

```javascript
import { useHistory } from 'react-router-dom';

const history = useHistory();
```

URL 주소를 변경할 때 사용하는 Hook으로, 리액트는 url변경 없이 컴포넌트만 변경시킬 수 있지만 `useHistory()` 로 url 주소를 변경해서 사용자가 인식하도록 할 수 있다.

사용자 친화적 페이지 구축에 필요하다.

- Router v5 : `useHistory()`

- Router v6 : `useNavigate()`

라우터 버전이 올라가면서 해당 기능을 하는 Hook의 이름이 바뀌었다고 한다.

<br>

나머지 시간은 자습시간이었다.

청약닷컴 리액트 버전 제작중!

<br>

