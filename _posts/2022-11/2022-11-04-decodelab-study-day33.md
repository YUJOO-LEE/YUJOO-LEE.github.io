---
layout: post
title:  Node-express, MongoDB, react로 게시판 만들기
date:   2022-11-04 15:49:02 +0900
comments : true
categories: Note
tags: [decodelab, node, mongoDB, react, axios]
---

학원 수업 Day33

mongoDB를 활용해 본격적으로 커뮤니티 게시판을 생성했다.

<br>

### 게시글 출력

#### back-end

```javascript
app.post('/api/read', (request, response)=>{
  Post.find()
    .exec()
    .then(doc=>{
      response.json({success: true, list: doc});
    })
    .catch(error=>{
      console.log(error);
      response.json({success: false});
    })
})
```

node/index.js에서 find(조건) 메서드를 사용해서 불러올 조건을 지정한다.

비워두면 전체 데이터를 불러오게 된다.

exec() 는 조건을 실행해 데이터를 받아온 후 Promise 객체를 반환한다.

성공적으로 불러오면 데이터를 반환해야 하기 때문에 then() 구문 내부에 성공시 반환값을 지정한다.

then은 파라미터로 반환된 데이터를 받아오니 해당 데이터를 임의의 키값으로 (여기서는 list) 반환시켜 주도록 한다.

<br>

#### front-end

List 컴포넌트를 생성해서 back-end에서 보낸 리스트 데이터를 axios로 불러와야 한다.

```javascript
import axios from 'axios';
import { useEffect, useState } from 'react';
import { Link } from 'react-router-dom';
```

마운트 시 axios 로 통신하기 위해 useEffect와 axios를, 리스트를 state에 담기 위해 useState를, 클릭 시 내용 페이지로 연결하기 위해 Link 를 `import` 한다.

<br>

```javascript
const [ List, setList ] = useState([]);

useEffect(()=>{
  axios.post('/api/read')
    .then(response=>{
      if (response.data.success) {
        console.log(response.data.communityList.length);
        setList(response.data.communityList);
      }
    })
    .catch(error=>{
      console.log(error);
    })
}, []);
```

node에서 설정한 경로 (/api/read) 로 연결하면 데이터를 불러오게 된다.

불러온 데이터를 state에 담는다.

<br>

```jsx
<Layout name='List'>
  {List.map(post=>{
    return (
      <article key={post._id}>
        <h2>
          <Link to={`/detail/${post.communityList}`}>
            {post.title}
          </Link>
        </h2>
      </article>
    )
  })}
</Layout>
```

불러온 데이터를 map을 돌며 뿌려준다.

key 값은 불러온 인덱스보다 고유한 데이터인 것이 좋다.

mongoDB에서 _id 로 고유 아이디 값을 생성하므로 key 값으로 지정한다.

클릭 시 해당 글의 detail 페이지로 이동하도록 Link 에 고유값을 추가한다.

자동으로 생성되는 _id 는 랜덤한 숫자구조로 노출되기에 적절.. 예쁘지 않아서 communityList라는 이름으로 고유값을 만들어서 사용할 예정이다.

<br><br>
<hr>
<br><br>

### 게시글 작성

#### back-end

리스트 출력 때 고유 아이디를 mongoDB에서 생성한 _id가 아닌 다른 보기 좋은 값으로 지정해주기로 했다.

해당 값을 DB에서 관리하기 위해 새로운 schema 를 생성해준다.

```javascript
const mongoose = require('mongoose');

const counterSchema = new mongoose.Schema({
  name: String,
  communityNum: Number
}, { collection: 'Counter'});
    // 값이 저장될 컬렉션 지정

const Counter = mongoose.model('Counter', counterSchema);
module.exports = { Counter };
```

CounterSchema.js로 명명해서 작성한 위 코드는 마지막 export 이름인 Counter로 호출하면 실행된다.

communityNum은 숫자 형태로 저장되며 게시글 작성 시 1씩 더해서 수정할 예정이다.

현재는 값이 없어 게시글 작성 시 데이터를 불러오지 못해 에러가 나므로 MongoDB 사이트에 가서 초기값을 지정해야 한다.

name 은 communityNum 값의 분류용으로 추가했고, 임의로 counter라고 지정했다.

communityNum 은 1으로 정해주었다.

주의할 점은 DB에서 insert 시 int32로 데이터타입을 지정해서 숫자값으로 저장되게 해야한다.

초기값 저장 후 다시 index.js로 돌아와 Counter를 호출하고, 해당값을 사용한다.


```javascript
app.post('/api/create', (request, response)=>{
  //Counter 에서 counter로 저장된 name을 찾아 해당 DOCUMENT 출력
  Counter.findOne({name: 'counter'})
    .exec()
    .then(doc=>{
      const PostModel = new Post({
        title: request.body.title,
        content: request.body.content,
        communityNum: doc.communityNum
        // 프론트에서 받은 데이터에 현재 Counter의 communityNum 값 추가
      })

      PostModel.save()
      .then(()=>{
        // 갱신 후 counter 의 communityNum 값 업데이트
        Counter.updateOne({name: 'counter'}, {$inc: {communityNum: 1}})
          .then(()=>{
            response.json({success: true});
          })
      })
      .catch(error=>{
        console.log(error);
        response.json({success: false})
      });
    })
})
```

