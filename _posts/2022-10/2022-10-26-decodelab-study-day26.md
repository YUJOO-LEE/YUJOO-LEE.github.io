---
layout: post
title:  framer motion 활용, 이미지/비디오 캐싱 및 로딩 상태 구현
date:   2022-10-26 16:54:01 +0900
comments : true
categories: Note
tags: [decodelab, react, framer, animation, api, hook]
---

학원 수업 Day26

### Framer Motion

컴포넌트 마운트 시에는 애니메이션 효과를 주기 쉽지만, 언마운트시에는 컴포넌트 자체가 사라져버려 애니메이션 효과를 주기 어렵다.

[Framer Motion플러그인](https://www.framer.com/motion/)을 사용하면 애니메이션 효과 후 언마운트 되도록하여 언마운트 효과를 손쉽게 구현할 수 있다.

```npm
npm i framer-motion@6 --force
```

@6을 지워서 최신 버전을 설치할 수도 있지만 현재 제작 환경을 고려하여 6버전으로 인스톨했다.

사용방법은 간단하다.

먼저 컴포넌트 최상단에 아래와 같이 framer-motion을 `import` 해야 한다.


```javascript
import { motion, AnimatePresence } from 'framer-motion';
```

그리고 동작을 원하는 태그 외부를 `AnimatePresence`으로 감싸고, 태그 자체에 `motion.`을 추가한다.

```JSX
{% raw %}
<AnimatePresence>
  <motion.aside
    initial={{opacity: 0}} 
    animate={{opacity: 1, transition: {duration: 0.5, delay: 0.5}}} 
    exit={{opacity: 0, transition: {delay: 0.5}}}
  >
  </motion.aside>
</AnimatePresence>
{% endraw %}
```

이제 aside는 태그 내 속성값만 넣어주면 framer-motion에 의해 애니메이션이 제어가 된다.

**initial** - 동작 전 시작 상태

**animate** - 동작 완료 후 상태

**exit** - 컴포넌트 언마운트 후 상태

속성에는 객체 형태로 속성값들을 넣어준다. transition내 duration설정은 물론, delay 설정도 할 수 있어 모션이 순차적으로 적용되는 듯한 효과도 가능하다.

<br>

### forwardRef

어제 배운 forwardRef를 다시한번 실습했다.

<br>

**사용방법**

1. 기존의 컴포넌트 함수를 대입형 함수로 바꾸고 `forwardRef()` 안쪽에 함수를 넣어준다.    
import도 잊지 말아야 한다.

2. 화살표 함수 두번째 인수로 ref 추가

3. `forwardRef(함수)` 내부에 `useImperativeHandle()` 추가

4. 해당 함수로 객체를 반환해서 상위 컴포넌트로 전달

6. 상위 컴포넌트의 `useRef()` 로 forwardRef에서 `return` 되는 자식 참조

만약 `useImperativeHandle()`이 없다면 상위 컴포넌트에서는 return되는 JSX 전체를 ref 값으로 갖는다.

```javascript
import { forwardRef, useImperativeHandle, useState } from 'react';

const 컴포넌트 = forwardRef((props, ref)=>{
  const [open, setOpen] = useState(false);

  useImperativeHandle(
    ref,
    () => {
      return {
        setOpen: ()=>setOpen(true),
      };
    }
  )
  
  return (
   // JSX 내용
  )
});
export default 컴포넌트;
```

이제 이 컴포넌트를 불러오는 상위 컴포넌트에서는 이 컴포넌트에서 보낸 `setOpen()` 메서드를 사용할 수 있게 된다.

상위 컴포넌트에서 ref를 만들어주고 REF.current.메서드()로 사용하면 된다.

```javascript
const pop = useRef(null);

<Popup ref={pop}>
  내용
</Popup>
```

<br>

### 이미지, 동영상 캐싱

웹 페이지의 렌더링 방식은 두 가지가 있다.

#### SSR : server side rendering

- 페이지 이동 시 일일이 HTML 파일을 서버로부터 불러오는 방식     
- 초기 로딩 속도가 빠르다.    
- SEO 최적화에 유리하다.

#### CSR : client side rendering

- 초기 페이지 로딩 시 모든 컴포넌트(JSX) 파일을 한번에 불러오는 방식    
- 한번 데이터를 가져온 이후 페이지 이동 시 전환 로딩이 없다.    
- 초기 로딩 속도가 상대적으로 길다.    
- SEO 최적화에 취약하다.

리액트는 CSR 방식으로 렌더링 한다.

먼저 컴포넌트들을 받아와서 화면에 렌더링 해주는 방식으로, 최초 로딩 이후 화면 전환 시 로딩이 없고, 변화가 있는 부분들만 바꾸어 보여주는 식이다.

하지만 이미지, 동영상은 해당 컴포넌트 마운트 시 불러오게 되는데, 용량이 비교적 큰 이미지는 불러오는 데 시간이 걸려서 리액트의 장점을 살리지 못한다.

페이지가 로드될 때 로딩 화면을 보여주면서 원하는 또는 중요한 이미지들을 미리 캐싱하고, 이미지와 비디오의 로드가 모두 완료되면 로딩 화면을 지우고 페이지를 보여주도록 하는 것이 화면 전환 시 자연스럽다.

<br>

#### DOM 요소 생성

`index.html` 파일에 이미지를 불러와서 넣을 요소와 로딩 화면을 연출할 요소를 추가한다.

```html
<div class="defaults"></div>
<div class="mask">Loading...</div>
```

```css
.defaults{
  position: absolute;
  top: -99999999px;
}
.mask{
  width: 100%;
  height: 100vh;
  background-color: #111;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 100;
  opacity: 1;
  transition: opacity 2s;
  display: flex;
  justify-content: center;
  align-items: center;
}
.mask.off{
  opacity: 0;
}
```

.default는 이미지들을 넣을 요소로 내용이 화면에서는 보이지 않도록 위치값을 준다.

.mask는 로딩화면이므로 처음에는 `opacity`값이 1으로 활성화 되었다가, 로딩완료 후 off상태 시 0으로 안보이게 해 준다.

<br>

#### 데이터 세팅

```javascript
const defaults = document.querySelector('.defaults');
const mask = document.querySelector('.mask');
let tags = '';
const baseUrl = 'img 폴더 절대경로';
const imgs = [
  baseUrl+'이미지파일명1.jpg',
  baseUrl+'이미지파일명2.jpg',
  baseUrl+'이미지파일명3.jpg',
  baseUrl+'이미지파일명4.jpg',
]
const vids= [
  baseUrl+'비디오파일명1.mp4',
  baseUrl+'비디오파일명2.mp4',
  baseUrl+'비디오파일명3.mp4',
  baseUrl+'비디오파일명4.mp4',
]
```

앞서 만든 두 DOM요소와 미리 캐싱할 이미지, 비디오 파일을 배열로 담아 준비한다.

<br>

#### 데이터 출력

```javascript
function createDOM() {
  imgs.forEach(src=>tags+=`<img src=${src} />`);
  vids.forEach(src=>tags+=`<video src=${src}></video>`);
  defaults.innerHTML = tags;
}
```

배열로 저장해 둔 이미지, 비디오를 DOM요소로 생성해서 defaults에 넣는다.

이렇게만 해도 이미 이미지와 비디오들은 페이지 오픈 시 미리 한번에 캐싱하게 된다.

##### 11/2 내용추가

아이폰에서 페이지를 열면 동영상을 불러올 수 없어서 로딩화면이 사라지지 않는 현상을 발견했다.

`video` 에 `autoplay muted playinline` 속성을 넣어주면 해결된다.

```javascript
function createDOM() {
  imgs.forEach(src=>tags+=`<img src=${src} />`);
  vids.forEach(src=>tags+=`<video src=${src} autoplay muted playinline></video>`);
  defaults.innerHTML = tags;
}
```

<br>

#### 로드 상태 반환

```javascript
function loadImg() {
  return new Promise((res,rej)=>{
    let countImg = 0;
    const imgDOM = defaults.querySelectorAll('img');

    imgDOM.forEach(img=>{
      img.onload = ()=>{
        // 로드 완료 후 변수 하나씩 증가
        countImg++;
        // 완료된 갯수가 배열 값의 갯수와 같다면 정상값 반환
        if (countImg === imgs.length) res(true);
      }
    })
  })
}
function loadVid() {
  return new Promise((res,rej)=>{
    let countVid = 0;
    const vidDOM = defaults.querySelectorAll('video');

    vidDOM.forEach(vid=>{
      vid.onloadeddata = ()=>{
        countVid++;
        if (countVid === vids.length) res(true);
      }
    })
  })
}
```

이미지와 비디오의 로드 상태를 확인하기 위해, 로드 상태를 `Promise`객체로 `return`받는 함수를 만든다.

이미지가 로드 되었는지 확인하는 이벤트명은 `onload`이고, 비디오를 확인하는 이벤트는 `onloadeddata`으로 서로 달라서 함수를 별도로 만들어주어야 한다.

각각의 함수는 load가 완료되면 true 값이 반환된다.

<br>

#### Promise.all

```javascript
async function endLoading() {
  const result = await Promise.all([loadImg(), loadVid()])
  // 배열 내 모든 Promise가 완료되면 각각의 값을 result로 받음 
  const [loadedImg, loadedVid] = result;
  // result의 결과값들이 배열에 담겨있으므로 편리하게 사용하기 위해 변수로 재할당 

  if (loadedImg && loadedVid) {
    // 둘 다 true 상태라면 로딩 화면을 안보이도록 처리
    mask.classList.add('off');
  }

  setTimeout(()=>{
    // 로딩화면 투명효과 적용이후, 생성했던 DOM 요소들을 제거
    mask.remove();
    defaults.remove();
  }, 2000); // 로딩화면 transition이 1s이므로 해당시간보다 길게 지정
}
```

두개의 Promise 가 모두 `true` 상태여야 로딩화면을 지우고 사이트를 보여주어야 하기 때문에 `Promise.all` 을 사용한다.

<br>

#### 함수 호출

지금까지 만든 함수들은 선언만 되고 실행이 되지 않았으므로 각 함수를 실행해주어야 작동된다.

```javascript
createDOM();
endLoading();
```

돔 생성과 로딩 종료 함수를 실행해야한다.

나머지 이미지, 비디오 로딩 함수는 종료 함수 내에서 불러오므로 전역에서 실행시킬 필요가 없다.

<br>

