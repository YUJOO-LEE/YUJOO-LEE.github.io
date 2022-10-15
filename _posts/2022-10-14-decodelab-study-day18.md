---
layout: post
title:  학원 수업 내용 Day18
date:   2022-10-13 14:49:04 +0900
comments : true
categories: Note
tags: [decodelab, react]
---


### useState() 복습

학습했던 hook 사용법을 복습했다.

#### 조건문으로 State 변경

```javascript
let [ time, setTime ] = useState(1);

function btnSetTime() {
	time = time >= 12 ? 1 : time + 1;
	setTime(time);
}
```

```JSX
<div>
  <span>타이머 : {time}시</span>
  <button onClick={btnSetTime}>Update</button>
</div>
```

<br>

#### input값 받아서 리스트 추가

실시간으로 input의 입력값을 받아와서 버튼 클릭 시 배열에 추가하여 화면에 출력한다.

추가 후에는 인풋 내용을 비워준다.

```javascript
let they = ['오렌지', '딸기'];
const [fruits, setFruits] = useState(they);
const [input, setInput] = useState('');

console.log(setInput);

function insertInput(e) {
  setInput(e.target.value);
}

function onUpload() {
  setFruits((prev)=>{
    return [...prev, input];
  });

  setInput('');
}
```

```JSX
<div>
  <input type="text" value={input} onChange={insertInput} />
  <button onClick={onUpload}>Upload</button>
  {
    name.map((data, i)=>{
      return <p key={i}>{data}</p>;
    })
  }
</div>
```

<br>

### useEffect()

화면에 렌더링 되는 개체를 여러 개 만들어서 각자 렌더링 될 때마다 이벤트가 실행되도록 할 수 있다.

```javascript
const [count, setCount] = useState(1);
const [name, setName] = useState('');

function update() {
  setCount(count + 1);
}

function inputUpdate(e) {
  setName(e.target.value);
}

useEffect(() => {
  console.log('렌더링 되었습니다.');
})  // 매 렌더링 마다 발생

useEffect(() => {
  console.log('마운트 되었습니다.');
}, [])  // 마운트시에만 발생

useEffect(() => {
  console.log('count 렌더링 되었습니다.');
}, [count]) // count 렌더링 시에만 발생

useEffect(() => {
  console.log('name 렌더링 되었습니다.');
}, [name]) // name 렌더링 시에만 발생
```

```JSX
<div>
  <button onClick={update}>Update</button>
  <span>count : {count}</span>

  <input type="text" value={name} onChange={inputUpdate} />
</div>
```

<br>

### axios

vanilla JS로 제작할 때는 fetch를 배웠는데, react로 제작하면서 axios를 배웠다.

Axios | Fetch
---- | ----
써드파티 라이브러리 설치 필요 | 내장 라이브러리로 설치 불필요
---- | ----
호환성이 뛰어남 | 크롬 42+, firefox 39+, edge 14+, safari 10.1+ 지원 (polyfill 이용해서 하위 호환성 지원 가능)
---- | ----
XSRF Protection 보안 기능 제공 | 없음
---- | ----
자동으로 JSON 데이터로 변환 | JSON 데이터로 바꿔 주어야 함
---- | ----
요청 취소와 타임아웃 설정 가능 | 없음 (AbortController 이용하여 구현 가능)
---- | ----
HTTP Requests 의 Intercept 가능 | Intercept Requests 기본적으로 제공되지 않음 (Global Fetch Method를 Overwrite 하여 나의 인터셉터 정의 가능)
---- | ----
Download 요청에 대해 기본적인 지원 | Upload Progress 지원안함 (Response Object의 Body Property 통해 제공되는 ReadableStream 인스턴스 이용 가능)

<br>

#### 사용법

axios는 설치해서 사용해야하는 라이브러리로 터미널에서 아래 명령어로 설치한다.

```npm
npm install axios
```

```javascript
import axios from 'axios';

axios.create({
  baseURL: 'https://www.googleapis.com/youtube/v3',
  params: { key: '키값' },
})
  .get('추가경로')
    .then((json)=>{
  받아온 데이터 처리;
})
```

useEffect 로 마운트 할때 적용한다.

<br>

#### 기본 파라미터

- `key(필수) : string;` 발급받은 키

- `part : string;`    
-- `videoid` 단일 영상ID
-- `snippet` 관련된 영상들

- `maxResults :  number;` 영상 결과 최대 개수

- `q : string;` 검색어

- `type : string;` 'video'로 설정

- `videoDuration : string;` 길이 ( long : 20분이상 )

<br>

### 기본 레이아웃 서브메뉴 추가

#### [Department 페이지](/layout_practice/#/department/)

DB가 따로 없어서 수기로 json 데이터를 만들어서 뽑아오는 연습을 했다.

