---
layout: post
title:  next.js 기초 Head, fetch, rewrites, 환경변수 생성
date:   2022-12-03 13:06:42 +0900
comments : true
categories: Note
tags: [javascript, nextjs]
---

next.js 사용법 정리

[청약닷컴](https://cheongyak.com) 에서 기존에 router 로 작업하던 것을 next.js 로 수정했다.

<br>

### Head

next.js 는 Head 태그로 페이지의 head 를 설정할 수 있다.

HeadInfo 컴포넌트를 별도로 생성해 title 등의 정보를 props 로 전달받아 사용하면 편리하다.

```javascript
import Head from 'next/head';

export default function HeadInfo({title}){
  return (
    <Head>
      <title>{title}</title>
    </Head>
  );
}
```

각 페이지에 HeadInfo 컴포넌트를 import 해서 title 값을 넘겨주도록 한다.

<br>

### fetch

next.js 가 적용되지 않은 react 에서의 사용법과 동일하다.

```javascript
import { useEffect, useState } from 'react';

export default function Page() {
  const [Items, setItems] = useState([]);

  const fetchData = async ()=>{
    const data = await fetch(API_URL);
    const result = await data.json();
  }

  useEffect(()=>{
    fetchData();
  }, [])

  return (
    <main>
    </main>
  );
}
```

<br>

### rewrites

외부 API 를 사용하면 url 에 key 정보를 전달해야 하는데, next.js 의 rewrites 로 API 의 url 을 숨길 수 있다.

rewrites 는 경로를 임의로 지정해 해당 경로로 요청이 오면 다른 목적지의 경로로 매핑해준다.

```javascript
//next.config.js

/** @type {import('next').NextConfig} */
const API_KEY = 'API KEY값';

const nextConfig = {
  reactStrictMode: true,
  async rewrites() {
    return [
      {
        source: '요청경로',
        destination: `API_URL`,
      }
    ]
  },
}
```

rewrites 작성 후 API 를 요청할 컴포넌트에서 fetch 시 source 에 기재한 경로를 호출하면 destination 에 기재한 경로로 호출된다.

next.config.js 파일은 수정되면 서버를 재구동 해야한다.

<br>

### env

호출되는 url을 숨겨도 next.config.js 파일을 열어보면 key 값을 볼 수 있으므로 api key 등의 정보는 환경변수로 만드는 것이 좋다.

.env 라는 이름의 파일을 프로젝트 root 폴더에 생성해 환경변수를 만들 수 있다.

```env
API_KEY=키
```

이렇게 생성한 환경변수는 아래와 같이 `process.env.API_KEY` 로 사용한다.


```javascript
//next.config.js

/** @type {import('next').NextConfig} */
const API_KEY = process.env.API_KEY;

...
```

배포 시 .env 파일은 gitignore 에 추가해 업로드되지 않도록 제외시킨다.

<br>

