---
layout: post
title:  학원 수업 내용 Day7
date:   2022-09-27 19:05:23 +0900
comments : true
categories: Note
tags: [decodelab]
---

오늘은 지난번에 만든 예제를 완성시키는 수업이었다.

먼저 flex로 만들었던 레이아웃에 js를 입히는 작업을 했다. [flex layout 완성](https://leeyujoo.com/decodelab/220927/flex_layout/)

레이아웃을 완성하기에 앞서 `target`과 `currentTarget`의 차이점을 먼저 배웠다.

-  **currentTarget** : 이벤트 리스너가 지정된 요소    

-  **target** : 실제 이벤트 발생 요소

<br>

추가적으로 연산자에 관한 정리도 다시 했다.

- **단항연산** : n++, n--    
  1개의 항을 대상으로 연산 수행

- **2항연산** : n + m    
  2개의 항을 대상으로 연산 수행

- **3항연산** : (조건식) ? true시 값 또는 연산식 : false시 값 또는 연산식;    
  3개의 항을 대상으로 연산 수행

<br>

오후에는 기업형 레이아웃에 서브 페이지를 입히고 gitHub에 올리는 작업까지 했다.

[기업형 레이아웃 (작업중)](https://leeyujoo.com/decodelab/220927/layout_practice/)

오늘은 COMMUNITY 페이지와 DEPARTMEN 페이지를 완성했다.

<br>

서브페이지로 나누기 위해 기본 틀으로 *sub_page.html*을 만들어 복사해서 내용을 작성했고,

scss 파일 또한 sub용으로 공통부분을 별도 파일으로 뺀 후

서브 페이지 별 scss도 따로 빼서 메인 파일인 *style.scss*에서 `@import`시켰다.

<br>

커뮤니티 페이지를 작성하기 전 `table` 태그에 대한 정리도 한 번 했다.

- **셀 병합**    
  가로 칸을 합칠 때는 합쳐지는 칸을 삭제한 후 시작칸에 colspan = '합쳐지는 칸 수'    
  세로 칸을 합칠 때는 합쳐지는 칸을 삭제한 후 시작칸에 rowspan = '합쳐지는 칸 수'

- **테이블 구조**
  **thead** - 테이블의 행 그룹 중 제목 셀 그룹. 테이블에 한번만 사용    
  **tbody** - 본문    
  **tfoot** - 레코드 내용의 소계, 합계 등 정보에 해당하는 푸터, 한번만 사용

- **scope**    
  제목셀과 내용셀의 관계 지정    
  데이터의 의미와 관계를 파악하기 쉽게 해줌    
  th에 scope속성을 지정하고 방향에 따라 col, row를 지정    

<br>