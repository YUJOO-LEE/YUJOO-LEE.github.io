---
layout: post
title:  react App을 typescript로 바꾸기
date:   2022-11-23 11:12:03 +0900
comments : true
categories: Note
tags: [typescript, react, terminal]
---

만들던 React app을 타입스크립트로 바꿔보았다.

음... 타입스크립트 처음인데 처음부터 ts로 만들었으면 훨씬 편했을 것 같다.

ts 써보니 일단 타입이 뭔지 보여서 코드를 이해하기가 수월해진 것 같다.

<br>

### 설치

아래 명령어로 global 하게 한번만 설치한다.

```terminal
npm i -g typescript
```

나머지는 사용하는 라이브러리 따라서 설치한다.

```terminal
npm i @types/node @types/react @type/jest @types/react-dom @types/react-router-dom @types/react-redux
```

음 jest 는 뭔지 모른다. 히히

<br>

#### tsconfig.json 생성

프로젝트 root 폴더에서 아래 명령어로 typescript 용 config 파일을 생성한다.

```terminal
tsc init
```

<br>

#### 파일 수정

1. js 확장자를 ts로 변경한다.

2. jsx 로 DOM 을 반환하는 파일은 확장자를 tsx로 변경한다.

3. 빨간 줄이 그어진 곳에 타입을 지정해준다.

사실 이게 다였는데... 중간중간 생각지 못한 오류들이 반환되었다.

<br><br>
<hr>
<br><br>

### string key 에러

```typescript
const handleClick = (e: React.MouseEvent, key: string, value: string) => {
  e.preventDefault();
  if (param === 'content') {
    navigate(`/list?${key}=${value}`);
    return;
  }
  if (queries[key] !== value + '') {
    setSearchParams({...queries, [key]: value});
  } else {
    setSearchParams({...queries, [key]: ''});
  }
}
```

js 에서 잘 동작하던 코드가 에러가 난다.

key를 string으로 선언할 수 없다는 것인데, 나는 매개변수로 문자열을 받아서 key로 사용해야 했으므로 type 에 key를 string으로 주어 해결했다.

<br>

**사용 예시**

```typescript
type 타입 = {
  [키: string]: string  // 여기!
  식별자: string
}

const 객체: 타입 = {
  식별자: "값",
}

let 변수 = "식별자"

console.log(객체[변수]);
```

<br>

### svg 컴포넌트 import

svg 이미지 파일을 img 태그가 아닌 컴포넌트 형태로 import 해서 사용하고 있었는데, ts 를 적용하자 에러가 났다.

img 태그로 넣을 수도 있겠지만... 로고에 fill값을 주어 다양하게 사용하고 싶었다.

svg 파일을 컴포넌트로 사용하려면 ReactComponent 로 export 시켜주는 작업이 필요하다고 한다.

<br>

src 폴더에 아래 코드로 custom.d.ts 파일을 생성한다.

```typescript
declare module '*.svg' {
  import React = require('react');
  export const ReactComponent: React.FC<React.SVGProps<SVGSVGElement>>;
  const src: string;
  export default src;
}
```

<br>

tsconfig.json 파일 내 해당 파일을 include 에 포함한다.

compilerOptions 내부가 아니라 동일 선상에 있어야 한다.

```json
{
  "compilerOptions": {
    ...
  },
  "include": [
    "custom.d.ts",
  ],
}
```

<br>

원하는 곳에 svg 파일을 import 한다.

```typescript
import { ReactComponent as LogoSvg } from '../../img/logo.svg';

export default function Logo () {
  return (
    <LogoSvg />
  );
}
```

<br>

### querySelectorAll 타입지정

섹션의 높이값을 받아 배열로 저장해두고 이동 메뉴 클릭 시 해당 섹션으로 이동하는 기능을 사용하고 있었다.

```javascript
const frame = useRef(null);

const getMenus = () => {
  if (!frame.current) return;

  position.current = [];
  const menus = frame.current.querySelectorAll('.tabBody>div');
  for (const li of menus) {
    position.current.push(li.offsetTop - 50);
  }
  activation();
};
```

원래는 이렇게 된 코드였고 `useRef()` 부분에 `<HTMLDivElement>` 제네릭 타입을 넣어줬는데 for문을 돌리면서 뽑아낸 li들의 타입인 `Element` 에서 `offsetTop` 값을 가져올 수 없다는 에러가 났다.

`Element` 가 `Element` 인게 뭐가 잘못된거지 싶어서 한참 고민했는데 생각보다 쉬운 문제였다.

`querySelectorAll()` 부분에 제네릭으로 `<HTMLElement>` 을 지정하면 `for` 문으로 나오는 엘리먼트들이 `HTMLElement` 로 지정되며 `offsetTop` 값을 사용할 수 있다.

```typescript
const frame = useRef<HTMLDivElement>(null);

const getMenus = () => {
  if (!frame.current) return;

  position.current = [];
  const menus = frame.current.querySelectorAll<HTMLElement>('.tabBody>div');
  for (const li of menus) {
    position.current.push(li.offsetTop - 50);
  }
  activation();
};
```

<br>

