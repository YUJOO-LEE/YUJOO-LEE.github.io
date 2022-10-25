---
layout: post
title:  청약닷컴 UI제작로그 Day3
date:   2022-10-20 23:29:03 +0900
comments : true
categories: Note
tags: [cheongyak.com, react, portfolio, scss]
---

청약닷컴 페이지 만들면서 있었던 좌충우돌 제작로그

<br>

### async / await

axios 로 JSON 데이터를 불러올 때 `promise.then` 를 사용했었는데 새로운 방법을 배웠다.

바로 `async` 와 `await`.

`async` 는 Promise 객체를 반환해주고, await는 Promise 완료 후 이어서 실행되도록 하는 짝궁같은 친구들이다. 

```javascript
axios.get(`${baseUrl}/파일명.json`).then((json)=>{
  setList(json.data);
})
```

기존에 이렇게 사용하던 걸 아래와 같이 바꿔주었다.

```javascript
async function fetchData() {
  const result = await axios.get(`${baseUrl}/파일명.json`);
  setList(json.data);
}
fetchData();
```

사실 아직 무슨 차이인지는 모르겠고 일단 사용만 해봤다.

따로 있는 걸 보면 분명 무언가 차이가 있을 것 같긴 하다.

`function` 이랑 화살표 함수처럼...

나중에 공부해봐야지.

<br>

### useRef()

```javascript
const baseUrl = process.env.PUBLIC_URL;

useEffect(()=>{
  async function fetchData() {
    const result = await axios.get(`${baseUrl.current}/파일명.json`);
    setList(json.data);
  }
  fetchData();
}, [])
```

위 상황에서는 baseUrl을 불러올 수 없다.

undefined 라고 출력이 된다.

개인적인 느낌으로는 axios가 비동기로 실행되게 되면서 이미 종료된 실행 컨텍스트에 있는 변수를 불러오지 못하는게 아닐까 하는데...

확실하진 않아서 모르겠다.

아무튼 baseUrl 변수를 계속 살려둘 필요가 있어서 아래와 같이 해결했다.

```javascript
const baseUrl = useRef(process.env.PUBLIC_URL);
```

`useRef()`는 virtual DOM 요소를 참조해오기 위해 사용하기도 하지만, 랜더링 시키지 않을 값을 저장하는 용도로도 쓰인다.

컴포넌트의 라이프 사이클이 종료되기 전 까지는 임의로 삭제하지 않는 한 삭제되지 않는 값이라고 한다.

<br>

### 부드러운 스크롤

컨텐츠 페이지 내용을 탭으로 분리하려다가, 내용이 그렇게 많지 않아서 한페이지에 다 넣고 버튼을 클릭하면 해당 내용으로 이동하는 이벤트를 만들고자 했다.

```javascript
Y좌표 = 엘리먼트.offsetTop;

window.scrollTo(X좌표, Y좌표);
```

offsetTop은 해당 엘리먼트의 위치가 시작되는 T좌표 값이다.

scrollTo 메서드는 스크롤을 해당 좌표로 이동시키도록 한다.

그래서 인수 자리에 받아온 좌표를 넣어주면 해당 엘리먼트로 이동시켜준다.

다만 이동이 한번에 이뤄지기 때문에, 그것보다는 부드럽게 이동되는 편이 내용이 한 페이지 안에 있다는 직관적인 느낌을 줄것이라고 판단했다.

자 어떻게 부드럽게 처리할까?

오늘의 삽질이 시작되었다.

<br>

부드러운 움직임을 보여주기 위해서는 움직이는 과정을 하나씩 보여줘야 한다.

마치 노트에 비슷한 그림 여러장 그려놓고 빠르게 촤르륵 보여주면 움직이는 것처럼 보이는 것과 비슷하다.

`setInterval()` 로 스크롤이 조금씩 이동하는 함수를 실행시킬 수도 있겠지만 setInterval은 브라우저가 화면 프레임을 리페인트하는 시간과 별개로 반복을 돌게 되므로 끊김이 발생할 수 있다고 한다.

```javascript
window.requestAnimationFrame()
```

그래서 이용하는 것이 `requestAnimationFrame()` 메서드이다.

인수로 함수를 넣으면 해당 함수를 프레임 생성에 맞춰서 실행시켜주게 되므로 화면에서 뿌려줄 수 있는 프레임에 각기 다른 값을 하나씩 넣어주게 되면 부드럽고 자연스러운 모션으로 보일 것이다.

```javascript
let 프레임;

function 함수(requestTime) {
  if (종료조건) {
    window.cancelAnimationFrame(프레임);
  };

  애니메이션처럼 보여줄 1개 프레임의 효과...

  프레임 = window.requestAnimationFrame(함수);
}
프레임 = window.requestAnimationFrame(함수);
```

기본적인 사용법은 이러하다.

1. 함수를 만들고

2. 함수를 리페인팅 타이밍에 맞춰서 한번 실행해준 뒤

3. 함수 내에서 마지막에 자기 자신을 다음 리페인팅 타이밍에 맞춰서 다시 부른다.

재귀를 도는 과정에서 애니메이션이 종료가 되는 조건이 되면 `cancelAnimationFrame()`로 지워준다.

```javascript
let 프레임;
const 종료 = 스크롤 멈출 좌표;

function 함수(requestTime) {

  const 스크롤 = window.scrollY; //현재 스크롤 위치
  if (스크롤 === 종료) {
    window.cancelAnimationFrame(프레임);
  };

  window.scrollTo(0, 스크롤 + 1);

  프레임 = window.requestAnimationFrame(함수);
}
프레임 = window.requestAnimationFrame(함수);
```

처음에는 이렇게 만들었다.

이렇게 만들어도 부드럽게 움직이긴 한다.

하지만 1px 움직이는데 프레임을 하나씩 써야하므로 멀리 있는 좌표로 이동하면 세월아 네월아 기어가게 된다.

가까이 이동하던지 멀리 이동하던지 항상 같은 시간 내에 완료할 필요가 있었다.

그래서 아래와 같이 계산식을 추가해야 했다.

<br>

#### 완성된 코드

```javascript
let 프레임;
const 종료 = 스크롤 멈출 좌표;
const 시작시간 = performance.now();
// 페이지가 로드 된 이후부터 지난 시간

function 함수(재실행시간) {
  //`requestAnimationFrame()`은 재귀해서 함수를 다시 실행하면 해당 함수에 다시 실행한 시간을 보내준다.

  const 스크롤 = window.scrollY; //현재 스크롤 위치
  if (스크롤 === 종료) {
    window.cancelAnimationFrame(프레임);
  };

  const 이동진척도 = (재실행시간 - 시작시간) / 500;
  // ms단위이므로 500 = 0.5s
  const 현재이동 = 스크롤 + (종료 - 스크롤) * 이동진척도;
  window.scrollTo(0, 현재이동);
  
  if (이동진척도 < 1) { // 이동진척도 100% 미만일때만 재귀
    프레임 = window.requestAnimationFrame(함수);
  };
}
프레임 = window.requestAnimationFrame(함수);
```

사실 아직 다 이해가 되지는 않는다.

수학 어렵다.

<br>

### text-underline-position

```css
text-underline-position: auto;
text-underline-position: under;
```

영어로 된 텍스트에 밑줄을 그리면 밑줄이 뚝뚝 끊겨서 점처럼 보일 때가 있다.

`text-decoration: underline` 시 밑줄의 기본 포지션은 auto값으로 알파벳 기준선에 밑줄을 그려준다.

이 포지션을 under로 바꿔주면 알파벳 기준선보다 아래로 선을 강제 이동 시킨다.

<br>

