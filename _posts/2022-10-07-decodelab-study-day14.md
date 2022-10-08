---
layout: post
title:  학원 수업 내용 Day14
date:   2022-10-07 19:32:23 +0900
comments : true
categories: Note
tags: [decodelab, javascript]
---


### [비밀번호 validation](/decodelab/221007/validation/)

<br>

<iframe src='/decodelab/221007/validation/' frameborder='0' width='100%' height='500px'></iframe>

<br>

#### 입력 비밀번호 보여주기

```javascript
$toggleBtn.addEventListener('click', ()=>{
  if ($pwd.type === 'password') {
    $pwd.type = 'text';
    $toggleBtn.classList.add('hide');
  } else {
    $pwd.type = 'password';
    $toggleBtn.classList.remove('hide');
  }
})
```

<br>

#### 조건별 검증

```javascript
$pwd.addEventListener('keyup', ()=>{
  checkPw($pwd.value);
})

function checkPw(data) {
  const lower = new RegExp('(?=.*[a-z])');
  const upper = new RegExp('(?=.*[A-Z])');
  const number = new RegExp('(?=.*[0-9])');
  const spercialChar = new RegExp('(?=.*[!@#\$%\^&\*])');
  const length = new RegExp('(?=.{8,})');

  setClass($lowerCase, lower.test(data));
  setClass($upperCase, upper.test(data));
  setClass($number, number.test(data));
  setClass($specialChar, spercialChar.test(data));
  setClass($length, length.test(data));
}

function setClass(el, show = false) {
  if(show) {
    el.classList.add('valid');
  } else {
    el.classList.remove('valid');
  }
}
```

<br>

유용하게 쓰일 것 같다.

<br><br>
<hr>
<br><br>

### [placeholder 대신 span 사용](/decodelab/221007/login/)

<br>

<iframe src='/decodelab/221007/login/' frameborder='0' width='100%' height='500px'></iframe>

<br>

placeholder는 다국어 번역기에서 번역되지 않고, 내용이 입력되면 지워져서 힌트를 보기 어렵다는 단점이 있다.

span으로 빼서 입력시에도 볼 수 있도록 인풋 상단에 위치시켰다.

<br><br>
<hr>
<br><br>

### [skip navi](/decodelab/221007/skip_navi/)

<br>

<iframe src='/decodelab/221007/skip_navi/' frameborder='0' width='100%' height='500px'></iframe>

<br>

이벤트 리스너 `focusin`, `focusout` 활용.

웹 접근성을 고려하여 페이지 내에서 tab 키 사용 시 최상단에 "본문 바로가기" skip navi가 나오도록 한다.

마우스를 사용할 수 없는 상황에서도 페이지마다 콘텐츠가 반복되는 영역을 건너뛸 수 있다.

<br><br>
<hr>
<br><br>

### [cookie 활용](/decodelab/221007/cookie/)

<br>

<iframe src='/decodelab/221007/cookie/' frameborder='0' width='100%' height='500px'></iframe>

<br>

**쿠키**

- 클라이언트 자원 사용

-- name: 식별자    
-- value: 값    
-- domain: 사이트 도메인    
-- path: 사이트 내 경로    
-- expires: 만기 날짜    

실행 시 서버에서 클라이언트에 저장된 쿠키 확인 후 팝업 div에 스타일 부여.

<br>

```javascript
function setCookie(name, value, due) {
  const today = new Date(); // 현재 날짜 시간 생성
  const date = today.getDate(); // 생성된 날의 '일' 사용
  today.setDate(date + due);  // 원하는 날짜로 바꿔서 쿠키 유효기간 설정 (오늘부터 n일 뒤)
  const duedate = today.toGMTString();  // 인간이 알아볼 수 있는 문자로 변환

  document.cookie = `${name}=${value}; path="/"; expires=${duedate}`;
}
```

<br>

```javascript
setCookie('today', 'done', 1);  // 쿠키 생성
setCookie('today', 'done', 0);  // 쿠키 삭제 (유효기간 현재로 설정하면 지워지도록 활용)
```

<br>
