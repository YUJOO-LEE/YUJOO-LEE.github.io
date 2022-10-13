---
layout: post
title:  학원 수업 내용 Day13
date:   2022-10-06 20:35:23 +0900
comments : true
categories: Note
tags: [decodelab, javascript, flickr, api, validation]
---


### [flickr 갤러리 적용](/decodelab/221006/layout_practice/gallery.html)

<br>

<iframe src='/decodelab/221006/layout_practice/gallery.html' frameborder='0' width='100%' height='500px'></iframe>

<br>

[어제 한 flickr API](/note/2022/10/05/decodelab-study-day12.html)를 기존에 만들었던 기업형 레이아웃에 적용했다.

새로운건 크게 없고 예외 처리에 대해 복습했다.

<br><br>
<hr>
<br><br>

### [회원가입 validation](/decodelab/221006/form/)

<br>

<iframe src='/decodelab/221006/form/' frameborder='0' width='100%' height='500px'></iframe>

<br>

**데이터 검증 요소**

1. text 길이

2. email @ 포함

3. 체크박스 선택

4. select 선택

5. 비밀번호 길이, 특수문자, 숫자 포함

함수 별 조건 만족 여부에 따라 `return true` / `return false` 이후, 에러 값 true 있으면 `e.preventDefault()`로 페이지 이동 막음.

모든 결과값이 false 여야 result.html로 이동

<br>

개인적으로 함수 패키징 및 재사용에 중점을 두고 작업했다.

비밀번호는 다중 조건이므로 에러 조건을 개별로 표시될 수 있도록 추가했다.

<br><br>

#### 비밀번호 확인

<br>

```javascript
const pwError = checkPw('pw1', 6);
if (pwError) {
  e.preventDefault();
  errorMsg('pw1', `비밀번호 양식에 맞게 입력하세요. (${pwError.join(', ')})`);
}
```

<br><br>

#### 데이터 검증

<br>

```javascript
const errReason = [];
if (txt.length < len) errReason.push(`${len}글자 이상`)

const regexp = {
  '숫자 포함': /[0-9]/,
  '영문자 포함': /[a-zA-Z]/,
  '특수문자 포함': /[~!@#$%^&*()_+?<>₩]/
};
for (let exp of Object.keys(regexp)) {
  if (!regexp[exp].test(txt)) errReason.push(exp);
}
```

<br><br>

#### 에러 메세지 출력

<br>

```javascript
function errorMsg(name, msg) {
  const el = $form.querySelector(`[name=${name}]`);
  const errMsg = document.createElement('p');
  errMsg.innerText = msg;
  el.closest('td').append(errMsg);
}
```

<br> 

