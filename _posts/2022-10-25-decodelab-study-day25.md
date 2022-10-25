---
layout: post
title:  학원 수업 내용 Day25
date:   2022-10-25 10:21:02 +0900
comments : true
categories: Note
tags: [decodelab, react, flickr, api]
---


### Flickr API

Flickr API 활용 두번째 시간.

#### buddy icon 표시

```jsx
<div className="profile">
  <img src={`http://farm${item.farm}.staticflickr.com/${item.server}/buddyicons/${item.owner}.jpg`} alt={item.owner}/>
  <span>{item.owner}</span>
</div>
```

JSX내 `데이터.map()`으로 개별 데이터를 뿌려주는 부분에서 이미지 게시자의 프로필 아이콘과 ID를 받아올 수 있다.

```jsx
<img 
  src={`http://farm${item.farm}.staticflickr.com/${item.server}/buddyicons/${item.owner}.jpg`} 
  alt={item.owner} 
  onError={(e)=>{e.target.setAttribute('src', 'https://www.flickr.com/images/buddyicon.gif');
}} />
```

onError로 이미지가 없는 상황에서는 flickr 에서 제공하는 기본 이미지를 불러오도록 한다.

```jsx
<span onClick={()=>{
  frame.current.classList.remove('on');
  getFlickr({type: 'user', userid: item.owner});
}}>{item.owner}</span>
```

아이디를 클릭하면 해당 작성자가 업로드한 이미지만 출력하도록 클릭 이벤트를 추가한다.

```javascript
const methodUser = 'flickr.people.getPhotos';
```

```javascript
// ...
else if (option.type === 'user') {
  url = `https://www.flickr.com/services/rest/?method=${methodUser}&per_page=${num}&api_key=${key}&format=json&nojsoncallback=1&user_id=${option.userid}`;
}
```

데이터를 출력 코드에서 user별 이미지를 추출하는 코드를 추가한다.

<br>

#### useImperativeHandle()

Youtube 리스트 페이지 제작시 만들었던 Popup 컴포넌트를 Filckr 에서도 재활용했다.

그 과정에서 Popup 컴포넌트의 코드를 약간 수정했는데, 그때 사용 한 Hook이 `forwardRef()`, `useImperativeHandle()` 이다.

하위 컴포넌트 내부에서 만들어진 함수를 상위 컴포넌트에서 사용할 수 있게 전달해주는 Hook이다.

<br>

**사용방법**

1. 기존의 컴포넌트 함수가 선언형 함수라면 대입형 함수로 바꾸고 `forwardRef()` 안쪽에 함수를 넣어준다.

2. `forwardRef()` 내부 함수의 두번째 인수로 ref 추가한다.

3. `forwardRef(함수)` 내부에 `useImperativeHandle()` 를 추가한다.

4. 해당 함수로 객체를 반환해서 상위 컴포넌트로 전달한다.

6. 상위 컴포넌트의 `useRef()` 로 하위 컴포넌트의 `useImperativeHandle()` 에서 `return` 되는 객체 참조

<br>

```javascript

const Popup = forwardRef((props, ref)=>{
  const [open, setOpen] = useState(false);

  useImperativeHandle(
    ref,
    () => {
      return {
        setOpen: ()=> setOpen(true),
      };
    }
  )
});
export default Popup;
```

위 코드는 `setOpen()` 를 상위 컴포넌트로 보내서 상위 컴포넌트에서 해당 State(=open)를 제어할 수 있게 한다.

<br>

```javascript
const pop = useRef(null);
```

상위 컴포넌트에서는 ref로 하위 컴포넌트를 지정해준다.

```jsx
<Popup ref={pop}>
  {Items.length > 0 &&  // 데이터가 있을 때만 출력
    // Popup창에 출력 될 내용
  }
</Popup>
```

하위 컴포넌트(=Popup)에서 받아온 메서드를 사용하고 싶다면 원하는 버튼에 아래와 같이 이벤트를 추가해 준다.

```javascript
onClick={()=>{
  pop.current.setOpen();  
  // ref로 지정한 pop 내의 setOpen() 메서드 실행
}}>
```

<br>

