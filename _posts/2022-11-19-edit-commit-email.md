---
layout: post
title:  커밋 이메일 일괄수정 (잔디 누락)
date:   2022-11-19 05:24:22 +0900
comments : true
categories: Scrapbook
tags: [github, terminal]
---

분명히 커밋을 했는데 잔디에 반영이 되지 않아서 확인 해 보니 이메일에 오타가 있었다.

계정에 잘못 입력된 이메일을 등록해두면 되지만 내 이메일도 아니고 근본적인 해결방법 또한 아니므로, 다른 방법을 찾아 보았다.

구글링 해 보니 커밋을 수정해 주어야 했는데 이미 수십개의 커밋이 잘못 되어있어 하나하나 수정하기는 번거로웠고, 그러던 중 일괄적으로 수정하는 방법을 찾아냈다.

수정하려고 하는 프로젝트의 root 폴더에서 아래 명령어를 그대로 입력하면 수정이 된다.

```terminal
git filter-branch --env-filter '
WRONG_EMAIL="잘못된이메일주소"
NEW_NAME="새로운 이름"
NEW_EMAIL="새로운이메일주소"

if [ "$GIT_COMMITTER_EMAIL" = "$WRONG_EMAIL" ]
then
  export GIT_COMMITTER_NAME="$NEW_NAME"
  export GIT_COMMITTER_EMAIL="$NEW_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$WRONG_EMAIL" ]
then
  export GIT_AUTHOR_NAME="$NEW_NAME"
  export GIT_AUTHOR_EMAIL="$NEW_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

> 출처 : [https://jhrun.tistory.com/276](https://jhrun.tistory.com/276)

<br>

그리고 아래 명령어로 push 한다.

```terminal
git push -f origin master
```

<br>

혹시나 깃허브 권고사항으로 Branch protection rules을 적용했다면 강제 푸시가 되지 않아 오류가 난다.

깃허브 사이트로 가서 해당 프로젝트의 레포지토리에서 Settings -> Branch -> Branch protection rules -> edit -> Rules applied to everyone including administrators 를 허용으로 저장하고 다시 push한다.

혹시나 싶어서 작업 완료 후 해당 세팅은 다시 비허용으로 변경했다.

<br>
