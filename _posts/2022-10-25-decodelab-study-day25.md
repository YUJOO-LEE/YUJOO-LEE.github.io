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

### memo()

상위 컴포넌트가 렌더링되면 하위 컴포넌트도 따라서 렌더링 된다.

렌더링을 막고 싶다면 하위 컴포넌트 `export` 시 `memo(컴포넌트명)` 를 사용해서 메모리에 넣어둔다.


```javascript
import { memo } from 'react';
function Child() {
  return(
    // 하위 컴포넌트 내용
  );
}
export default memo(Child);
```

이렇게 메모리에 넣어둔 하위 컴포넌트는 상위 컴포넌트가 다시 렌더링 될 때 상위 컴포넌트로부터 하위 컴포넌트로 보내는 props를 기준으로 렌더링 여부가 정해진다.

props가 기존과 같다면 다시 렌더링 하지 않고 메모리에 저장해 둔 컴포넌트를 그대로 활용하며, props가 기존과 다를 경우에만 렌더링 한다.

```javascript
import Child from './Child';
function App() {
	return (
    // 상위 컴포넌트 내용
    // ...

    <Child Counter={Counter}></Child>
	);
}
export default App;
```

위의 코드를 예시로 들면, props로 넘긴 Counter가 변해야 하위 컴포넌트인 Child가 다시 렌더링 되고, Counter가 아닌 다른 값이 변하는 것에 대해서는 Child는 다시 렌더링 되지 않는다.

컴포넌트가 같은 props로 자주 렌더링되거나 무겁고 비용이 큰 연산이 있는 경우, `memo()` 로 컴포넌트를 래핑할 필요가 있다.

<br>

#### 참조형 데이터타입

앞서 메모이징한 컴포넌트에 props로 값을 넘길때 해당 값이 변하지 않는 이상 하위 컴포넌트는 다시 렌더링 되지 않는다고 했다.

하지만 props로 넘긴 값이 객체, 배열 등 참조형 데이터 타입이라면 그 값이 변하지 않더라도 하위 컴포넌트가 계속해서 렌더링이 된다.

리액트는 컴포넌트 안에 객체가 선언이 되어 있다면 이 객체는 해당 컴포넌트가 렌더링될 때 마다 새로운 메모리에 저장된다.

그리고 참조형 데이터 타입인 객체, 배열, 그리고 함수는 내부의 값이 서로 같더라도 객체끼리 비교하면 다르다는 결과값이 나온다.

```javascript
객체A = { name: 'yujoo'};
객체B = { name: 'yujoo'};

객체A === 객체B // false
```

메모리에 저장된 값을 봤을 때 객체는 각 프로퍼티 값을 저장하는 것이 아니라, 프로퍼티가 있는 주소의 메모리 주소를 모아놓은 공간을 만들고, 그 메모리의 주소를 값으로 갖기 때문이다.

간략하게 예시를 들자면 아래와 같다.


```
메모리1번 = 이름: 객체A / 값: 메모리10번~
메모리2번 = 이름: 객체B / 값: 메모리20번~
...
메모리10번 = 이름: name / 값: 메모리100~
메모리20번 = 이름: name / 값: 메모리100~
...
메모리100번 = 값: yujoo
```

따라서 객체는 const로 선언해도 내부의 값을 변경할 수 있다.

`객체A.name` 을 바꾸는 것은 객체A 자체의 값을 바꾸는 것이 아니라 name의 값을 바꾸는 것이기 때문에 변경이 가능하다.

위의 구조에 따라 객체A와 객체B는 그 값이 다르다는 것 또한 알 수 있다.

`memo()` 사용 시 props로 보낸 값이 참조형 데이터일 경우 비교 시 무조건 `false` 가 나오는 것은 객체 그 자체를 비교하기 때문이다.

내부의 데이터를 비교해서 내부 데이터의 변경 여부에 따라 렌더링을 하고 싶다면 `lodash` 등 라이브러리를 사용해야 한다.

<br>

#### lodash

```npm
npm i lodash
```

lodash를 사용하면 다양한 메서드를 통해 참조형 데이터를 손쉽게 다룰 수 있다.

사용할 수 있는 메서드는 여러가지가 있지만, 오늘 사용한 메서드는 `isEqual`이다.

`isEqual`은 두 객체를 비교하는데, 단순하게 객체 자체를 얕게 비교하는 것이 아니라 객체 내의 모든 값들을 비교해서 boolean 값으로 반환한다.

<br>

### useMemo()

복잡한 계산을 필요한 경우에만 할당하여 메모리에 담아두고 재활용 할 수 있다.

원하는 경우가 아니면 다시 계산하지 않기 때문에 속도면에서 상당히 유리하다.

다만, 메모리 공간을 차지하므로 정말 속도가 필요한 경우가 아니라면 비효율적일 수 있다.

```javascript
const saveHeavyNum = ()=>{
  let heavyNum = 0;
  for(let i = 0; i < 6000000000; i++){
    heavyNum++;
  }
  return heavyNum;
};
```

컴포넌트에서 위 함수를 실행하면 실행할 때 마다 부하가 걸린다.

```javascript
const saveHeavyNum = useMemo(()=>{
  let heavyNum = 0;
  for(let i = 0; i < 6000000000; i++){
    heavyNum++;
  }
  return heavyNum;
}, []);
```

하지만 이처럼 `useMemo()` 로 감싸둔다면 배열에 저장된 값이 변할때만 (빈 배열이면 마운트시에만) 함수를 다시 실행한다.

이 외의 상황에서는 한번 실행한 값을 저장해놓고 쓰게 된다.

<br>

