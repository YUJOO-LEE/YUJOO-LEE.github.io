---
layout: post
title:  Z축 스크롤, 입체 큐브, 반전 이펙트 커서
date:   2022-11-09 17:34:23 +0900
comments : true
categories: Note
tags: [decodelab, javascript]
---

학원 마지막 날!

포트폴리오에 적용할 수 있도록 다양한 효과의 ui를 실습했다.

<br><br>
<hr>
<br><br>

#### Z 스크롤 [링크](/decodelab/221115/z_scroll/)

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

#### 커서 디자인 [링크](/decodelab/221115/cursor/)

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

#### 입체 회전 큐브 [링크](/decodelab/221115/cube/)

<br>

<iframe src='/decodelab/221115/cube/' frameborder='0' width='100%' height='500px'></iframe>

<br>

대부분 효과를 css 로 주고 js 로는 class 정도만 입힌 간단하면서 화려한 디자인이다.

<br><br>
<hr>
<br><br>
