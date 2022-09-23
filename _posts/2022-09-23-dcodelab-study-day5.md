---
layout: post
title:  학원 수업 내용 Day5
date:   2022-09-23 18:00:23 +0900
comments : true
categories: Note
tags: [decodelab, css, flex, layout, faq]
---


오늘 진짜 알차고 보람찼다!

flex는 예습해서 어느정도 알고 있었지만,

자유시간이 주어져서 flex 이용해서 [header부분](#h-header-실습)만 만들었는데 반응형까지 하고싶어서 진짜 급하게 했다.

정신 날아가는 줄 알았다...

<br>

그리고 다양한 css 선택자를 배웠는데 유용하게 쓸 수 있을 것 같다.

```css
/* A+B : A 바로 뒤에 있는 B 선택 */
h2+ul{
  color: blue;
}

/* A~B : A 뒤에 나오는 모든 연결된 B 선택 */
h2~ul{
  color: blue;
}

/* 대괄호 : 해당 속성 선택 */
input[type='email']{
  color: blue;
}

a[target]{  /* 속성 입력 안하면 유무만 확인 */
  color: blue;
}

a[target='_blank']{
  color: blue;
}

h2[class^='main']{ /* ^='값' 값으로 시작하는 속성 선택*/
  color: blue;
}
```

<br>

마지막에 이런 선택자를 활용한 예제([FAQ](#h-css만을-활용한-faq))도 만들었는데,

이 부분이 너무 신선한 점이 많았다.

radio input 체크유무로 효과들을 바꿨는데, position을 적극적으로 사용해서 준 효과들이 신선했다.

`appearance: none;`도 오늘 처음 배웠다.

인풋 요소를 존재는 하지만 보이지만 않게 하는 속성이라고 한다. 

<br><br>
<hr>
<br><br>

#### [flex 기본](/d-code-lab/220923/flex_basic/)

<br>

<iframe src='/d-code-lab/220923/flex_basic/' frameborder='0' width='100%' height='500px'></iframe>

<br><br>
<hr>
<br><br>

#### [flex 활용 레이아웃](/d-code-lab/220923/flex_layout/)

<br>

<iframe src='/d-code-lab/220923/flex_layout/' frameborder='0' width='100%' height='500px'></iframe>

<br><br>
<hr>
<br><br>

#### [header 실습](/d-code-lab/220923/header_practice1/)

<br>

<iframe src='/d-code-lab/220923/header_practice1/' frameborder='0' width='100%' height='500px'></iframe>

<br><br>
<hr>
<br><br>

#### [css만을 활용한 FAQ](/d-code-lab/220923/faq/)

<br>

<iframe src='/d-code-lab/220923/faq/' frameborder='0' width='100%' height='500px'></iframe>

<br><br>


