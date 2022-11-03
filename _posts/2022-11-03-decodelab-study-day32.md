---
layout: post
title:  Node Express로 MongoDB와 react 연결
date:   2022-11-03 12:49:02 +0900
comments : true
categories: Note
tags: [decodelab, node, terminal, mongoDB, react]
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

#### react

서버 구동 시 Hello world 대신 리액트 화면을 보여주기 위해 react 폴더로 이동해 [새로운 App을 생성](/note/2022/10/11/decodelab-study-day15.html#h-리액트)한다.

그리고 아래 명령으로 react app을 빌드시킨다.

```terminal
npm run-script build
```

<br>

#### sendFile

`sendFil()` 메서드로 react 에서 빌드한 파일을 서버 구동 시 보여줄 수 있다.

```javascript
const path = require('path');

app.use(express.static(path.join(__dirname, '../react/build')));
```

먼저 node폴더의 index.js 에서 `app.use()` 을 사용해 react/build 폴더까지의 경로를 static으로 지정한다.

```javascript
app.get('/', (request, response)=>{
  //response.send('Hello World'); 삭제
  response.sendFile(path.join(__dirname, '../react/build/index.html'));
});
```

기존 `app.get()` 내부의 `send()` 는 삭제하고 `sendFile()` 로 수정한 후 경로를 지정한다.

서버를 구동하면 react로 만들어진 화면이 보이는 것을 확인할 수 있다.

이 화면은 react에서 `build` 할 때마다 갱신된다.

<br>

### Mongo DB

Node.js로 DB에 연결하기 위해 [Mongo DB](https://www.mongodb.com/) 사이트에 회원가입 후 cloud DB를 생성한다.

그리고 편리하게 연결하기 위해 node 폴더에서 Mongoose를 설치한다.

```terminal
npm i mongoose --save
```

그리고 node 폴더의 index.js 의 `app.listen()` 부분을 아래와 같이 수정한다.

```javascript
app.listen(port, ()=>{
  mongoose.connect('mongodb+srv://<아이디>:<비밀번호>@cluster0.f8z7pj4.mongodb.net/?retryWrites=true&w=majority')
    // MongoDB 사이트에서 제공하는 주소
  .then(()=>
    // 성공 시 출력
    console.log(`Server app listening on port ${port} with MongoDB`)
  ).catch(
    // 실패 시 출력
    error=>console.error(error)
  );
});
```

<br>

### Proxy 설정

react의 포트에서 node의 8888번 포트의 서버가 응답을 받아야 하므로 중간에 proxy 중계설정을 해야한다.

react 폴더에서 아래 미들웨어를 설치한다.

```terminal
npm i http-proxy-middleware --save
```

proxy 설정을 위해 react / src 폴더에 setupProxy.js 파일을 생성해서 아래 코드를 입력한다.

```javascript
const { createProxyMiddleware } = require('http-proxy-middleware');

module.exports = (app) => {
	app.use(
		'/api',
		createProxyMiddleware({
			target: 'http://localhost:포트번호',
			changeOrigin: true,
		})
	);
};
```

`/api` 는 DB를 연결할 라우터 경로이다.

<br>

### axios

```terminal
npm i axios
```

DB의 데이터를 송수신하기 위해 react 에 axios를 설치한다.

설치 후 axios 로 데이터를 송신할 수 있다.

아래는 react 폴더의 app.js 에서 테스트해본 코드이다.

```javascript
import axios from 'axios';
import { useEffect } from 'react';
```

먼저 aixos 사용을 위해 axios 를 import 하고, 마운트 시 송신하기 위해 `useEffect()` 도 import 한다.

```javascript
const 데이터 = {
  name: 'yujoo'
}

useEffect(()=>{
  axios.post('/api/send', 데이터).then(response=>{
    console.log(response);
  })
  .catch(err=>{
    console.log(err);
  })
}, []);
```

위에서 지정했던 api 경로에서 더욱 세분화하기 위해 send 경로까지 지정해주었고 두번째 인자로 송신을 원하는 데이터를 넘긴다.

<br>

이제 클라이언트에서 보낸 데이터를 서버에서 받을 수 있다.

node 폴더의 index.js 에서 아래 코드로 클라이언트에서 보내는 데이터를 받도록 설정한다.

```javascript
app.use(express.json());
app.use(express.urlencoded({extended: true}));
```

그리고 같은 파일 하단에 아래 코드를 추가한다.

```javascript
app.post('/api/send', (request, response)=>{
  console.log(request.body);
  response.json({success: true, result: request.body});
})
```

post 를 사용해 '/api/send' 경로로 요청이 들어왔다면 내부 함수에서 결과를 처리한다.

여기에서 `request.body` 가 요청 들어온 값이다.

서버쪽의 코드이기 때문에 `console.log()` 를 사용하면 브라우저가 아닌 node 콘솔에 찍힌다.

`response.json()` 을 사용하면 클라이언트쪽으로 넘어가게 된다.

앞에 클라이언트 쪽에서 `then()` 구문으로 성공 시 결과값을 `console.log()` 로 출력시켰으므로 결과값이 브라우저에 보여지게 된다.


<br>

#### Schema

데이터베이스에 저장하기 전에 저장될 자료 형식이나 키값을 강제하는 시스템적인 틀인 스키마를 생성한다.

```javascript
const mongoose = require('mongoose');

const postSchema = new mongoose.Schema({
  title: String,
  content: String
});

const Post = mongoose.model('Post', postSchema);
module.exports = { Post };
```

객체형태로 각 키에 해당하는 값의 데이터 타입을 지정준다.

그리고 `mongoose.model()` 으로 저장될 collection 과 스키마를 지정한다.

export 시키면 데이터를 받아오는 node/index.js 에서 불러와서 사용할 수 있다.

<br>

#### DB로 데이터 전송

node/index.js 에서 생성한 Post 를 불러온다.

```javascript
const { Post } = require('./model/postSchema.js')
```

앞에 생성했던 `app.post()` 내에서 데이터를 받아 schema 객체로 생성한 후 `save()` 처리한다.

```javascript
const PostModel = new Post({
  title: request.body.title,
  content: request.body.content
})
```

생성된 객체 저장은 `객체.save()` 이나 결과 상태 반환을 위해 `then()` , `catch()` 문까지 추가해준다.

```javascript
PostModel.save()
  .then(()=>{
    response.json({success: true, result: request.body});
  })
  .catch(error=>{
    console.log(error);
    response.json({success: false})
  });
```

클라이언트에서 input 등으로 title, content 등의 값을 post로 보내면 DB에 저장이 되는 것을 확인할 수 있다.

<br>

