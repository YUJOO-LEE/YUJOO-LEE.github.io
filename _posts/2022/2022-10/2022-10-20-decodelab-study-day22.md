---
layout: post
title:  스크롤 이벤트 구현
date:   2022-10-20 15:43:23 +0900
comments : true
categories: Note
tags: [decodelab, react, scroll]
---

학원 수업 Day22

### scroll event

실습용으로 만든 기업형 레이아웃의 메인 페이지에 스크롤 이벤트를 입혔다.

<br>

#### section 정보 저장

`height: 100vh` 로 지정되어 있는 각 section의 정보들을 받아온다.

```javascript
const main = useRef(null);
const pos = useRef([]);
let secs;

const getPos = ()=>{
  pos.current = [];
  secs = main.current.querySelectorAll('.myScroll');
  for (const sec of secs) {
    pos.current.push(sec.offsetTop);
    // 각 section 높이값 배열로 저장
  }
}
```

main의 `useRef()` 는 main DOM을 사용하기 위해 지정했으나, pos의 `useRef()` 는 정보 저장용이다.

useRef는 값이 변해도 랜더링 되지 않아 정보 저장용으로 사용할 수 있다.

`엘리먼트.offsetTop` 문서 최상단으로부터 해당 엘리먼트 높이 위치를 반환한다.

여기서는 section들의 위치를 가져오는 용으로 사용했다.

<br>

#### 초기값 세팅 실행

```javascript
useEffect(()=>{
  getPos();
  window.addEventListener('resize', getPos);
  
  return (()=>{
    window.removeEventListener('resize', getPos);
  });
}, [])
```

컴포넌트가 마운트 되면 미리 만들어 둔 `getPos()` 를 실행 해 초기값을 세팅하고 `resize` 이벤트 리스너를 생성한다.

언마운트되면 불필요한 이벤트리스너를 지워준다.

<br>

#### 클릭 이벤트로 이동

```javascript
const [ Index, setIndex ] = useState(null);
```

```jsx
<li onClick={()=>setIndex(0)}></li>
<li onClick={()=>setIndex(1)}></li>
<li onClick={()=>setIndex(2)}></li>
<li onClick={()=>setIndex(3)}></li>
```

선택한 위치로 이동시키기 위해 Index state 를 생성하고 `onClick` 으로 변경되게 한다.

```javascript
useEffect(()=>{
  new Anime(window, {
    prop: 'scroll',
    value: pos.current[Index],
    duration: 500
  })
}, [Index])
```

Index 값이 변경되면 스크롤 위치를 이동시킨다.

<br>

#### 위치에 따른 클래스 지정

```javascript
const activation = ()=>{
  const base = window.innerHeight / 2 * -1;
  // 스타일을 줄 section 위치의 -50% 부터 실행
  // = section이 50%만 보여도 실행 
  const scroll = window.scrollY || window.pageYOffset;
  // 현재 스크롤 Y좌표 값
  const btns = main.current.querySelectorAll('.scrollNavi li');
  // 각 버튼 엘리먼트 참조
  
  pos.current.map((top, i)=>{
    if (scroll >= top + base) {
      // 현재 스크롤 좌표가 미리 배열로 저장한 높이값 + 기본 실행값보다 크다면 실행
      for (let el of btns) el.classList.remove('on');
      for (let sec of secs) sec.classList.remove('on');
      // 모든 버튼, 모든 섹션의 클래스 제거
      secs[i].classList.add('on');
      btns[i].classList.add('on');
      // 해당 버튼, 섹션에만 클래스 부여
    }
  });
}
```

각 섹션의 컨텐츠는 해당 섹션의 클래스가 'on' 상태이면 동작하도록 css를 부여하면 된다.

또는 scroll 값을 `useState()` 로 넘겨 받아서 직접 style에 사용한다면 스크롤 좌표에 맞춘 효과를 보여줄 수 있다.

<br>

