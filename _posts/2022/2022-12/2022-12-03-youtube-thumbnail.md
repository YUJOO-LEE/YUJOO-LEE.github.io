---
layout: post
title:  유튜브 영상별 썸네일 주소
date:   2022-12-03 12:56:23 +0900
comments : true
categories: Note
tags: [youtube]
---

청닷에서 유튜브 썸네일 사용하면서 정리

### 스크린샷

```javascript
`http://i.ytimg.com/vi/${youtubeId}/0.jpg`
`https://img.youtube.com/vi/${youtubeId}/0.jpg`
```

<br>

### 처음, 중간, 끝

사이즈 : 120x90

```javascript
`http://i.ytimg.com/vi/${youtubeId}/1.jpg`
`http://i.ytimg.com/vi/${youtubeId}/2.jpg`
`http://i.ytimg.com/vi/${youtubeId}/3.jpg`
`https://img.youtube.com/vi/${youtubeId}/1.jpg`
`https://img.youtube.com/vi/${youtubeId}/2.jpg`
`https://img.youtube.com/vi/${youtubeId}/3.jpg`
```

<br>

### 썸네일

#### 120x90

```javascript
`http://i.ytimg.com/vi/${youtubeId}/default.jpg`
`https://img.youtube.com/vi/${youtubeId}/default.jpg`
```

<br>

#### 480x360

```javascript
`http://i.ytimg.com/vi/${youtubeId}/hqdefault.jpg`
`https://img.youtube.com/vi/${youtubeId}/hqdefault.jpg`
```

<br>

#### 320x180

```javascript
`http://i.ytimg.com/vi/${youtubeId}/mqdefault.jpg`
`https://img.youtube.com/vi/${youtubeId}/mqdefault.jpg`
```

<br>

#### 640x480

```javascript
`http://i.ytimg.com/vi/${youtubeId}/sddefault.jpg`
`https://img.youtube.com/vi/${youtubeId}/sddefault.jpg`
```

<br>

#### 1280x720/1920x1080

```javascript
`http://i.ytimg.com/vi/${youtubeId}/maxresdefault.jpg`
`https://img.youtube.com/vi/${youtubeId}/maxresdefault.jpg`
```

<br>
