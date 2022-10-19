---
layout: post
title:  학원 수업 내용 Day21
date:   2022-10-19 15:18:11 +0900
comments : true
categories: Note
tags: [decodelab, react, localstorage]
---


### Community 페이지

간단한 글을 올릴 수 있는 게시판을 만들고 Local Storage에 저장했다.


#### 게시물 등록

```jsx
<div className="inputBox">
  <input type="text" 
    placeholder="제목을 입력하세요" 
    ref={input} />
  <textarea 
    placeholder="본문을 입력하세요"
    ref={textarea}
    rows='5'
  ></textarea>
  <div className="btnSet">
    <button onClick={resetForm}>CANCLE</button>
    <button onClick={createPost}>WRITE</button>
  </div>
</div>
```

데이터 저장 시 값을 불러오기 위해 각 인풋에 ref를 지정한다.

클릭 버튼에도 이벤트를 지정한다.

```javascript
const [ posts, setPosts ] = useState([]);

const createPost = () => {
  const title = input.current.value.trim();
  const content = textarea.current.value.trim();
  if (!title || !content) return;

  setPosts([
    {"title": title, "content": content},
    ...posts
  ])
  resetForm();
};
```

각 인풋의 값을 받아와서 비어있다면 저장하지 않고 return으로 빠져나간다.

값이 있다면 posts 배열내에 객체 형태로 추가 저장한다.

[값, ...배열] 은 배열을 복사해서 가장 앞에 값을 추가한다는 뜻이다.

[...배열, 값] 의 순서라면 복사한 배열의 가장 뒤에 값을 추가하게 된다.

배열 복사를 하지 않고 값만 저장하면 기존 데이터는 모두 지워지게 된다.

<br>

#### 리스트 출력

```jsx
<div className="listBox">
  {posts.length && posts.map((data, i)=>{
    return (
      <article key={i}>
        <div className="txt">
          <h3>{data.title}</h3>
          <p>{data.content}</p>
        </div>
        <div className='btnSet'>
          <button onClick={()=>{enableUpdate(i)}}>EDIT</button>
          <button onClick={()=>{deletePost(i)}}>DELETE</button>
        </div>
      </article>
    );
  })}
</div>
```

저장된 데이터가 있으면 map을 돌면서 배열의 요소를 하나씩 뽑아와 DOM 요소로 그려준다.

수정, 삭제 기능을 위해 버튼도 만들어서 인덱스값을 인수로 넘겨준다.

<br>

#### 삭제 기능

```javascript
const deletePost = (index) => {
  // const newData = posts.slice().splice(index, 1);
  // setPosts(newData);
  setPosts(posts.filter((_, i)=> i !== (index)));
}
```

처음에는 주석처리 한 부분처럼 작성했는데 굳이 변수를 생성하지 않고서 filter로 처리할 수 있었다.

> #### `filter(함수)`
> 
> 요소를 돌면서 함수의 조건을 통과하는 값만 return시켜 새로운 배열으로 반환한다.
> 
> return 값이 true 라면 신규 배열으로 값을 가져오고, return 값이 false 라면 값을 가져오지 않는다.

위 메서드를 활용해서 매개변수로 받아온 index 값과 일치하는 요소만 제외하고 나머지를 새로운 배열로 반환해서 데이터로 저장한다.

<br>

#### 수정 기능

```javascript
const enableUpdate = (index) => {
  setPosts(posts.map((data, i)=>{
      if (i === index) {
        data.enableUpdate = true;
      } else {
        data.enableUpdate = false;
      };
      return data;
    })
  );
}
```

먼저 선택한 인덱스에 해당하는 데이터에 enableUpdate로 명명한 수정 상태 여부를 boolean 데이터 타입 true 값으로 넣어준다.

선택한 인덱스가 아니라면 모두 false 값을 주어서 수정 상태에서 벗어나도록 한다.

<br>

