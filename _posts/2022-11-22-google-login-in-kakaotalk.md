---
layout: post
title:  구글 소셜로그인 카카오톡 인앱브라우저에서 403 오류
date:   2022-11-22 15:56:23 +0900
comments : true
categories: Note
tags: [firebase, error]
---

[인프런 강의](https://www.inflearn.com/course/%EB%A7%8C%EB%93%A4%EB%A9%B4%EC%84%9C-%EB%B0%B0%EC%9A%B0%EB%8A%94-%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C#)를 보며 [질문 답변 서비스](https://qna-yujoo-lee.vercel.app/)를 만들었다.

firebase 를 사용해 google 소셜로그인을 구현했는데, 카카오톡의 인앱브라우저로 열어서 로그인하려고 하니 로그인화면 대신 아래와 같이 403 에러가 반환되었다.

![구글 소셜 로그인 불가 스크린샷](/img/posts/22-11-22/google-login.jpeg)

<br>

그에 대한 카카오의 답변

> 안녕하세요.
> 구글 소셜로그인은 구글 내부 정책으로 인해,
> 안드로이드 인앱 브라우저를 사용하는 앱 에서의 로그인을 차단하고 있습니다.
> https://developers.googleblog.com/2016/08/modernizing-oauth-interactions-in-native-apps.html 14
> 
> 카카오톡의 브라우저는 안드로이드 인앱브라우저를 사용하여 제공되고 있어,
> 구글의 소셜로그인이 동작하지 않는 것 으로 이해하시면 되어요.
> 
> 네이버의 경우, 구글 소셜로그인이 정상적으로 되는 것을 저희 쪽 에서도 인지하고 있으나,
> 네이버는 자체 브라우저를 개발하여, 정상적으로 구글 소셜로그인이 가능 한 것 으로 예상됩니다.
> 카카오톡의 자체 인앱브라우저 지원은 현실적으로 어려움이 있어,
> 아쉽지만 톡 내 웹뷰에서의 구글 소셜로그인은 제한이 있을 수 밖에 없습니다.

출처 : [https://devtalk.kakao.com/t/oauth-403/117012/2](https://devtalk.kakao.com/t/oauth-403/117012/2)

<br>

결론적으로 아직까지는 링크를 복사해서 별도로 모바일 브라우저에서 여는 것 외에는 해결방법이 없다.

<br>
