---
layout: post
title:  '.gitignore 생성 및 불필요한 파일 삭제'
date:   2022-09-26 01:40:03 +0900
comments : true
categories: Note
tags: [github, gitignore]
---


그동안 만든 파일들을 GitHub에 올려뒀는데,

언젠가부터 .DS_Store 등 이상한 파일들이 같이 올라가 있었다.

좀 신경쓰여서 찾아봤더니 .gitignore이라는 파일을 생성해서 그 안에 파일명을 입력하면 해당 파일은 올라가지 않는다고 한다.

<br>

그리고 난 쓸 줄 모르지...

<br>

하지만 [gitignore.io](https://gitignore.io)라는 사이트에서 이 파일을 자동으로 생성해 준다고 한다.

![gitignore 스크린샷](/img/posts/22-09-26/gitignore.png)

인풋창에 OS, IDE 등을 입력하면 코드? 로 보이는 문자들이 쭉 나오고 .gitignore 파일에 갖다 붙여 넣으면 된다.

<br>

그런데 또 하나 문제가 있었다.

나는 이미 GitHub를 사용중이라 이미 불필요한 파일이 올라가있던 것이다.

그래서 아래와 같이 이미 올라가 있는 파일을 지워주기로 한다.

<br>

```linux
git rm -r --cached .
git add .
git commit -m "Commit Message"
```

<br>

첫번째 줄로 local 제외한 git에서만 전체 다 삭제한다고 리스트에 올리고,

두번째 줄로 전체를 다시 올린다고 해서 리스트에서 제외시킨다.

이때 불필요한건 제외되지 않기 때문에 삭제로 남아있고

커밋하면 해당 파일들만 삭제되는 것 같다.

<br>

사실 처음봐서 잘 모른다. 대충 이런 느낌이지 않을까? 히히.

<br>
