---
layout: post
title:  학원 수업 내용 Day24
date:   2022-10-24 23:19:13 +0900
comments : true
categories: Note
tags: [decodelab, react, flickr, api]
---


### Flickr API

HTML 버전으로 했었던 Flickr API를 리액트 페이지에서도 적용했다.

```javascript
const [ Items, setItems ] = useState([]);
const frame = useRef(null);
const searchInput = useRef(null);
```

먼저 데이터를 담고 받아올 초기 변수 지정을 해준다.

```javascript
const showSearch = (e)=>{
  e.preventDefault();
  const keyword = searchInput.current.value;
  frame.current.classList.remove('on');
  getFlickr({type: 'search', tags: keyword});
  searchInput.current.value = '';
  // 검색 데이터 전송후 input 비우기
};
```

검색 form을 만들어서 submit 시 이벤트를 처리해준다.

키워드를 받고 리스트를 안보이게 한 뒤, 해당 키워드로 새로운 데이터를 받아서 뿌려준다.

여러개의 정보를 객체 형태로 넘길 수 있다.

```javascript
const getFlickr = async (option)=>{
  const key = 'API Key';
  const methodInterest = 'flickr.interestingness.getList';
  const methodSearch = 'flickr.photos.search';
  const num = 50;

  let url = '';
  if (option.type === 'interest') {
    url = `https://www.flickr.com/services/rest/?method=${methodInterest}&per_page=${num}&api_key=${key}&format=json&nojsoncallback=1`;
  } else {
    url = `https://www.flickr.com/services/rest/?method=${methodSearch}&per_page=${num}&api_key=${key}&format=json&nojsoncallback=1&tags=${option.tags}`;
  }
  
  const result = await axios.get(url);
  setItems(result.data.photos.photo);
}
```

API Key와 필요한 파라미터를 저장해놓고, 해당 파라미터로 url을 만들어서 `aixos`를 사용해 데이터를 뽑아온다.

검색 상황이 아닐 시 interest라는 flickr에서 지정한 추천목록으로 불러올 예정이기 때문에 매개변수 객체 내에 type으로 분리 해 주었다.

그리고 필요한 부분에 이벤트를 부여해서 원하는 데이터를 출력하도록 한다.

```jsx
<button
  onClick={()=>{
    frame.current.classList.remove('on');
    getFlickr({type: 'interest'});
  }}
>
  Interest Gallery
</button>
```

interest 리스트를 출력하는 일반적인 버튼과 원하는 데이터를 검색하는 검색창으로 만들었다.

```jsx
<div className="searchBox">
  <form action="#" onSubmit={showSearch}>
    <input type="text"
      ref={searchInput}
      placeholder='검색어를 입력하세요' 
    />
    <button type='submit'>Search</button>
  </form>
</div>
```

<br>

#### Loading

검색되는 동안 로딩 이미지가 출력되도록 했다.

```javascript
const [ Loading, setLoading ] = useState(true);
const [ isClickable, setClickable ] = useState(true);
```

먼저 로딩 상황을 담을 State를 만든다.

로딩중일 시 재클릭 방지를 위한 State도 만들었다.

```javascript
if (!isClickable) return;
setClickable(false);
setLoading(true);
```

데이터를 받아오는 `get()`코드 바로 앞에서 클릭 방지 상황을 체크하고, 클릭 방지 상황이 아니라면 클릭 방지 상황으로 만든 다음 로딩바를 띄우고 데이터를 받아온다.

```javascript
setTimeout(() => {
  setLoading(false);
  frame.current.classList.add('on');
  setClickable(true);
}, 500);
```

데이터를 받아오는 `get()`코드 다음에 로딩바를 지워주고 목록이 출력 되도록 한다.

클릭도 허용 되도록 바꿔준다.

그리고 이 내용들은 모두 `get()`으로 데이터를 받아온 이후에 실행되어야 하므로 `setTimeout()`으로 이전 코드가 종료되면 실행되도록 한다.

너무 빨리 받아오면 로딩바 이미지도 잠깐 나왔다 사라져서 해당 효과를 느끼기 위해 실행 지연 시간을 500ms 넣어주었다.

<br>

#### Masonry

HTML로 제작했던 갤러리 페이지에서는 이미지 배열을 위해 [Isotope](https://www.npmjs.com/package/isotope-layout)를 사용했는데, 리액트 버전에서는 [Masonry](https://www.npmjs.com/package/react-masonry-css)를 사용했다.

```npm
npm i react-masonry-component
```

먼저 Masonry 컴포넌트를 설치한다.

그리고 사용을 원하는 컴포넌트 상위에 Masonry를 불러온다.

```javascript
import Masonry from 'react-masonry-component';
```

JSX에서 Masonry 컴포넌트를 사용한다.

필요에 따라 옵션값도 넣어주도록 한다.

```jsx
<Masonry
  elementType={'div'}
  options={masonryOptions}
>
{Items.length ? Items.map((item, idx)=>{
  return (
    <article key={idx}>
      {/* 출력할 내용 */}
    </article>
  );
})}
</Masonry>
```

elementType은 정렬될 각자의 엘리먼트를 의미한다. article으로 출력할 예정이므로 div로 입력했다.

엘리먼트들이 움직이는 시간도 옵션으로 지정할 수 있다.

```javascript
const masonryOptions = { transitionDuration: '0.5s' };
```

<br>

#### 빈 데이터 처리

데이터를 받아와서 JSX로 출력하는 상황에서, `map()`을 돌기 전에 조건문을 붙여준다.

```JSX
{Items.length ? Items.map((item, idx)=>{
  return (
    // 데이터 출력 부분
  );
})
: <div className='noData'>검색된 데이터가 없습니다.</div>
}
```

데이터가 배열 형태로 되어있으므로 `length`로 데이터 유무를 확인하고 있으면 데이터를 출력, 없으면 데이터가 없다고 띄워준다.

<br>