게시글 저장 후 `updateOne( 찾을조건, 변경식 )` 으로 현재 communityNum의 값을 1 더해준다.

- $inc : 증가    
- $dec : 감소    
- $set : 새로운 값으로 변경

<br>

#### front-end

```javascript
import axios from 'axios';
import { useState } from 'react';
import { useNavigate } from 'react-router-dom';
```

input 값을 받아오기 위해 useState를, 작성 버튼 클릭 시 통신하기 위해 axios를, 데이터 작성 후 페이지를 이동시키기 위해 useNavigate 를 `import` 한다.

<br>

```javascript
const navigate = useNavigate();
const [ Title, setTitle ] = useState('');
const [ Content, setContent ] = useState('');

const handleCreate = ()=>{
  if (!Title.trim() || !Content.trim()) return alert('제목과 본문을 모두 입력하세요.');
  const item = {title: Title, content: Content};

  axios.post('/api/create', item)
  .then(response=>{
    if (response.data.success){
      alert('글 저장이 완료되었습니다.');
      navigate('/list');
    } else {
      alert('글 저장이 실패했습니다.');
    }
  })
  .catch(err=>{
    console.log(err);
  })
}
```

버튼 클릭 시 발생할 이벤트를 생성하고 빈 값을 저장하지 못하도록 조건문도 추가해준다.

그리고 `post()` 인자에 create로 경로를 지정하고 작성한 데이터를 두번째 인자로 보낸다.

<br>

```jsx
<Layout name='Post'>
  <ul>
    <li>
      <label htmlFor='title'>Title</label>
      <input type='text'
        name='title' id='title'
        value={Title}
        onChange={e=>setTitle(e.target.value)}
      />
    </li>
    <li>
      <label htmlFor='content'>Content</label>
      <textarea
        name='content' id='content'
        cols='30' rows='3'
        value={Content}
        onChange={e=>setContent(e.target.value)}
      ></textarea>
    </li>
    <li>
      <button
        onClick={handleCreate}
      >SEND</button>
    </li>
  </ul>
</Layout>
```

input값을 state로 지정하고, onChange시 state값이 변경되도록 이벤트를 지정한다.

<br><br>
<hr>
<br><br>

### 게시글 내용 출력

리스트에서 제목 글릭 시 게시글 내용 페이지로 이동해 글 내용을 불러온다.

#### back-end

```javascript
app.post('/api/detail', (request, response)=>{
  Post.findOne({communityNum: request.body.num})
    .exec()
    .then(doc=>{
      response.json({success: true, detail: doc});
    })
    .catch(error=>{
      console.log(error);
      response.json({success: false});
    })
})
```

리스트 출력과 비슷한 양식이나 find() 가 아닌 findOne() 으로 조건에 따른 한가지 document 만 불러온다.

쿼리스트링으로 document 별 고유값으로 만들어 줬던 communityNum 을 보내줄 예정이기 때문에 그 값을 받아와서 출력 조건으로 사용한다.

그리고 성공 시 detail 이라는 키값으로 반환한다.

<br>

#### front-end

```javascript
import axios from 'axios';
import { useParams } from 'react-router-dom';
import { useEffect, useState } from 'react';
```

마운트 시 데이터를 통신해야 하므로 useEffect와 axios, 받아온 데이터를 담을 useState, 그리고 리스트에서 클릭 시 넘겨준 쿼리스트링 값을 사용해야 하기 때문에 useParams 을 `import` 한다.

```javascript
const [ Detail, setDetail ] = useState(null);
const params = useParams();

const item = {
  num: params.num
}
```

`useState()` 로 State를 생성하고, `useParams()` 로 게시물의 고유 id 값을 받아온 쿼리 스트링을 params으로 가져와서 변수에 담아준다.


```javascript
useEffect(()=>{
  axios.post('/api/community/detail', item)
    .then(response=>{
      if (response.data.success) {
        console.log(response.data.detail);
        setDetail(response.data.detail);
      }
    })
    .catch(error=>{
      console.log(error);
    })
}, []);
```

마운트 시 데이터를 받아와야 하므로 `useEffect()` 으로 게시글 데이터를 가져오는 경로에 연결한다.

`post()` 의 두번째 인자값으로 앞에서 미리 정의해둔 쿼리스트링이 담긴 객체를 보내면 back-end 에서 해당 값으로 게시물을 검색해서 반환 해 줄 것이다.

반환된 데이터를 State 에 저장해서 사용하면 된다.

```javascript
<Layout name='Detail'>
  {Detail &&
    <>
      <h2>{Detail.title}</h2>
      <p>{Detail.content}</p>
    </>
  }
</Layout>
```

State 기본값이 null 이므로 출력 조건을 걸어주고 State 값이 null 이 아닐경우 데이터를 출력하도록 작성한다.

<br>

