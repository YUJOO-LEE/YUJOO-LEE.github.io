---
layout: post
title:  heroku 배포
date:   2022-11-09 17:34:23 +0900
comments : true
categories: Note
tags: [decodelab, node, mongoDB, react, terminal]
---

학원 수업 Day36

node-express를 이용해 서버를 구축해서 DB를 연결한 동적인 페이지는 github에 배포할 수 없으므로 Heroku 를 이용했다.

<br>

### [Heroku](https://herokuapp.com/)

헤로쿠는 여러 프로그래밍 언어를 지원하는 Paas(Platform as a Service, 서비스로서의 플랫폼) 이다.

여기서 PaaS 가 무엇인지 궁금해졌다.

<br>

원래 직접 만든 App 을 다른 사람도 볼 수 있게 하려면 서버 컴퓨터를 세팅 해 두어야 그곳에 저장된 데이터를 볼 수 있었다.

하지만 최근에는(사실은 내가 모르는 오래 전 부터) 클라우드 컴퓨팅 서비스를 통해, 하드웨어나 소프트웨어 등의 자원을 직접 소유하지 않고 업체에서 제공하는 자원을 이용할 수 있게 되었다.

그리고 이 서비스는 이용하는 자원의 범위에 따라 크게 SaaS, IaaS, PaaS 로 분류한다.

<br>

#### SaaS

Software as a Service

모든 하드웨어와 소프트웨어가 세팅이 되어 있어서 사용자는 이미 만들어진 소프트웨어만 이용할 수 있는 서비스이다.

풀옵션인 집에 내 짐만 보관하고 꺼내는 형태이다.

대표적으로 네이버 클라우드, 드롭박스 등이 있다.

<br>

#### IaaS

서버, 스토리지 등 하드웨어 자원을 제공해서 하드웨어부터 이용할 수 있는 서비스이다.

땅을 임대받아 내 마음대로 집을 짓고 사는 형태이다.

아마존의 AWS 가 대표적인 IaaS 라고 한다.

<br>

#### PaaS

하드웨어적인 세팅은 업체에서 미리 해 두고, 해당 하드웨어에 맞는 소프트웨어를 사용자가 직접 개발, 구축해서 이용할 수 있는 서비스이다.

지어진 집을 임대 받았지만 인테리어는 내가 원하는대로 하고 사는 형태이다.

지금 이용할 Heroku 가 PaaS 이다.

<br>

Heroku 는 Node.js, Ruby, Java, PHP 등의 언어로 만들어진 소프트웨어를 설치, 배포 할 수 있도록 지원한다.

사용자는 환경, 보안 등을 신경 쓸 필요 없이 해당 언어로 만들어진 소프트웨어만을 설치해 사용하면 된다.

내가 만든 페이지는 node.js 로 구동되므로 배포용으로 적합했다.

(하지만 Heroku 서비스가 11월 중순 이후부터 유료화 된다고 한다..)

<br>

### heroku cli

heroku cli 에서 cli 는 Command Line Interface 로 텍스트 형태로 heroku 에 연결, 배포, 관리할수 있도록 하는 인터페이스이다.

터미널에서 아래 명령어로 heroku cli를 설치한다.

```terminal
brew tap heroku/brew && brew install heroku
```

<br>

### config Vars

Heroku 사이트 내 Settings 탭의 Config Vars 메뉴에서 공개되면 안되는 비밀번호, App Key 등을 환경변수로 설정한다.

App 내에서 `process.env.이름` 의 형태로 해당 값을 불러올 수 있다.

MongoDB 접속 정보를 가리기 위해 해당 URI 를 환경변수 MONGO_URI key 의 값으로 설정했다.

<br>

### 환경 변수 설정

개발중인 node 폴더 안에 config 폴더를 만들어서 dev.js, product.js, key.js 파일을 생성한다.

key.js 파일에서 배포상태인지 개발상태인지를 인지해 개발버전에서는 dev.js를 배포버전에는 product.js를 실행하도록 한다.

