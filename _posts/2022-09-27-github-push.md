---
layout: post
title:  gitHub, sass 터미널 명령어 정리
date:   2022-09-27 18:36:23 +0900
comments : true
categories: Note
tags: [github, linux]
---


내가 쓰려고 정리해놓는 글..

<br>

### 기본 명령어

#### 원하는 경로로 이동

```linux
cd '폴더경로'
```

<br>

### sass 관련

#### 설치

```linux
sudo npm install sass -g
```

<br>

#### 기존 sass제거

```linux
sudo gem uninstall sass
```

<br>

#### 실시간 컴파일

```linux
sass --watch scss/style.scss:css/style.css
```

scss 폴더 내 style.scss을 css 폴더 내 style.css으로 자동 컴파일 시키겠다는 의미

실행 전 scss/style.scss가 있어야 하니 cd로 scss 폴더가 있는 경로로 이동해야함

<br>

### gitHub 관련

#### 깃 시작

```linux
git init
```

<br>

#### 저장소 연결

```linux
git remote add origin 저장소 주소
git config --global user.name 깃허브 아이디
git config --global user.email 깃허브 가입 이메일주소
```

저장소 주소 예시 : https://github.com/YUJOO-LEE/YUJOO-LEE.github.io.git

<br>

#### 상태확인

```linux
git status
```

<br>

#### 변동 사항 추가

```linux
git add . 
```

현재 폴더에서 변동사항 있는 모든 파일 stage에 올리기

<br>

#### 커밋

```linux
git commit -m "커밋메세지"
```

중간 저장 지점 등록

<br>

#### 푸시

```linux
git push origin master
```

커밋 내역 포함해서 깃허브에 업로드

<br>

#### 파일 삭제

```linux
git rm 파일명
git rm --cached 파일명
```

--cached 있으면 원격 파일만 삭제, 없으면 원격 + 로컬 파일 모두 삭제

<br>
