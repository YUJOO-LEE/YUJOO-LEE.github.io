---
layout: post
title:  학원 수업 내용 Day15
date:   2022-10-11 17:36:23 +0900
comments : true
categories: Note
tags: [decodelab, javascript, kakaomap, api, react]
---

오전 시간에는 카카오 맵 API 다루기,

오후 시간에는 드디어 리액트를 시작했다!

<br><br>
<hr>
<br><br>

### 구조분해(비구조화) 할당

Destructuring assignment

배열을 분해해서 각각 값을 대입한 변수를 선언하는데, 한 줄 한 줄 지정하는 게 아니라 리터럴 모양으로 선언할 수 있다.

<br>

#### 배열

<br>

```javascript
const arr = ['red', 'green', 'blue'];

const red = arr[0];
const green = arr[1];
const blue = arr[2];
// 이렇게 하나씩 선언하는 것이 아니라 아래 한 줄로 표현가능함.

const [red, green, blue] = arr;
```

<br>

배열이기 때문에 순서만 맞으면 변수 명은 임의로 지정이 가능하다.

<br><br>

#### 객체

<br>

```javascript
const student = {
  name: 'David',
  age: 20,
  address: 'seoul'
}

const {age, ...rest} = student;
```

<br>

순서는 상관없으나 key 값이 맞게 가져와야 한다.

...rest는 선택한 프로퍼티 외 나머지들을 rest 배열으로 모은다는 뜻이다.

<br>

```javascript
const {age:studentAge, ...rest} = student;
```

<br>
 
위와 같이 key를 식별자로 사용하는 것이 아니라 별도의 식별자 지정 또한 가능하다.

<br><br>
<hr>
<br><br>

### 전개 연산자

spread operator

<br>

```javascript
const arr = [1,2,3];
const arr2 = arr;

arr[0] = 0;
// arr2[0]도 0으로 바뀜
```

<br>

배열, 객체와 같은 참조형 데이터는 직접 대입 시 연결해서 참조를 할 뿐 복사가 되지 않는다.

한쪽 배열의 일부 값을 바꾸면, 연결된 배열의 값 또한 바뀐다.

<br>

```javascript
const arr3 = [...arr];

arr3[0] = 100;
// arr[0]은 바뀌지 않음
```

<br>

전개 연산자를 사용해서 얕은 복사를 할 수 있다.

이 때 두 배열은 같은 값을 가진 것 같지만 별도의 배열으로 생성되어 배열끼리는 연결되어 있지 않다.

<br>

```javascript
const arr4 = [...arr, 4];

const arr5 = [...arr, ...arr4];
```

<br>

복사 시 새로운 값을 추가하거나, 두 개의 배열을 합칠 수도 있다.

<br><br>
<hr>
<br><br>

### [기업형 레이아웃 내 카카오맵 적용](/decodelab/221011/layout_practice/location.html)

<br>

<iframe src='/decodelab/221011/layout_practice/location.html' frameborder='0' width='100%' height='500px'></iframe>

<br>

단순히 맵만 갖다 넣는게 아니라 객체 배열을 만들어서 여러 개의 마커를 생성하고, 버튼 클릭 시 해당 위치로 중심을 이동하게 했다.

<br>

```javascript
// 브라우저 리사이즈 시 지도 중심 이동
window.addEventListener('resize', ()=>{
  const activeLi = document.querySelector('.branch li.on');
  const activeIndex = activeLi.dataset.index;
  moveTo(markerOptions[activeIndex].latLng);
})
```

<br>

다른 부분은 필요에 따라 활용하겠지만, 이부분은 항상 쓰일 것 같아서 별도 메모.

<br><br>
<hr>
<br><br>

### 리액트

<br>

기존에는 HTML 파일에 DOM 요소를 작성하고 제어했지만, REACT는 데이터가 변화할 때 변경사항이 적용 된 가상의 DOM요소를 만들어서 실제 DOM과 비교 후 변경된 부분을 실제 DOM에 적용한다.

동적인 웹에서 성능이 빨라서 여러 사이트에서 이용하고 있다.

<br><br>
<hr>
<br><br>

#### 설치 

<br>

```npx
npx create-react-app 폴더명
```

<br>

#### 실행

<br>

```npm
npm start
```

<br>

#### 빌드

```npm
npm run build
```

<br><br>
<hr>
<br><br>

#### 이미지 삽입

1. import image변수명 from '경로';    
필요한 자리에 변수명으로 불러옴

2. 필요한 자리에서 `process.env.PUBLIC_URL`로 public의 절대 경로를 찾아 그 안에 있는 파일 사용

<br>

#### 주의사항

- 내보내는 요소 식별자 첫번째 글자 대문자 필수

- return은 하나의 요소만 가능하므로 fragment <></> 감싸서 내보내기

- 하나만 내보낼 경우    
`export default A;`    
-- 하나 받을 경우    
`import A from './components/Layout';`

- 여러개 내보낼 경우 객체로 리턴    
`export { A, B };`    
-- 받아올때도 객체로 받아옴    
`import { A, B } from './components/Layout';`

- 받아온 요소 삽입    
`<A />`

- class 사용 시 className으로 사용

- style 속성은 객체 형태로 사용

<br><br>

