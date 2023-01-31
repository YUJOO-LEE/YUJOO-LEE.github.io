---
layout: post
title:  README 파일내 뱃지 아이콘 추가
date:   2023-01-31 09:05:04 +0900
comments : true
categories: Note
tags: [markdown, github]
---

매번 검색하기 번거로워서 남겨두는 글.

<br>

README 파일 작성하면서 사용한 라이브러리/프레임워크를 뱃지 형태의 이미지로 나열했는데,

[shields.io](https://shields.io/) 에서 제공하는 url 로 원하는 뱃지를 만들어 사용할 수 있다.

<br>

```html
<img src="https://img.shields.io/badge/{표시텍스트}-{컬러코드}?style=flat-square&logo={로고}&logoColor=white"/> 
```

`{}` 표시 한 부분만 수정해서 사용한다.

<br>

`{컬러코드}` : HexCode 형태의 컬러코드

`{로고}` : 표시할 라이브러리 이름

`{표시텍스트}` : 뱃지에 표시할 텍스트 (라이브러리 이름을 동일하게 기재)

<br>

정확한 로고 출력용 라이브러리 이름과 컬러코드는 [simpleicons.org](https://simpleicons.org/) 사이트에서 손쉽게 찾을 수 있다.

<br>

아래는 기존에 사용한 뱃지들이다.

<img src="https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=Vite&logoColor=white"/>  `<img src="https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=Vite&logoColor=white"/>`

<img src="https://img.shields.io/badge/Next.js-000000?style=flat-square&logo=Next.js&logoColor=white"/> `<img src="https://img.shields.io/badge/Next.js-000000?style=flat-square&logo=Next.js&logoColor=white"/>`

<img src="https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=white"/> `<img src="https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=white"/> `

<img src="https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typeScript&logoColor=white"/> `<img src="https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typeScript&logoColor=white"/>`

<img src="https://img.shields.io/badge/ReactQuery-FF4154?style=flat-square&logo=ReactQuery&logoColor=white"/> `<img src="https://img.shields.io/badge/ReactQuery-FF4154?style=flat-square&logo=ReactQuery&logoColor=white"/>`

<img src="https://img.shields.io/badge/Styled-components-DB7093?style=flat-square&logo=styled-components&logoColor=white"/> `<img src="https://img.shields.io/badge/Styled-components-DB7093?style=flat-square&logo=styled-components&logoColor=white"/>`

<img src="https://img.shields.io/badge/ChakraUI-319795?style=flat-square&logo=ChakraUI&logoColor=white"/> `<img src="https://img.shields.io/badge/ChakraUI-319795?style=flat-square&logo=ChakraUI&logoColor=white"/>`

<img src="https://img.shields.io/badge/Web3.js-F16822?style=flat-square&logo=Web3.js&logoColor=white"/> `<img src="https://img.shields.io/badge/Web3.js-F16822?style=flat-square&logo=Web3.js&logoColor=white"/>`

<img src="https://img.shields.io/badge/Electron-47848F?style=flat-square&logo=Electron&logoColor=white"/> `<img src="https://img.shields.io/badge/Electron-47848F?style=flat-square&logo=Electron&logoColor=white"/>`

<img src="https://img.shields.io/badge/Firebase-FFCA28?style=flat-square&logo=Firebase&logoColor=white"/> `<img src="https://img.shields.io/badge/Firebase-FFCA28?style=flat-square&logo=Firebase&logoColor=white"/>`