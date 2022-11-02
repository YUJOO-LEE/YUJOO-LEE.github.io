---
layout: post
title:  gitHub 커밋 되돌리기, branch 생성
date:   2022-11-01 11:02:12 +0900
comments : true
categories: Note
tags: [terminal, github]
---

### 원하는 커밋 시점 보기

```terminal
git checkout 커밋아이디
```

gitHub 페이지 상에서 확인 가능한 commit 의 고유한 아이디를 입력해서 파일을 되돌린다.

<br>

### branch 생성

```terminal
git branch 브랜치명
```

새로운 branch를 생성한다.

이 때 현재 작업중인 파일이 해당 branch 에 업로드 된다.

<br>

### branch 이동

```terminal
git checkout 브랜치명
```

생성한 브랜치로 이동해서 작업한다.

<br>

### commit

#### 모든 브랜치 push

```terminal
git push origin --all
```

#### 원하는 브랜치만 push

```terminal
git push origin 브랜치명
```

<br>

### merge

특정 브랜치에서 작업한 내용을 다른 브랜치에 병합시킨다.

```terminal
git switch 이동할 브랜치 이름
git merge 병합하여 가져올 브랜치
```

<br>

