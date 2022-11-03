---
layout: post
title:  node express 사용
date:   2022-11-03 12:49:02 +0900
comments : true
categories: Note
tags: [decodelab, node, terminal]
---

학원 수업 Day32

먼저 node 와 react 를 연결해서 사용할 예정이기 때문에 폴더 하나에 node, react 폴더를 생성한다.

gitHub 업로드시에도 node, react 두 폴더가 모두 업로드 될 수 있도록 상위 폴더에서 push 한다.

<br>

### node express

node 세팅을 위해 node 폴더로 이동해 node express 을 설치 후 세팅한다.

먼저 아래 명령어로 package.js 을 초기값으로 생성한다.

```terminal
npm init -y
```

그리고 아래 명령어로 express 를 설치한다.

```terminal
npm i express --save
```

같은 경로(node 폴더)에 index.js를 생성해서 아래 코드를 입력한다.

```javascript
const express = require('express');
const app = express();
const port = 8888;
// 포트 번호 지정

app.get('/', (request, response)=>{
  response.send('Hello World');
});

app.listen(port, ()=>{
  console.log(`Server app listening on port ${port}`);
});
```

port 번호는 임의로 지정한다.

```terminal
node index.js
```

서버를 구동한다.

아직 변경점 생길때마다 재구동이 필요하다.

```json
"scripts": {
  ...
  "start": "node index.js"
},
```

package.json 파일 내 scripts에 start 로 서버 구동 명령어를 지정해주면, 터미널에서 npm start 만 입력하면 서버 구동이 된다.

<br>

#### nodemon

서버 구동 후 변경점을 모니터링하고 있다가 변경점이 생기면 자동으로 서버를 재구동 시켜준다.

```terminal
npm i nodemon
```

npm 으로 nodemon 설치 후, package.json 파일 내 scripts에서 node 로 서버 구동을 시키던 부분을 nodemon 으로 수정한다.  

```json
"scripts": {
  ...
  "start": "nodemon index.js"
},
```

`npm start` 로 서버 구동을 시켜준 후, index.js 파일 내 `Hello World` 를 다른 내용으로 수정하고 새로고침하면 바로 반영이 되는 것을 확인 할 수 있다.

<br>

