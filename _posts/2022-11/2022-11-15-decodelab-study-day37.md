---
layout: post
title:  IntersectionObserver, Z축 스크롤, 입체 큐브, 반전 이펙트 커서
date:   2022-11-15 17:34:23 +0900
comments : true
categories: Note
tags: [decodelab, javascript]
---

학원 마지막 날!

포트폴리오에 적용할 수 있도록 다양한 효과의 ui를 실습했다.

<br><br>
<hr>
<br><br>

### Intersection Observer [링크](/decodelab/221115/intersection_observer/)

<br>

<iframe src='/decodelab/221115/intersection_observer/' frameborder='0' width='100%' height='500px'></iframe>

<br>

`IntersectionObserver()`는 DOM 엘리먼트가 뷰포트상에 노출되었는지 여부를 비동기적으로 감시하는 API이다. 

스크롤 좌표값에 따라 스타일을 적용하는 대신, `IntersectionObserver()`를 활용해 현재 화면에 보여지고 있는지 여부에 따라 스타일을 적용할 수 있다.

`IntersectionObserver()`를 선언해두면 변경점이 생길 때 마다 다시 실행한다.

옵저버 생성 시 인자로 변경값에 대한 실행 함수를 입력한다.

```javascript
const options = {
  root: section,
  rootMargin: '0px',
  threshold: 1.0
}

const observer = new IntersectionObserver((entries)=>{
  console.log(entries);
  // entries : 옵저버에 등록된 요소들이 배열 형태로 반환됨
}, options)
```

이렇게 생성한 옵저버에 감시 요소를 등록한다.

```javascript
observer.observe(등록할 요소);
```

<br>

#### 반환된 정보값

-- boundingClientRect : 감시 요소의 가로, 세로, 위치, 넚이, 높이    
-- intersectionRatio : 감시 요소의 현재 화면에서 보이는 영역 비율    
-- intersectionRect : 현재 보이는 부분에 대한 정보    
-- isIntersecting : threshold 비율만큼 보여지고 있는지    
-- rootBounds : 감시 요소의 상위 요소의 정보    
-- target : 감시 대상 요소    
-- time : 로드된 시점부터 감시 요소의 상태가 변경되기 까지의 시간    

<br>

#### 옵션

- root    
-- 뷰포트 대신 사용할 요소 객체(루트 요소)를 지정한다.
-- 기본값은 null 으로, null 입력 시 브라우저 뷰포트가 지정된다. 

- rootMargin    
-- margin 값을 주어 Root 범위를 확장하거나 축소할 수 있다.    
-- 기본값은 0px 0px 0px 0px이며 단위를 꼭 입력해야 한다.    
-- css와 같이 TOP, RIGHT, BOTTOM, LEFT 순으로 입력한다.

- threshold    
-- 화면에 보여지는 비율 지정 (0-1)    
-- 0 : 아주 조금이라도 노출 시 isIntersecting 값이 true 가 됨    
-- 1 : 완전히 노출 시 isIntersecting 값이 true 가 됨

<br>

실행 함수 내부에 배열을 반복문으로 풀어서 정보값에 따라 요소를 변화시킨다.

```javascript
entries.forEach((entry)=>{
  entry.target.classList.toggle('on', entry.isIntersecting);
})
```

`classList.toggle()` 메서드는 첫번째 인자로 추가/제거 될 클래스 명을 입력하고, 두번째 인자로 변경될 조건을 입력한다.

isIntersecting 값은 boolean 으로 반환되므로 해당 값의 true/false 에 따라 'on' 클래스가 붙게 된다.

<br>

더이상 옵저버를 쓰지 않을 때, 예를 들어 리액트에서 컴포넌트 언마운트 시에는 `옵저버.disconnect()` 로 종료시켜주는 것이 좋다.

<br><br>
<hr>
<br><br>

### Z 스크롤 [링크](/decodelab/221115/z_scroll/)

<br>

<iframe src='/decodelab/221115/z_scroll/' frameborder='0' width='100%' height='500px'></iframe>

<br>

각 요소들을 css 로 `position:fixed` 후 z방향으로 배치한 뒤 스크롤 값에 따라 요소를 z 방향으로 이동시켜 앞으로 튀어나오는 효과를 구현한다.

```javascript
window.addEventListener('scroll', ()=>{
  const scroll = window.scrollY;

  boxs.forEach((box, idx)=>{
    // 음수로 이동시켜 뒤에 있는 요소 앞으로 당김 
    box.style.transform = `translateZ(${distance * idx * -1 + scroll}px)`;

    // 해당 메뉴에 클래스 부여
    if (scroll >= idx * distance) { 
      for (const btn of btns) {
        btn.classList.remove('on');
      }
      btns[idx].classList.add('on');
    }
  })
})
```

<br><br>
<hr>
<br><br>

### 커서 디자인 [링크](/decodelab/221115/cursor/)

<br>

<iframe src='/decodelab/221115/cursor/' frameborder='0' width='100%' height='500px'></iframe>

<br>

DOM 요소를 만들어서 커서를 따라다니게 한다.

```javascript
const mouseMove = (e)=>{
  cursor.style.left = e.clientX + 'px';
  cursor.style.top = e.clientY + 'px';
}
```

<br>

아래는 css 관련 새로 알게된 속성이다.

`pointer-events`    
- 포인터 이벤트를 적용 여부를 결정한다.     
- 기본값 auto, 비적용시 none

`mix-blend-mode`    
- 적용한 대상과 배경을 혼합할 스타일을 지정한다.    
- 포토샵의 레이어 스타일 같은 느낌.    
- 기본값은 normal 이며 difference, multiply, overlay 등 다양한 속성이 있다.

<br><br>
<hr>
<br><br>

### 입체 회전 큐브 [링크](/decodelab/221115/cube/)

<br>

<iframe src='/decodelab/221115/cube/' frameborder='0' width='100%' height='500px'></iframe>

<br>

대부분 효과를 css 로 주고 js 로는 class 정도만 입힌 간단하면서 화려한 디자인이다.

<br><br>
<hr>
<br><br>
