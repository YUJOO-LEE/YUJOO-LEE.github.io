---
layout: post
title:  청약닷컴 UI제작로그 Day1
date:   2022-10-15 19:42:04 +0900
comments : true
categories: Note
tags: [cheongyak.com, react, portfolio]
---

청약닷컴 페이지 만들면서 있었던 좌충우돌 제작로그

<br>

### width: auto; 애니메이션

index의 텍스트 로고 부분에 애니메이션 효과를 주고 싶었다.

```css
@keyframe index-logo {
  0% {
    width: 0;
  }
  100% {
    width: auto;
  }
}
```

처음에는 위와 같이 하면 당연히 될 줄 알았는데, 애니메이션을 입히면 애니메이션이 작동되지 않고 0에서 바로 auto로 적용이 되었다.

<br>

**해결방법**

```css
/* Logo 부분 */
span{
  width: auto;
  animation: index-logo 1s linear 1;
}

/* keyframe 부분 */
@keyframe index-logo {
  0% {
    max-width: 0;
  }
  100% {
    max-width: 200px;
  }
}
```

로고 자체의 width값에 auto를 주고 키프레임에서는 max-width로 주는 방법이었다.

100%의 max-width에는 실제 가로 사이즈를 살짝 넘을만한 사이즈를 주었다.

100%의 max-width를 크게 주어도 실제 width는 auto이므로 그 이상 커지지는 않아서 auto값에 맞춰지는 현상을 이용한 것이다.

다만 너무 크게 하면 지정한 시간 안에 큰 사이즈를 만들어야 하므로 지정한 시간보다 훨씬 빠른 속도로 작동되어서 유사한 가로값을 넣어주는 것이 좋다.

<br>

### 웹폰트 적용

기존에는 웹폰트 적용을 위해서 [구글 폰트](https://fonts.google.com/)에서 제공하는 코드로 편리하게 적용했지만 구글 폰트에는 한글 폰트가 그리 많지 않아서 네이버 무료 폰트를 이용했다.

그 중 최근에 나온 [나눔 스퀘어 네오](https://campaign.naver.com/nanumsquare_neo/)가 예뻐 보여서 다운로드 받아 페이지에 적용했다.

다운로드 받아서 보면 웹폰트와 PC용 폰트로 나누어져 있는 것을 확인할 수 있다.

그 중 웹폰트 폴더를 열어보면 폰트 파일이 많은데, 임의로 가변이 가능한 Variable font 파일이 있어서 해당 파일들을 사이트 경로에 추가했다.

파일 확장자는 `eot`, `woff`, `woff2` 로 브라우저 별로 사용되는 파일이 다른 것 같다.

세개 다 불러와서 사용했다.

eot파일이 가장 오래되어서 구형 브라우저에서도 지원이 된다고 한다. 그래서 가장 첫 번째로 넣었다.

```css
@font-face {
  font-family: "NanumSquareNeo";
  src: url("../fonts/NanumSquareNeo-Variable.eot"),
       url("../fonts/NanumSquareNeo-Variable.woff2") format("woff2"),
       url("../fonts/NanumSquareNeo-Variable.woff") format("woff");
}
```

자세한 weight별 폰트 디자인은 `index_withVF.html` 파일에서 확인할 수 있다.

<br>

### mixin 매개변수 사용

js의 함수는 매개변수를 받아서 하나의 함수를 여러 방면으로 다양하게 재활용 할 수 있는데 scss의 mixin도 미리 스타일을 저장해두고 재사용하기 위해 나온 명령이라 매개변수를 받을 수 있지 않을까 해서 찾아보니 역시나 가능했다.

웹폰트의 font-weight를 조정하기 위해 두개의 값을 주어야 하는데, 그걸 한줄로 사용하기 위해 mixin으로 만들어서 include로 불러와 적용시켰다.

아래 코드의 `font-variation-settings: 'wght` 가 variable 폰트의 wight를 지정하는 방법이다.

```scss
/* mixin 부분 */
@mixin font-weight($weight: 300) {
  font-weight: $weight;
  font-variation-settings: 'wght' $weight;
}

/* include 부분 */
span{
  font-family: $font-neo;
  @include font-weight(500);
}
```

js 함수 사용과 비슷하게 `@include mixin식별자(인수)`로 넘기고 `@mixin 이름(매개변수 식별자: 기본값)`으로 받아서 사용할 수 있다.

`@include` 시 인수가 없으면 기본값으로 적용이 된다.

<br>

### 절대경로 import

리액트에서 import시 상대경로를 이용하게 되는데, [Create React App](https://create-react-app.dev/docs/importing-a-component/#absolute-imports)의 문서에 따라 손쉽게 절대경로를 React APP내 src폴더로 지정할 수 있었다.

프로젝트 root에 `jsconfig.json` 파일을 생성하고 아래 내용을 입력하고 저장하기만 하면 된다.

```json
{
  "compilerOptions": {
    "baseUrl": "src"
  },
  "includes": ["src"]
}
```

그리고 include에서 불러올 때 경로의 앞부분에 `/`를 쓰지 말고 폴더명만 쓰면 src폴더 내부의 해당 경로의 파일을 불러온다.

```javascript
import Logo from "components/common/Logo";
```

훨씬 편한 것 같다.

<br>

### svg 컴포넌트

```javascript
import {ReactComponent as LogoSvg} from 'img/logo.svg';

export default function Logo() {
  return (
    <i>
      <LogoSvg />
    </i>
  );
}
```

로고 파일을 다양하게 사용하기 위해 Adobe Illustrator에서 svg로 저장했다.

매번 이미지 파일을 불러서 넣는 것 보다 컴포넌트화 하면 css에서도 활용하기 좋을 것 같아서 컴포넌트화 했다.

import시 `{ReactComponent as 식별자}`로 불러오면 손쉽게 JSX 태그로 불러올 수 있다.

그걸 i태그로 감싸서 추후 사용이 용이하도록 했다.

<br><br>
