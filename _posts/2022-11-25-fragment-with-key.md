---
layout: post
title:  map으로 리스트 출력 중 return시 발생 문제
date:   2022-11-25 23:54:22 +0900
comments : true
categories: Note
tags: [javascript, react]
---

오늘의 삽질일기.

<br>

리스트를 동적으로 출력하기 위해 `map()` 을 돌 때, 조건에 따라 일부 값을 나타내고 싶지 않을 수 있다.

그래서 처음에는 아래와 같이 조건을 주어 빈값을 `return` 시켰다.

```javascript
리스트.map((데이터, 인덱스)=>{
  if (조건) return;

  return (
    데이터출력
  )
})
```

그러나 JSX에서 작성한 코드이기 때문에 돔 요소를 return 시켜야 했고, 그래서 빈 요소를 `return` 시켰다.

```javascript
리스트.map((데이터, 인덱스)=>{
  if (조건) return <></>;

  return (
    데이터출력
  )
})
```

그러나 또 다시 문제가 발생했다.

map 으로 리스트를 출력하면 고유한 key값을 넣어주어야 하기 때문에 경고가 발생했다.

> Warning: Each child in a list should have a unique "key" prop.

경고를 해결하기 위해 react 에서 Fragment 컴포넌트를 불러와서 사용하고 key 값을 부여했다.

```javascript
import { Fragment } from 'react';

리스트.map((데이터, 인덱스)=>{
  if (조건) return <Fragment key={고유값}></Fragment>;

  return (
    데이터출력
  )
})
```

이제 에러가 해결 되었다.

하지만 해결하고 보니 코드가 너무 지저분해 보였다.

아무리 생각해도 화면에 출력되지도 않을 return 값이 불필요하게 길다.

그래서 `map()` 으로 출력하기 전에 `filter()` 로 걸러주기로 한다.

```javascript
리스트.filter((_, 인덱스)=>{
  if (조건) return false;
  return true;
}).map((데이터, 인덱스)=>{
  return (
    데이터출력
  )
})
```

`filter()` 는 배열을 돌면서 `true` 가 반환될 때만 해당 값을 포함해서 새로운 배열으로 반환한다.

`filter()` 로 먼저 데이터를 걸러서 `map()` 에서는 조건 없이 모든 데이터를 반환하도록 했다.

<br>
