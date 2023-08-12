---
layout: post
title:  vite 환경에서 환경변수 사용
date:   2023-08-12 21:41:05 +0900
comments : true
categories: Note
tags: [vite]
---

#### 용도 별 env 파일 생성

```text
.env                # 모든 상황에서 사용될 환경 변수
.env.local          # 모든 상황에서 사용되나, 로컬 개발 환경에서만 사용될(Git에 의해 무시될) 환경 변수
.env.[mode]         # 특정 모드에서만 사용될 환경 변수
.env.[mode].local   # 특정 모드에서만 사용되나, 로컬 개발 환경에서만 사용될(Git에 의해 무시될) 환경 변수
```

<br />

#### 환경변수 생성

vite 환경에서 사용할 변수는 앞에 `VITE_` 를 붙인다.

```text
VITE_TEXT_A=aaa
```

<br />

#### 사용

- javascript

  - `import.meta.env.VITE_TEXT_A` 으로 사용
  - env 파일 내 해당 변수가 없으면 `undefined` 반환

- html

  - `%VITE_TEXT_A%` 으로 사용
  - env 파일 내 해당 변수가 있어야 대체되며, 없으면 일반 텍스트로 노출됨

<br />

#### 참고 (vite 공식 가이드)

https://ko.vitejs.dev/guide/env-and-mode.html