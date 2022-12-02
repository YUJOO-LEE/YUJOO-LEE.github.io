---
layout: post
title:  next.js 기초
date:   2022-12-02 20:35:23 +0900
comments : true
categories: Note
tags: [javascript, nextjs, terminal]
---

next.js 사용법 정리

[청약닷컴](https://cheongyak.com) 에서 기존에 router 로 작업하던 것을 next.js 로 수정했다.

<br>

### 설치

프로젝트 폴더에서 아래 명령어로 next.js 기반의 app을 생성한다.

```terminal
npx create-next-app@latest .
```

이때 typescript 와 ESLint 의 사용여부를 묻는데 프로젝트에 맞춰서 선택한다.

ESLint 는 Javascript 의 문법을 확인해 주는 도구이다.

CRA 로 app 생성시에는 ESLint 가 자동으로 설치된다.

app 을 생성하면 기존 CRA 로 생성한 것과 다르게 pages 와 styles 폴더, next.config.js 가 있다.

<br>

### 프로젝트 실행

local 에서 프로젝트를 실행할때 아래 명령어를 사용한다.

```terminal
npm run dev
```

run 시킨 후 localhost:3000 에서 개발중인 프로젝트를 확인할 수 있다. 

<br>

### pages

pages 폴더 내 파일을 생성하면 파일 및 폴더명에 맞춰서 자동으로 router 를  설정해 페이지를 생성한다.

pages 폴더의 index 파일은 사이트의 index 페이지가 되고, pages/about.js 파일은 사이트도메인/about 의 경로를 갖는다.

단독 페이지가 아닌 컴포넌트를 생성하고 싶다면 pages 외부에 별도로 components 등의 폴더를 만들어 작성한다.

<br>

#### Link

next.js 환경에서 페이지를 이동하고 싶다면 next 의 link 컴포넌트를 사용한다.

Link 컴포넌트는 자동으로 a 태그로 렌더링된다.

```javascript
import Link from 'next/link';

<Link href='URL'>Button</Link>
```

<br>

### useRouter()

`useRouter()` 로 현재 경로에 대한 정보를 받을 수 있다.

먼저 아래와 같이 import 및 사용선언을 한 후 사용한다.

```javascript
import { useRouter } from 'next/router';

const router = useRouter();
```

이렇게 선언을 하면 router 에 다양한 정보값들이 담긴다.

<br>

- pathname    
현재 페이지 경로를 반환한다.    
도메인/about 경로의 페이지라면 '/about' 이 값이 된다.

- query    
쿼리스트링을 객체형태로 반환된다.    
도메인/about?id=1&name=myName 이라면 {id: '1', name: 'myName'} 을 값으로 가질 것이다.    
기본값은 {} 이다.

<br>

#### router.push()

jsx 가 아닌 스크립트에서 페이지를 이동시키고 싶을 때 사용한다.

```javascript
router.push('/about?id=1');
```

아래와 같이 인자를 객체 형태로 작성해 쿼리스트링을 객체 상태로 전달할 수도 있다.

```javascript
router.push(
  {
    pathname: '/about',
    query: { id: 1 }
  }
);
```

두번째 인자로 경로를 기재하면 기재되지 않는 쿼리스트링은 경로에 표시되지 않고 값으로만 전달된다.

```javascript
router.push(
  {
    pathname: '/about',
    query: { id: 1 }
  },
  '/about'
);
```

<br>

### style.jsx

next.js 는 기본적으로 style jsx 기능을 지원한다.

```jsx
<style jsx>{`
  div{
    width: 100px;
    height: 100px;
  }
`}</style>
```

<br>

`style jsx` 태그를 컴포넌트에서 지정하면 해당 컴포넌트 내부의 스타일링이기 때문에 전역 스타일을 지정할수 없다.

공통되는 컴포넌트의 스타일링은 _app.js 을 생성해 공통 컴포넌트로 묶어서 처리할 수 있다.

<br>

### _app.js

next.js 환경에서 모든 페이지에 기본적으로 사용되는 마스터 페이지이다.

공통적으로 반복되는 내용을 작성한다.

```javascript
function App({ Component, pageProps }) {
  return (
    <>
      <공통컴포넌트 />
      <Component {...pageProps} />
    </>
  )
};
```

Component 는 각 페이지에서 출력되는 컴포넌트를 의미하며 pageProps 로 페이지 호출 시의 Props 를 출력 컴포넌트에 전달한다.

공통적인 스타일 또한 App 컴포넌트 내부에서 global 하게 지정할 수 있다.

```javascript
function App({ Component, pageProps }) {
  return (
    <>
      <공통컴포넌트 />
      <Component {...pageProps} />
      <style jsx global>{`
        *{
          margin: 0;
          padding: 0;
          box-sizing: border-box;
        }
      `}</style>
    </>
  )
};
```

<br>

#### 코드 분리

컴포넌트에 css와 공통 컴포넌트를 불러오는 코드를 모두 작정해 내용이 너무 길어지면 가독성이 떨어지므로 파일을 분리한다.

**css**

styles 폴더에 css 파일을 별도로 생성해 해당 파일을 컴포넌트에서 import 한다.

```javascript
// pages/_app.js
import '../styles/globals.css';

function App({ Component, pageProps }) {
  return (
    <>
      <공통컴포넌트 />
      <Component {...pageProps} />
    </>
  )
};
```

<br>

**컴포넌트**

_app.js 내에서 공통으로 불러오는 부분을 Layout 요소를 components/Layout.js 파일으로 이동시킨다.

```javascript
// components/Layout.js
import '../styles/globals.css';
import Layout from '../components/Layout';

function App({ children }) {
  return (
    <>
      <Header />
      {children}
      <Footer />
    </>
  )
};
```

_app.js 에서 Component 를 Layout 의 Prop 으로 전달해 Layout 의 children 부분에 출력되게 한다.

```javascript
// pages/_app.js
import Layout from '../components/Layout';

function App({ Component, pageProps }) {
  return (
    <>
      <Layout>
        <Component {...pageProps} />
      </Layout>
    </>
  )
};
```

<br>

