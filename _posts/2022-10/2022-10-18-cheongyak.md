---
layout: post
title:  청약닷컴 UI제작로그 Day2
date:   2022-10-18 23:38:12 +0900
comments : true
categories: Note
tags: [cheongyak.com, react, portfolio, scss]
---

청약닷컴 페이지 만들면서 있었던 좌충우돌 제작로그

<br>

### BrowserRouter

청약닷컴은 단순히 포트폴리오 용이 아니라 실제로 서비스를 할 예정이기 때문에 SEO에서 불리한 HashRouter를 쓰는 것 보다는 BrowserRouter를 사용하는 것이 좋겠다고 판단했다.

새로고침하면 404에러가 뜨지만 실제 서버에서는 세팅하면 되겠지.

```npm
npm i react-router-dom
```

우선 react router dom 패키지를 설치 해 준다.

```javascript
import ReactDOM from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import App from './App';

ReactDOM.createRoot(root).render(
  <BrowserRouter basename={process.env.PUBLIC_URL}>
    <App />
  </BrowserRouter>,
);
```

index.js에서 root에 브라우저 라우터를 생성한다.

실제 서버에 올리기 전에 github에 우선 배포했는데, basename을 지정해 줘야 페이지가 보였다.

<br>

```javascript
import { Route, Routes } from 'react-router-dom';

function App() {
  return (
    <Routes>
      <Route path="/" element={<IndexSelect />} />
      <Route path="/list" exct="false" element={<ArticleList />} />
    </Routes>
  );
}

export default App;
```

App.js 에서 실제로 route를 분리해준다.

라우터가 기본으로 최신 버전으로 설치 되었는데, 설치된 6버전은 route에 기본적으로 exct 가 붙는다고 한다.

- exct : 하위 경로 없이 정확히 해당 경로일때만 컴포넌트 불러오기

또한 6버전은 switch가 routers로 바뀌었다고 한다.

<br>

```javascript
import { Link } from 'react-router-dom';
```

```jsx
<Link to={'/'}>
  <Logo></Logo>
  <span>청약닷컴</span>
</Link>
<Link to='/list?state=0'>
  <span>청약예정지</span>
</Link>
```

링크로 원하는 경로를 지정해서 이동하면 해당 경로에 맞는 컴포넌트가 뜬다.

<br>

### useSearchParams

필터 조건에 따른 리스트 출력을 위해 url 쿼리 스트링을 활용해야 했다.

처음 해보는 거라 꽤 삽질함..

되는 대로 만든거라 나중에 비웃으면서 코드 수정할지도 모르겠다.

```javascript
import { Link, useSearchParams } from 'react-router-dom';

const [ searchParams, setSearchParams ] = useSearchParams();
const params = {
  'state': searchParams.get('state') || '',
  'area': searchParams.get('area') || '',
  'type': searchParams.get('type') || ''
}
```

먼저 useSearchParams로 파라미터 값을 받아왔다.

setSearchParams로 값을 변경시킬 수 있게 됐다. 

다만 setSearchParams를 사용하면 기존 값들은 날려버리고 새로운 값만 세팅되어버렸다.

내가 필요한 기능은 저장된 값들을 가지고 클릭시 해당 부분만 수정되어야 하기 때문에 값들을 미리 객체로 저장 해 주었다.

```javascript
const handleClick = (e, key, value) => {
  e.preventDefault();
  if (params[key+''] !== value + '') {
    setSearchParams({...params, [key]: value});
  } else {
    setSearchParams({...params, [key]: ''});
  }
}
```

클릭 시 이벤트로 a태그 기본기능을 막기 위해 e값을 받아왔고, 변경할 key와 value값도 받아왔다.

토글 효과를 위해 해당 key에 값이 없다면 추가하고, 값이 있다면 비워줬다.

```jsx
<li key={j}>
  <Link 
    onClick={(e)=>{handleClick(e, filter.key, j)}}
    className={params[filter.key] === j + '' ? 'on' : null}
  >
    {sub}
  </Link>
</li>
```

Link에서 클릭 이벤트를 실행하고, 저장된 객체 내 원하는 key와 해당 버튼 요소의 값을 비교해 클래스까지 붙여줬다.

<br>

key와 value는 무조건 문자열이므로 조건문에서 문자열로 형변환 시켜야 에러를 방지할 수 있다.

<br>
