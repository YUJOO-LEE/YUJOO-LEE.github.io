---
layout: post
title:  vite react app github 에 배포하기
date:   2022-12-13 13:58:43 +0900
comments : true
categories: Note
tags: [terminal, github, vite]
---

<br>

### vite config 설정

vite.config.js 파일 내 base 값을 설정한다.

base 값을 추가하고 레포지토리 이름을 적는다.

경로 설정이기 때문에 앞뒤로 `/` 를 꼭 붙인다.

```typescript
export default defineConfig({
  plugins: [react()],
  base: '/레포지토리이름/'
})
```

<br>

### deploy.sh 생성

프로젝트 root 폴더에 deploy.sh 파일을 생성해서 아래의 내용으로 저장한다.

```sh
#!/usr/bin/env sh
set -e
yarn run build  # npm 또는 yarn
cd dist
echo > .nojekyll
git init
git checkout -B master  # main 또는 master
git add -A
git commit -m 'deploy'
git push -f git@github.com:깃허브아이디/레포지토리.git master:gh-pages
cd -
```

npm 와 브랜치는 본인의 환경에 맞게 저장하면 된다.

<br>

### 배포 실행

터미널에서 아래 명령어로 위에서 생성한 deploy.sh 파일을 실행한다.

```terminal
sh deploy.sh
```

<br>

### 기타

- deploy.sh 은 실행용 파일이므로 굳이 github 에 올라가지 않도록 .gitignore 에 추가했다.

- github 의 setting > Pages 에서 설정된 branch 가 gh-pages 여야 한다.

<br>

참고 : [https://vitejs-kr.github.io/guide/static-deploy.html#building-the-app](https://vitejs-kr.github.io/guide/static-deploy.html#building-the-app)

<br>
