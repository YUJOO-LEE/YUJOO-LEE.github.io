---
layout: post
title:  Node-express, MongoDB, react로 게시판 만들기 (회원 정보 연동)
date:   2022-11-08 16:52:32 +0900
comments : true
categories: Note
tags: [decodelab, node, mongoDB, react, axios]
---

학원 수업 Day35

기존에 만들어 둔 게시판에 회원 정보를 연동시켜 작성자를 출력하고 동일한 작성자일 때만 수정, 삭제 버튼이 출력되도록 한다.

<br>

### 게시글 작성


#### front-end

글 작성 컴포넌트 Create.js 에서 현재 저장된 유저 정보를 받아온다.

```javascript
import { useSelector } from 'react-redux';

const User = useSelector(store=> store.user);
```

정보를 저장하는 item 객체에 회원 고유값(여기서는 uid)을 합쳐서 back-end 로 보낸다.

```javascript
const item = {
  ...

  uid: User.uid
};
```

<br>

#### back-end

게시글 관련 스키마인 postSchema.js 파일에서 writer 를 추가한다.

유저 데이터가 저장된 Users 컬렉션을 참조하도록 type 과 ref 로 이루어진 객체 형태를 데이터타입으로 지정한다.

```javascript
const postSchema = new mongoose.Schema({

  ...

  writer: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'Users'
  }
}, { collection: 'Posts'});
```

<br>

communityRouter.js 로 명명한 게시판 관련 요청을 처리하는 라우터에서 글 작성 부분을 찾아 User 데이터를 검색해 추가한다.

Counter 에서 communityNum 값을 찾아 추가한 이후, 추가적으로  전달받은 uid 값으로 User 데이터를 찾아 추가한 후 저장한다.

```javascript
...
const temp = request.body;
temp.communityNum = doc.communityNum;

User.findOne({uid: temp.uid})
  .exec()
  .then(doc=>{
    temp.writer = doc._id;
    const PostModel = new Post(temp);

    PostModel.save()
      ...
  })
```

<br><br>
<hr>
<br><br>

### 작성자 표시

#### back-end

communityRouter.js 의 read 와 detaile 등 데이터를 출력하는 부분에서, 데이터를 찾은 뒤 실행하기 전 `populate()` 메서드로 참조한 데이터를 객체 형태로 치환한다. 

```javascript
router.post('/read', (request, response)=>{
  Post.find()
    .populate('writer') // writer 데이터 객체로 치환
    .exec()
    .then(doc=>{
      ...
    })
})
```

<br>

#### front-end

출력을 원하는 컴포넌트에서 참조한 데이터의 key 값을 사용해 출력한다.

```jsx
<Item key={post._id}>

  ...

  <span>
    {post.writer.displayName} 
    {/* 작성자 이름 출력 */}
  </span>
</Item>
```

<br><br>
<hr>
<br><br>

### 작성자만 수정, 삭제 노출

먼저 store 에 저장된 현재 로그인 된 사용자의 정보를 받아온다.

```javascript
import { useSelector } from 'react-redux';
const User = useSelector(store=>store.user);
```

현재 사용자 정보의 uid와 게시글 정보의 uid가 일치하면 버튼이 출력되도록 한다.

게시글 데이터가 참조한 uid를 불러오기 위해 list 부분의 작성자 표기를 위한 설정과 같이 back-end 의 라우터에서 detail 값 요청 부분에 `populate()` 로 writer 참조 데이터를 객체형태로 치환해주어야 한다.

```jsx
{User.uid === Detail.writer.uid &&
  <BtnSet>
    <button><Link to={`/edit/${Detail.communityNum}`}>EDIT</Link></button>
    <button onClick={handleDelete}>DELETE</button>
  </BtnSet>
}
```

<br>

