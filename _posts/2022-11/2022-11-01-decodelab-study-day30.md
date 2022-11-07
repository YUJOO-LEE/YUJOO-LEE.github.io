---
layout: post
title:  커스텀 슬라이드 (css, forwardRef hook활용)
date:   2022-11-01 22:47:54 +0900
comments : true
categories: Note
tags: [decodelab, javascript, css, react, hook]
---

학원 수업 Day30

### [커스텀 슬라이드](/decodelab/221101/custom_slider/)

<br>

<iframe src='/decodelab/221101/custom_slider/' frameborder='0' width='100%' height='500px'></iframe>

<br>

js로는 `엘리먼트.append(엘리먼트.firstElementChild)`, `prepend(엘리먼트.lastElementChild)` 를 통해 하위 엘리먼트의 위치만 지정하고, 모션 효과는 css를 활용한 슬라이드이다.

<br>

#### css

```css
ul{
    width: 800px; height: 400px; border: 1px solid #000; position: relative; 
}

li{
  width: 100%; height: 100%; position: absolute; top: 0; left: 0; 
  background: hotpink; display: flex; justify-content: center; align-items: center;
  font-size: 100px; color: #fff; transition: opacity 0.5s, transform 0.5s;
}

li:nth-of-type(1) {
  transform: translateX(90%) scale(0.4); opacity: 0; z-index: 1;
}
li:nth-of-type(2) {
  transform: translateX(-70%) scale(0.7); opacity: 0.7; z-index: 2;
}
li:nth-of-type(3) {
  transform: translateX(0%) scale(1); opacity: 0.9; z-index: 3;
}
li:nth-of-type(4) {
  transform: translateX(70%) scale(0.7); opacity: 0.7; z-index: 2;
}
li:nth-of-type(5) {
  transform: translateX(-90%) scale(0.4); opacity: 0; z-index: 1;
}
```

css로 슬라이드 모양을 만들어준다.

`nth-of-type()` 으로 원하는 모양대로 커스텀 해준 후 자바스크립트를 통해 요소의 순서를 바꿔주면 순번대로 모션이 동작해서 슬라이드가 구현이 된다.

<br>

#### 리액트 버전

리액트에서는 버튼과 패널들을 별개의 컴포넌트로 나누어서 관리할 수 있다.

먼저 패널 컴포넌트에서 아래와 같이 `forwardRef()` 로 패널 자체를 상위 컴포넌트로 보내준다.

```javascript
import { forwardRef } from 'react';

const Panels = forwardRef((_, ref) => {
  return (
    <ul ref={ref}>
      {Array(5)
        .fill()
        .map((_, idx) => 
          <li key={idx}>{idx + 1}</li>
        )}
    </ul>
  );
})

export default Panels;
```

부모 컴포넌트에서 패널 엘리먼트를 ref로 받아와서 다시 버튼 컴포넌트로 보내준다.

```javascript
function App() {
  const panel = useRef(null);

  return (
    <>
      <Btns panel={panel} />
      <Panels ref={panel} />
    </>
  );
}
```

버튼 컴포넌트에서 패널을 받아와서 이벤트를 부여한다.

```javascript
function Btns({ panel }) {
  const moveNext = () => {
    panel.current.append(panel.current.firstElementChild);
  }

  const movePrev = () => {
    panel.current.prepend(panel.current.lastElementChild);
  }

  return (
    <>
      <Btn className='prev' onClick={movePrev}>prev</Btn>
      <Btn className='next' onClick={moveNext}>next</Btn>
    </>
  );
}
```

<br>

```javascript
useEffect(() => {
  movePrev();
  movePrev();
}, [])
```

패널 내부 요소들 중 첫번째 특정 요소를 먼저 띄우고 싶다면 `useEffect()` 로 마운트 시 이벤트를 발생시켜서 위치를 지정해 주면 된다.

<br>

