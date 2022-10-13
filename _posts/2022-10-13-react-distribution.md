---
layout: post
title:  React App 깃허브 배포하기
date:   2022-10-13 12:36:02 +0900
comments : true
categories: Note
tags: [github, react]
---


내가 쓰려고 남기는 방법

<br><br>
<hr>
<br><br>

### 1. 작업물 업로드

신규 repository 생성 후, [gitHub, sass 터미널 명령어 정리](/note/2022/09/27/github-sass.html)에 따라 작업물을 깃허브로 업로드 한다.

<br><br>
<hr>
<br><br>

### 2. package.json 수정

#### 2-1. 사이트 주소 추가

json파일을 열면 중괄호로 묶인 객체?가 하나 있는데 그 안쪽의 최하단에 아래 내용을 추가한다.

```json
"homepage": "https://사이트주소/레포이름/"
```

사이트 주소는 아이디.github.io 일 수도 있지만, 나의 경우 도메인을 연결 해 놓았으므로 `https://leeyujoo.com/레포이름/` 이 된다.

레포지토리 이름 뒤에는 `/`가 꼭 붙어야 한다.

추가하는 항목의 바로 앞 항목에 `,`가 없다면 추가 해 주어야 한다.

<br>

#### 2-2. scripts 추가

중간쯤에 script 내 아래 두 줄을 추가한다.

```json
"predeploy": "npm run build",
"deploy": "gh-pages -d build"
```

추가하면 script는 아래와 같이 수정된다.

```json
"scripts": {
  "start": "react-scripts start",
  "build": "react-scripts build",
  "test": "react-scripts test",
  "eject": "react-scripts eject",
  "predeploy": "npm run build",
  "deploy": "gh-pages -d build"
},
```

<br><br>
<hr>
<br><br>

### 3. gh-pages 설치

터미널에서 아래 명령어를 실행해서 gh-pages 패키지를 설치한다.

```npm
npm install gh-pages --save-dev
```

<br><br>
<hr>
<br><br>

### 4. deploy 실행

터미널에서 아래 명령어를 실행해서 빌드한다.

```npm
npm run deploy
```

<br>

완성!

<br>
