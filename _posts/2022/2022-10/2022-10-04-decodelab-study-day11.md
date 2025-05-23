---
layout: post
title:  iframe, youtube API, swiper.js 활용
date:   2022-10-04 22:42:23 +0900
comments : true
categories: Note
tags: [decodelab, javascript, youtube, api]
---

학원 수업 Day11

### iframe관련

<br>

```html
<iframe src="#url1" name="site" frameborder="0">이 브라우저는 iframe을 지원하지 않습니다.</iframe>
<a href="#url2" target="site">쿠팡</a>
```

<br>

#### name 속성

- 주 사용 : name속성은 form 전송 이벤트 발생 시 서버로 데이터를 전송하기 위한 식별자

- 파라미터를 전송하기 위해 지정하는 것으로 중복 가능

<br>

#### id 속성

- name과 비슷한 용도로 사용이 가능하지만 고유값을 가져서 한번만 사용이 가능

- 주 사용 : 자바스크립트에서 다루기 위한 목적으로 지정

<br>

#### iframe

- inline 요소

- html 웹문서 안에 또 다른 웹문서를 넣을 수 있으며, 보통 동영상을 불러올 때 사용

- 지원하지 않는 브라우저가 많아 대체될 내용을 넣어주는 것이 좋음

- 기존의 iframe에서 사용하던 속성은 html5가 되면서 지원을 안하는 경우가 많음


<br><br>
<hr>
<br><br>

### 이벤트 버블링

브라우저는 특정 화면 요소에서 이벤트가 발생하였을 때, 그 이벤트를 최상위에 있는 화면 요소까지 이벤트를 전파시킨다.

<br>

#### 이벤트 캡쳐

버블링과 반대방향으로 진행되는 전파방식

<br>

#### 이벤트 위임

하위 요소에 각각 이벤트를 붙이지 않고 상위요소에서 하위 요소의 이벤트를 제어하는 방식

<br>

```javascript
const lis = document.querySelectorAll('li');

lis.forEach((el)=>{
  el.addEventListener('click', ()=>{
  })
})
```

<br>

위 코드 사용 시 li가 100개이면 이벤트 리스너를 100개 붙여야 하기 때문에 이벤트 위임을 이용해 상위 요소인 ul에 이벤트 리스너를 부여해서 하위 요소에 전가시켜야 함.

이후 li가 얼마나 추가가 되던지 따로 부여하지 않아도 알아서 이벤트가 하위 요소에 전가되어 부여됨.

<br>

```javascript
const ul = document.querySelector('ul');

ul.addEventListener(('click'), ()=>{
  console.log('click');
})
```

<br><br>
<hr>
<br><br>

### [유튜브 API 활용](/decodelab/221004/thumbnail/)

<br>

<iframe src='/decodelab/221004/thumbnail/' frameborder='0' width='100%' height='500px'></iframe>

<br>

[구글 API키](https://console.cloud.google.com/apis/credentials)를 받아 유튜브 사이트 내 정보를 받아올 수 있다.

fetch를 사용할 땐 두 단계를 거쳐야 한다. 

1. 올바른 url로 요청을 보냄.

2. 뒤에오는 응답에 대해 json()을 해줘야 함.

<br>

```javascript
const url = `https://www.googleapis.com/youtube/v3/playlistItems?part=snippet&key=${key}&playlistId=${playlist}&maxResults=${resultNum}`;

fetch(url)
  .then((data)=>{ // url에서 정보 받아오면 실행
    return data.json(); // json 본문 콘텐츠 추출
  })
  .then((json)=>{ 
    // 추출한 json 객체 활용
  })
```

<br><br>
<hr>
<br><br>

### [기업형 레이아웃](/decodelab/221004/layout_practice/youtube.html)

<br>

<iframe src='/decodelab/221004/layout_practice/youtube.html' frameborder='0' width='100%' height='500px'></iframe>

<br>

앞서 학습한 유튜브 API를 이용한 갤러리 제작

<br><br>
<hr>
<br><br>

### [슬라이드 예제](/decodelab/221004/review/)

<br>

<iframe src='/decodelab/221004/review/' frameborder='0' width='100%' height='500px'></iframe>

<br>

swiper.js를 활용한 슬라이드 예제 제작

<br>