```jsx
{data.enableUpdate
? {/* update 상태가 true라면 수정 인풋 표시 */}
<> 
  <div className="editTxt">
    <input
    type="text"
    defaultValue={data.title} 
    ref={editInput} 
    />
    <textarea 
    rows="5" 
    defaultValue={data.content}
    ref={editTextarea} 
    ></textarea>
  </div>
  <div className='editBtnSet'>
    <button onClick={()=>{disableUpdate(i)}}>CANCLE</button>
    <button onClick={()=>{editPost(i)}}>UPDATE</button>
  </div>
  </>
: {/* update 상태가 false라면 저장된 데이터 표시 */}
  <>
    생략
  </>
}
```

수정된 값을 저장하기 위해 수정 Input 에도 ref를 붙여주고, 용이한 수정을 위해 선택한 인덱스에 해당하는 값을 미리 기본 입력값으로 불러온다.

버튼에도 이벤트를 걸고 선택한 인덱스를 인수로 넘긴다.

<br>

#### 수정 취소

작성중인 수정 내용을 저장하지 않고 취소한다.

```javascript
const disableUpdate = (index) => {
  setPosts(posts.map((data, i)=>{
      if (i === index) data.enableUpdate = false;
      return data;
    })
  );
}
```

선택한 인덱스의 enalbeUpdate 값만 false 로 바꾸면 화면 내에서 인풋 요소들이 사라지도 기존 데이터로 다시 렌더링된다.

<br>

#### 수정 입력

```javascript
const editPost = (index) => {
  const title = editInput.current.value.trim();
  const content = editTextarea.current.value.trim();
  if (!title || !content) return;

  setPosts(posts.map((data, i)=>{
    if (i === index) {
      data.title = title;
      data.content = content;
      data.enableUpdate = false;
    }
    return data;
  }));
}
```

수정 input에 들어있는 요소들을 선택한 인덱스에 맞는 데이터로 저장해준다.

전체적으로 map을 사용했는데, 이 부분은 아래처럼 배열에서 해당 인덱스만 불러와서 수정할 수도 있지 않을까?

```javascript
const newData = posts.slice();
newData[index] = {
  "title": title,
  "content": content,
  "enableUpdate": false
};
setPosts(newData);
```

map은 배열을 다 돌아봐야 해서 이쪽이 조금 더 효율적이지 않을까 한다.

<br>

#### local storage 사용

로컬 스토리지는 만료기간이 없어서 임의로 삭제하지 않는 이상 데이터가 브라우저에 영구적으로 남아있다.

저장되는 값은 모두 string이기 때문에 다른 데이터타입으로 사용하고자 한다면 형변환을 해주어야 한다.

객체의 경우 JSON 메서드로 변환 후 사용 가능하다.

<br>

##### 문자열-객체 형변환

`JSON.stringfy()` 문자열로 변환

`JSON.parse()` 객체로 변환

<br>

##### local storage 사용법

- 저장 : `localStorage.setItem('key', 'value')`

- 읽기 : `localStorage.getItem('key')`

- 삭제 : `localStorage.removeItem('key')`

- 전체 삭제 : `localStorage.clear()`

<br>

```javascript
const getLocalData = () => {
  const dummyPosts = [
    { title: 'TITLE01', content: "HERE COMES DESCRIPTION IN DETAILS."},
    { title: 'TITLE02', content: "HERE COMES DESCRIPTION IN DETAILS."},
    { title: 'TITLE03', content: "HERE COMES DESCRIPTION IN DETAILS."},
    { title: 'TITLE04', content: "HERE COMES DESCRIPTION IN DETAILS."},
    { title: 'TITLE05', content: "HERE COMES DESCRIPTION IN DETAILS."}
  ]

  // 로컬 스토리지 데이터 불러오기
  let data = localStorage.getItem('post');
  // 로컬 데이터 없으면 더미 데이터 넣기
  data = data ? JSON.parse(data) : dummyPosts;
  return data;
}
```

서버를 다루지 않으므로 로컬 스토리지에 데이터가 없다면 더미 데이터를 넣어준 후 출력 되도록 했다.

```javascript
useEffect(()=>{
  localStorage.setItem('post', JSON.stringify(posts));
}, [posts])
```

그리고 데이터를 이용하기 위해 js에서 만든 posts 배열 내 값이 바뀌면 해당 데이터를 local storage에 저장하도록 한다.

<br>

오늘도 너무 재밌었다!

<br>
