---
layout: post
title:  Node-express, MongoDB, react로 게시판 만들기 (수정, 삭제 구현)
date:   2022-11-07 11:33:03 +0900
comments : true
categories: Note
tags: [decodelab, node, mongoDB, react, axios]
---

학원 수업 Day34

[지난 시간](/note/2022/11/04/decodelab-study-day33.html)에 이어서 수정, 삭제 기능을 구현했다.

<br>

### 게시글 수정

#### back-end

먼저 edit로 호출이 들어오면 같이 넘어온 정보들을 temp 에 저장한다.

```javascript
router.post('/edit', (request, response)=>{
  const temp = {
    title: request.body.title,
    content: request.body.content
  }
})
```

`updateOne()` 메서드를 이용해 데이터를 수정한다.

첫번째 인자로 원하는 조건(고유 id값, 여기서는 communityNum 값)으로 수정할 Document를 하나 검색하고, 두번째 인자에 $set으로 데이터를 넘긴다.

$set을 사용해야 현재 내용을 복사한 후, 넘기는 데이터의 key에 해당되는 데이터만 교체된다.

예를들어, temp에 title이라는 key만 있었다면, title만 수정되고 content는 이전 상태 그대로 유지된다.

$set을 사용하지 않으면 데이터가 일부 수정되는 것이 아니라 통채로 갈아치워 버린다.

title 값만 넘긴다면 content 등 나머지 데이터는 지워지고 title 만 남게 된다.

```javascript
router.post('/edit', (request, response)=>{

  ...

  Post.updateOne({communityNum: request.body.num}, {$set: temp})
    .exec()
    .then(()=>{
      response.json({success: true});
    })
    .catch(error=>{
      console.error(error);
      response.json({success: false});
    })
})
```

<br>

#### front-end

`import` 부분 생략

<br>

```javascript
const params = useParams();
const navigate = useNavigate();
const [ Detail, setDetail ] = useState({});
const [ Title, setTitle ] = useState('');
const [ Content, setContent ] = useState('');
const [ Loaded, setLoaded ] = useState(false);
```

먼저 쿼리스트링으로 전달 받은 현재 글번호를 불러오기 위해 `useParams()`, 글 저장 후 화면 전환을 위해 `useNavigate()`를 사용한다.

State의 Detail은 기존 글 내용, Title과 Content는 각 input 요소에 입력된 수정할 내용, Loaded는 로딩 화면 구성을 위해 만든다.

그리고 마운트 시 쿼리스트링에 서 받아온 num 값으로 현재 글 번호에 해당되는 데이터를 받아와서 Detail State 값으로 저장한다. 

```javascript
useEffect(()=>{
  const item = {num: params.num};
  axios.post('/api/community/detail', item)
    .then(respons=>{
      if (respons.data.success) {
        console.log(respons.data.detail);
        setDetail(respons.data.detail);
      }
    })
    .catch(error=>{
      console.error(error);
    })
}, [])
```

받아온 데이터를 각 input 요소의 값으로 출력한다.

데이터를 출력한 후 Loaded State 값을 변경한다.

```javascript
useEffect(()=>{
  setTitle(Detail.title);
  setContent(Detail.content);
  Detail.title && setLoaded(true);
}, [Detail])
```

데이터를 받아오는 부분의 스크립트 작성 후 jsx에서 실제로 데이터를 출력한다.

Loaded State의 값에 대한 조건문으로 로딩이 완료되지 않았다면 로딩 화면을, 완료 되었다면 데이터를 출력하도록 작성한다.

input 에는 value 값이 들어가고, onChange 이벤트로 State 값을 현재 input 에 입력된 값으로 변경하도록 한다.

```jsx
{Loaded ?
  <>
    <ul>
      <li>
        <label htmlFor='title'>Title</label>
        <input type='text' id='title' 
          defaultValue={Title}
          onChange={(e)=>setTitle(e.target.value)}
        />
      </li>
      <li>
        <label htmlFor='content'>Content</label>
        <textarea type='text' id='content' 
          cols='30' rows='4'
          defaultValue={Content}
          onChange={(e)=>setContent(e.target.value)}
        />
      </li>
    </ul>
  </>
: <p>Loading...</p>
}
```

input form 하단에 cancel과 update 버튼을 만든다.

`useNavigate()` 가 연결된 `navigate()` 의 인자로 -1 을 보내면 history에 저장된 현재로부터 바로 직전 페이지로 이동한다.

update 버튼에는 이벤트 핸들을 연결한다.

```jsx
<button onClick={()=>navigate(-1)}>Cancel</button>
<button onClick={handleUpdate}>Update</button>
```

연결한 이벤트 핸들 코드를 작성한다.

먼저 데이터를 저장하기에 앞서 저장할 데이터가 비었는지 확인한 후, 값이 있다면 객체에 담는다.

```javascript
if (!Title.trim() || !Content.trim()) return alert('제목과 본문을 모두 입력하세요.');
const item = {
  title: Title,
  content: Content,
  num: params.num
};
```

객체에 담은 데이터를 axios를 이용해 back-end로 보내서 저장 후 결과값에 따라 처리한다.

```javascript
axios.post('/api/community/edit', item)
  .then(response=>{
    if (response.data.success){
      alert('글 수정이 완료되었습니다.');
      navigate(`/detail/${params.num}`);
    } else {
      alert('글 수정이 실패했습니다.');
    }
  })
  .catch(err=>{
    console.log(err);
  })
```

글 수정이 성공했다면 `navigate` 로 글 내용 화면으로 이동한다.

<br><br>
<hr>
<br><br>

### 게시글 삭제

글 수정의 updateOne() 대신 deleteOne() 을 사용한다.

첫번째 인자로 넣은 key값에 해당하는 데이터를 찾아 하나 지운다.

**인자로 넣은 데이터를 찾지 못하면 가장 첫번째 데이터를 지워버리니 정확한 값을 전송하도록 주의한다.**

#### back-end

```javascript
router.post('/delete', (request, response)=>{
  Post.deleteOne({communityNum: request.body.num})
    .exec()
    .then(()=>{
      response.json({success: true});
    })
    .catch(error=>{
      console.error(error);
      response.json({success: false});
    })
})
```

<br>

#### front-end

이미 만들어진 게시글 내용 페이지에 삭제 버튼을 추가하고 이벤트 핸들을 연결한다.

```jsx
<button onClick={handleDelete}>DELETE</button>
```

삭제 여부를 재확인한 후 axios 를 통해 back-end 로 삭제 호출 후, 성공 응답이 돌아오면 `navigate()` 로 목록 화면으로 이동한다.

```javascript
const handleDelete = ()=>{
  if (!window.confirm('정말 삭제하시겠습니까?')) return;

  axios.post('/api/community/delete', item)
    .then(response=>{
      if (response.data.success) {
        alert('게시글이 삭제되었습니다.');
        navigate('/list');
      } else {
        alert('게시글 삭제에 실패했습니다.');
      }
    })
    .catch(error=>{
      console.error(error);
    })
}
```

간단한 게시판 구현 끝-

<br>
