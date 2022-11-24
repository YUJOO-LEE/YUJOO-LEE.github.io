---
layout: post
title:  Next.js Typescript에서 svg를 컴포넌트로 불러오기
date:   2022-11-24 14:10:02 +0900
comments : true
categories: Note
tags: [typescript, react, next, terminal, svgr, webpack]
---

react로 만든 app에 typescript를 적용했다.

그 과정에서 svg 파일이 컴포넌트 형태로 import 되지 않아, [어제](/note/2022/11/23/typescript-react.html) 아래 방법으로 해결했다.

<br>

### svg 컴포넌트 import

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

### Next.js 적용

> Element type is invalid: expected a string (for built-in components) or a class/function (for composite components) but got: undefined. You likely forgot to export your component from the file it's defined in, or you might have mixed up default and named imports.

단순히 react app 을 ts 로 적용하면 위에 방법만 사용해도 되지만, next.js 까지 적용하니 위의 문구와 함께 import 되지 않았다.

svg 를 사용하기 위해 [svgr 플러그인](https://www.npmjs.com/package/@svgr/webpack)을 사용했다.

```terminal
npm install @svgr/webpack --save-dev
```

설치 후, 프로젝트 root 경로에 `next.config.js` 파일을 생성하고 아래 코드를 적용한다.

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {

  webpack: config => {
    config.module.rules.push({
      test: /\.svg$/,
      use: [
        {
          loader: '@svgr/webpack'
        },
        {
          loader: 'file-loader',
          options: {
            name: 'static/[path][name].[ext]'
          }
        }
      ],
      type: 'javascript/auto',
      issuer: {
        and: [/\.(ts|tsx|js|jsx|md|mdx)$/]
      }
    });

    return config;
  }
};

module.exports = nextConfig;
```

재실행하면 정상적으로 import 되어 svg 이미지가 출력된다.

<br>

