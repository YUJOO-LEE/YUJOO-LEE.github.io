---
layout: post
title:  styled components 설치 & 사용
date:   2022-11-02 14:35:21 +0900
comments : true
categories: Note
tags: [decodelab, react, library, terminal]
---

학원 수업 Day31

<br>

### styled components

styled components 를 활용해 개별 스타일을 지정한 컴포넌트를 만들 수 있다.

css 또는 scss 파일을 생성하지 않고 js 파일 내에서 스타일을 지정해서 컴포넌트에 입히는 방식이기 때문에 해당 컴포넌트에서 사용할 css만 정의해주면 사용, 관리하기 편리하다.

컴포넌트 js 파일 내에서 사용하게 되므로 props 값을 사용할 수도 있다.

<br>

#### 설치

```terminal
npm i styled-components
```

<br>

#### 스타일지정 요소 생성

```javascript
import styled from 'styled-components';
```

먼저 스타일을 적용할 컴포넌트에 styled-components 를 `import` 한다.

```javascript
const 태그이름 = styled.생성할요소`
	css 문법으로 스타일 지정
`;
```

함수 외부에서 태그 이름으로 태그를 생성한다.

**생성할 요소**는 div, article, li 등 만들어질 태그를 의미한다.

**태그 이름**은 생성된 요소의 이름이다.

임의로 지정하면 되지만 생성할 요소의 이름과 동일하게 지정할 수는 없다.


```javascript
const article = styled.article`
  ...
`;
```

위의 코드는 사용할 수 없다.

비슷하게 만들고 싶다면 변수 이름을 'Article' 과 같이 대문자로 지정하면 된다.

```jsx
<태그이름>
  내용
</태그이름>
```

JSX 문에서 위에서 정의했던 태그 이름으로 작성하면 해당 요소가 생성된다.

태그이름은 해당 컴포넌트 내부에서만 사용하는 이름이므로 개발자 도구 등으로 확인하면 해당 부분은 태그이름으로 작성되지 않고 `styled.생성할요소` 에서 지정한 요소로 생성이 된 것을 확인할 수 있다.

다른 컴포넌트 등에서 해당 요소를 사용할 때도 실제 요소 태그를 사용해야 한다.

```javascript
const Item = styled.article`
  ...
`;
```

```jsx
<Item>
  내용
</Item>
```

예시로, 위 코드를 개발자 도구에서 확인하면 Item 요소가 아닌 article 요소 인 것을 확인 할 수 있다.

<br>

#### css 스타일 지정

백틱(``) 내부에는 css 문법으로 작성하면 된다.

```javascript
const Item = styled.article`
  width: 100%;
  background-color: blue;
  color: #fff;
`;
```

위와 같이 작성하면 Item 요소를 생성하면서 해당 스타일이 입혀진다.

js가 아니라 css문법이므로 카멜 표기법으로 작성하지 않아도 된다.

<br>

#### 전역 스타일 지정

styled-components 는 reset 등과 같이 전역에서 사용할 style을 지정할 수도 있다.

먼저 GlobalStyle.js 파일을 생성해서 createGlobalStyle 을 `import` 하고, createGlobalStyle 을 사용해 style을 지정한 컴포넌트를 `export` 한다.

```javascript
import { createGlobalStyle } from "styled-components";

const GlobalStyle = createGlobalStyle`
  * {
    margin: 0; padding: 0; box-sizing: border-box;
  }
`;

export default GlobalStyle;
```

그리고 기본 화면인 App.js 에서 해당 컴포넌트를 `import` 해서 다른 컴포넌트보다 상위에 불러온다.

```jsx
<GlobalStyle />
<Header />
<Main />
```

이렇게 하면 GlobalStyle 에서 정의한 스타일이 전역으로 지정된다.

<br>

#### props 사용

styled-cumponents는 컴포넌트 js파일에서 사용하므로 props 값을 받아와서 편리하게 사용할 수 있다.

```jsx
<BoxWrap bg={process.env.PUBLIC_URL + '/경로/파일명.png'}>
  {children}
</BoxWrap>
```

먼저 생성할 요소에 props 로 전달을 원하는 값을 지정한다.

```javascript
const BoxWrap = styled.article`
  width: 100%;
  height: 100%;
  background: url(${(props) => props.bg}) no-repeat left center;  
`;
```

요소를 생성할 때 화살표 함수 형태로 props 값을 받아와 적용한다.

<br>

#### theme

theme 로 css 용 변수를 미리 지정해두고 전역에서 사용하도록 할 수 있다.

먼저 theme.js 파일을 생성하고 전역에서 사용을 원하는 값들을 미리 지정해서 `export` 한다.

```javascript
const 전역변수 = {
	키1: '값1',
	키2: '값2',
	키3: '값3'
};

export default deviceSize;
```

그리고 전역에서 사용하기 위해 app.js 파일로 가서 ThemeProvider 와 전역변수가 담긴 파일을 `import` 한다.

```javascript
import styled, { ThemeProvider } from 'styled-components';
import 전역변수 from './theme';
```

jsx 문에서 해당 변수를 사용할 컴포넌트를 ThemeProvider 로 감싸고 theme 로 객체를 넘긴다.

```jsx
<ThemeProvider theme={전역변수}>
  <Header />
  <Main />
</ThemeProvider>
```

컴포넌트 파일 내에서 style을 입힌 요소를 생성할 때 아래와 같이 해당 값을 사용할 수 있다.


```javascript
const SpanWrap = styled.span`
  color : ${({ theme }) => theme.키1};
`;
```

<br>

#### 미디어쿼리

미디어쿼리 또한 사용할 수 있다.

작성 방법은 일반적인 css와 동일하다.

```javascript
const NavWrap = styled.nav`
	width: 1180px;
	height: 100%;

	@media screen and (max-width: 1179px ) {		
		width: 100%;
	}

	@media screen and (max-width: 539px ) {	
		display: none;
	}
`;
```

<br>