```json
{
  "members": [
    {
      "name": "이름",
      "position": "직업",
      "pic": "이미지 파일명.jpg"
    }
  ]
}
```

```javascript
const path = process.env.PUBLIC_URL;
const [Members, setMembers] = useState([]);

useEffect(() => {
  axios.get(`${path}/db/members.json`).then((json)=>{
    setMembers(json.data.members);
  })
}, [])

useEffect(() => { // 데이터 확인용
  console.log(Members);
}, [Members])
```

<br>

JSX에서는 Members를 `map()`으로 돌려서 출력시켰다.

JSX return 내부에서 JS코드 사용 시 중괄호로 둘러주는 것에 유의해야 한다.

아직 중괄호 사용이 어색하다.

<br>

<iframe src='/layout_practice/#/department/' frameborder='0' width='100%' height='500px'></iframe>

<br>

#### [Youtube 페이지](/layout_practice/#/youtube/)

youtube에서 제공하는 API를 활용해서 미리 설정한 플레이리스트를 받아와서, useEffect로 마운트 시 뿌려준다.

```javascript
const url = `https://www.googleapis.com/youtube/v3/playlistItems?part=snippet&key=${key}&playlistId=${playlist}&maxResults=${num}`;

const [videos, setVideos] = useState([]);
const [open, setOpen] = useState(false);
const [index, setIndex] = useState(0);

useEffect(() => {
  axios.get(url).then((json)=>{
    setVideos(json.data.items);
  })
}, [])
```

JSX에서는 역시나 `map()`으로 하나씩 출력 해 줬다.

출력 시 제목과 내용은 너무 길지 않도록 `substring()`으로 일부 잘라냈고, 날짜 데이터 또한 시간은 뜨지 않도록 `split()`으로 배열로 변환하여 활용했다.

클릭 시 Popup 컴포넌트도 불러왔다.

```JSX
<>
  <Layout name='youtube'>
    {videos.map((data, index)=>{
      let title = data.snippet.title;
      let description = data.snippet.description;
      let date = data.snippet.publishedAt;

      if(title.length > 30) title = title.substring(0, 30) + '...';
      if(description.length > 100) description = description.substring(0, 100) + '...';
      date = date.split('T')[0];

      return (
        <article key={index}>
          <div className="txt">
            <h3>{title}</h3>
            <p>{description}</p>
            <span>{date}</span>
          </div>
          <div className="pic" onClick={()=>{ setOpen(true); setIndex(index)}}>
            <img src={data.snippet.thumbnails.standard.url} alt={data.snippet.title} />
          </div>
        </article>
      );
    })}
  </Layout>
  { open && <Popup setOpen={setOpen}>
    <iframe src={`https://www.youtube.com/embed/${videos[index].snippet.resourceId.videoId}`} frameBorder="0"></iframe>
  </Popup> }
</>
```

#### Popup 컴포넌트 마운트

1. 상단에서 `const [open, setOpen] = useState(false);`로 컴포넌트 마운트 조건을 만들어 준다.    
-- 최초값 `false`

2. 클릭 시 `()=>{setOpen(true)}}>`으로  `useState()`를 사용해서 마운트 조건을 `true`로 바꿔준다.

3. `&&`로 마운트 조건이 `true일` 시에만 Popup 컴포넌트를 불러온다.    
-- `&&` : 앞 내용 `true` 시 뒷 내용 실행

4. JSX내 Popup컴포넌트 호출 태그의 인라인요소로 `iframe`을 넣어 Popup컴포넌트에서 `props.children`으로 내용을 받아올 수 있도록 했다.    
오픈 시 `useState()`로 index값을 변경해 해당 값을 사용하도록 했다.    
-- *오픈 이벤트의 코드는 아래와 같다.*

```javascript
onClick={()=>{ setOpen(true); setIndex(index)}}
```

5. 닫기 구현을 위해 클릭 시 이벤트에 `setOpen={setOpen}` 를 prop으로 넘긴다.    

6. Popup 컴포넌트 내 close버튼 클릭 시 이벤트에 `props.setOpen(false)`를 주어 open 값을 `false`로 변경한다.

<br>

#### Body 스크롤 방지

`useEffect()`를 사용해서 Popup 상태 시 Body의 스크롤이 되지 않도록 했다.

useEffect() 내에서 return 시 clean-up을 함수를 실행할 수 있다고 이론으로만 배웠는데, 이렇게 활용하는구나 싶었다.

```javascript
useEffect(() => {
  document.body.style.overflow = 'hidden';
  // Popup 마운트 시 body 스크롤 방지
  return (()=>{
    document.body.style.overflow = 'auto';
    // Popup 언마운트 시 body 스크롤 실행
  })
}, [])
```

<br>