```javascript
if (process.env.NODE_ENV === 'production') {
	//배포상태
	module.exports = require('./product.js');
} else {
	//개발상태
	module.exports = require('./dev.js');
}
```

dev.js, product.js 각각의 파일을 설정한다.

product.js 파일은 배포용이므로 환경변수 process.env.MONGO_URI를, dev.js 파일에는 실제 URI를 넣는다.

```javascript
module.exports = {
  mongoURI: process.env.MONGO_URI
}
```

그리고 이 key.js 를 node 의 index.js 파일 최상단에 불러온다.

```javascript
const config = require('./config/key.js');
```

mongoose 에 연결하는 URI 부분에 해당 값을 사용한다.

```javascript
app.listen(port, ()=>{
  mongoose
    .connect(config.mongoURI) // 기존 직접적인 URI 에서 수정
    ...
});
```

<br>

### 폴더 구조 변경

기존에는 하나의 프로젝트 폴더에 node, react 폴더를 따로 생성해서 back-end와 front-end 파일을 분리했지만, Heroku 배포를 위해 하나의 App 으로 합쳐야 한다.

1. node 폴더를 App 폴더로 변경한다.

2. react 폴더를 App 폴더 안으로 이동시킨다.

3. App 내부에 server 폴더 생성 후, index.js 와 package.json, package-lock.json 을 제외한 나머지 back-end 용 폴더(config, model, router)를 server 폴더로 넣는다.

결국 App 내부에 index.js, package.json, package-lock.json 파일과 react, server 폴더와 back-end 용 node_modules 가 있게 된다.

각 폴더의 위치가 바뀌었으니, react 의 node_modules 가 업로드 되지 않도록 **.gitignore** 를 수정한다.

이 때 /App/server/config/dev.js 의 mongoDB 연결 URI 도 노출되지 않아야 하므로 해당 경로도 추가한다.

index.js 에서 불러오는 파일들의 경로 또한 수정해야 한다.

<br>

### node 구동 설정

#### package.js

nodemon을 사용하면서 start 명령어를 지정해 주었던 것을 다시 node 로 수정한다.

```json
  ...
  "start": "node index.js"
  ...
```

<br>

#### Procfile

App 폴더 내부에 Procfile (확장자 없음) 파일을 아래와 같이 생성한다.

```
web: node index.js
```

<br>

#### index.js

포트 번호가 Heroku에 의해서 지정이 되므로 환경변수 값으로 설정한다.

환경변수 비어있으면 8888 으로 열리도록 설정했다.

```javascript
const port = process.env.PORT || 8888;
```

<br>

### 배포

먼저 heroku 에 로그인한다.

아래 명령어를 입력하면 브라우저에서 로그인 화면이 출력된다.

로그인 후 다시 terminal로 돌아온다.

```terminal
heroku login
```

<br>

react 폴더에서 아래 명령어를 실행해서 빌드한다.

```terminal
npm run-script build
```

<br>

App 외부, .gitignore 가 있는 프로젝트의 root 폴더에서 아래 명령어를 실행한다.

```terminal
git init
git add .
git commit -m '메세지'
heroku git:remote -a 프로젝트 이름
git subtree push --prefix App heroku main
```

<br>

배포가 완료되면 https://프로젝트 이름.herokuapp.com 으로 접속이 가능하다.

<br>

### 수정 후 재배포

수정 작업을 완료했다면 다시 빌드해서 배포한다.

1. react 폴더에서 빌드한다.

```terminal
npm run-script build
```

2. root 폴더로 이동해 모두 커밋한다.

```terminal
git add .
git commit -m '메세지'
```

3. 재배포한다.

```terminal
git subtree push --prefix App heroku main
```

<br>

### not found 에러

빌드 후 배포를 완료했는데 자꾸 not found가 나와서 뭐지 싶었다.

알고보니 빌드 시 github에 매번 파일이 올라갈 필요가 없어서 .gitignore 에서 build 폴더를 추가해뒀더니 heroku에서도 안올라가서 발생한 오류였다.

build 폴더를 업로드 제외 리스트에서 빼서 업로드 되도록 해야한다.

<br>

