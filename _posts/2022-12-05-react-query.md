---
layout: post
title:  react-query 설치, 사용
date:   2022-12-05 11:29:09 +0900
comments : true
categories: Note
tags: [react, query, terminal, typescript]
---

React query는 react App 에서 서버의 상태를 불러오고, 캐싱하며, 지속적으로 동기화하고 업데이트 하는 비동기 로직을 지원하는 라이브러리이다.

server state 의 메모리를 관리하고 garbage collection 을 지원하고, 동일 데이터를 여러번 요청하면 알아서 한번만 요청한다.

사용법이 react 의 hook 과 비슷하다.

pagination 및 lazy loading 에 최적화 되어있고, 인피니트 스크롤을 손쉽게 만들 수 있다.

<br>

### 설치

```terminal
yarn add @tanstack/react-query
```

<br>

### QueryClient

react query 를 사용하기 위해 클라이언트를 생성한다.

src 폴더에 queryClient.ts 로 파일을 생성했다.

```typescript
import { QueryClient } from '@tanstack/react-query';

// Create a client
export const getClient = (()=>{
  let client: QueryClient | null = null;
  return ()=>{
    if (!client) client = new QueryClient({});
    return client;
  }
})();
```

<br>

### QueryClientProvider

query client 를 전역에서 사용하기 위해 app을 QueryClientProvider 로 감싸준다.

이때 전달할 client 속성값을 위에서 생성한 getClient 로 전달한다.

```typescript
// _app.ts
import { QueryClientProvider } from '@tanstack/react-query';
import { getClient } from './queryClient';

function App() {
  const element = useRoutes(routes);
  const queryClient = getClient();

  return (
    <QueryClientProvider client={queryClient}>
      {element}
    </QueryClientProvider>
  );
}

export default App;
```

<br>

### devtools

react-query 는 전용 개발자도구를 제공해 편리한 개발을 돕는다.

개발자도구를 사용하기 위해 먼저 react-query-devtools 를 설치한다.

```terminal
yarn add @tanstack/react-query-devtools
```

QueryClientProvider 내부에 ReactQueryDevtools 을 추가한다.

```tsx
<QueryClientProvider client={queryClient}>
  {element}
  <ReactQueryDevtools initialIsOpen={false}></ReactQueryDevtools>
</QueryClientProvider>
```

run dev 를 실행하면 화면 좌측 하단에 개발자도구가 추가된 것을 확인할 수 있다.

<br>

### query key

쿼리를 지정해 실행하기 위한 query key 를 생성한다.

queryClient.ts 파일 내 객체형태로 정해둔다.

```typescript
export const QueryKeys = {
  PRODUCTS: 'PRODUCTS',
}
```

<br>

### fetcher

client 에서 보내는 요청을 받아 처리할 fetcher 를 만든다.

먼저 전달받을 값에 대한 type 을 생성한다.

```typescript
type TypeMethod = 'GET' | 'POST' | 'PUT' | 'DELETE' | 'PATCH';
type TypeBodyOBJ = { [key: string]: any };
```

실제로 통신을 전달받을 함수를 생성한다.

매개변수로 method 로 요청 종류를 받고, path 로 상세 경로를 지정하고, body 및 params 로 데이터를 전달받는다.

비동기통신의 동기화를 위해 async, await 로 함수를 생성하고, try, catch 문으로 fetch 를 실행한다.

```typescript
export const fetcher = async ({
  method,
  path,
  body,
  params,
}: {
  method: TypeMethod;
  path: string;
  body?: TypeBodyOBJ;
  params?: TypeBodyOBJ;
}) => {
  try {
    const url = `${BASE_URL}${path}`;
    const fetchOptions: RequestInit = {
      method,
      headers: {
        'Content-Type': 'application/json',
        'Access-Control-Allow-Origin': BASE_URL
      }
    }
    const res = await fetch(url, fetchOptions);
    const json = await res.json();
    return json;
  } catch(err) {
    console.error(err);
  }
}
```

RequestInit 는 request 의 초기화를 위해 ts 에서 기본적으로 제공하는 interface 이다.

<br>

### useQuery

client 에서 실제로 server 와 통신하기 위해 사용하는 react-query 의 메서드이다.

useQuery 를 사용하기 위해 먼저 react-query 로부터 `useQuery()` 를 `import` 하고, 위에서 생성한 fetcher, QueryKey 도 가져온다.

```typescript
import { useQuery } from "@tanstack/react-query";
import { fetcher, QueryKeys } from "../../queryClient";
```

<br>

`useQuery()` 를 사용해 데이터를 GET 한다.

파라미터를 객체 형태로 넘겨서 key 와 fetcher 를 전달한다.

여기서 key 는 객체 형태로 전달되어야 한다.

```typescript
const { data } = useQuery({
  queryKey: [QueryKeys.PRODUCTS], 
  queryFn: () => fetcher({
      method: 'GET',
      path: '경로',
    })
})
```

data 를 콘솔에 찍어보면 데이터가 잘 전달되는 것을 확인할 수 있다.

<br>
