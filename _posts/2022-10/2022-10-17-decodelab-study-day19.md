---
layout: post
title:  카카오 맵 API (리액트)
date:   2022-10-17 14:49:04 +0900
comments : true
categories: Note
tags: [decodelab, react, kakaomap, api]
---

학원 수업 Day19

### 카카오맵 API

HTML에서 실습했던 카카오맵 API 예제를 리액트에서도 실습했다.

<br>

#### kakao 불러오기

```html
<script type="text/javascript" src="//dapi.kakao.com/v2/maps/sdk.js?appkey=키"></script>
```

하나밖에 없는 소중한 *public/index.html* 파일의 `<head></head>` 내부에 kakao Api파일을 불러온다.

```javascript
const { kakao } = window;
```

카카오맵을 사용할 js 컴포넌트 `function` 내부 최상단에 kakao 객체를 할당해준다.

window 내에 있는 파라미터 중에서 kakao를 불러온다.

<br>

#### 지도 정보 추가

배열로 지도에서 표시할 각 포인트의 정보를 배열 > 개체의 구조로 저장해둔다.

<br>

#### State 지정

```javascript
const container = useRef(null);
// 지도를 담을 영역의 DOM 레퍼런스

const [ isTraffic, setTraffic ] = useState(false);
// 트래픽 표시 여부

const [ location, setLocation ] = useState(null);
// useEffect에서 생성된 맵 인스턴스

const [ index, setIndex ] = useState(0);
// 마커 및 지도 위치 변경 위한 info배열의 인덱스
```

컴포넌트 내에서 사용할 state를 생성한다.

<br>

#### 마커 정보 생성

```javascript
const imageSrc = info[index].imgSrc;
const imageSize = info[index].imgSize;
const imageOption = info[index].imgPos;
// 마커 이미지 정보 생성

const marker = new kakao.maps.Marker({
  position: info[index].latLng,
  image: new kakao.maps.MarkerImage(imageSrc, imageSize, imageOption)
})
// 마커 생성
```

현재의 index(기본값 0)을 이용해 마커 정보를 생성한다.

<br>

#### 마운트 시 지도 그리기

```javascript
useEffect(() => {
  // 최초 마운트 시 지도 그리기
  const mapInstance = new kakao.maps.Map(container.current, option);
  // 지도 생성 및 객체 리턴
  marker.setMap(mapInstance);
  // 지도상에 마커 표시
  setLocation(mapInstance);
  // location State 업데이트

  window.addEventListener('resize', ()=>{
    // 윈도우 리사이즈 시 중심점도 따라서 이동
    mapInstance.setCenter(info[index].latLng);
  })

  return (()=>{
    // cleanup : 지도 중복 생성 방지
    mapInstance.innerHTML = '';
  });
}, [index])
```

useEffect를 사용해서 컴포넌트 마운트 시 지도를 그린다.

리스트 버튼 클릭 시 해당 위치로 이동되면서 마커를 찍는 효과를 위해 `[deps]`를 `[index]`로 지정해준다.

반응형 대응을 위해 윈도우 리사이즈 시 센터점을 재지정해주었다.

**마지막으로 언마운트 시 cleanup 되도록 생성된 인스턴스의 `innerHTML`을 지워준다.**

필요시 지도 타입 컨트롤이나 줌 컨트롤을 추가한다.

<br>

#### 트래픽 정보 추가

```javascript
useEffect(() => {
  // traffic 버튼으로 표시여부 업데이트 때 마다 지도에 트래픽 그리기
  if ( !location ) return;
  // location 값 비어있을 시 오류 방지

  isTraffic
    ? location.addOverlayMapTypeId(kakao.maps.MapTypeId.TRAFFIC)
    : location.removeOverlayMapTypeId(kakao.maps.MapTypeId.TRAFFIC);
  // 교통 상황 표시 활성화
}, [isTraffic])
```

앞서 지정했던 isTraffic State의 변경값에 따라 트래픽 정보를 지도에 표시한다.

```jsx
<button onClick={()=>{setTraffic(!isTraffic)}}>
  {isTraffic ? 'Traffic Off' : 'Traffic On'}
</button>
```

JSX내에서도 State 자체를 표시하지 않고, 값에 따라 표시되는 문자열을 변경할 수 있다.

<br>

#### info 리스트 출력

```jsx
info.map((data, i) => {
  return (
    <li key={i} 
    onClick={()=>{setIndex(i)}} 
    className={index === i ? 'on' : null}>
      {data.title}
    </li>
  );
})
```

JSX 내에서 info 리스트를 `map`을 돌면서 출력한다.

각각의 li에 고유의 키값을 주고, 클릭 이벤트로 index State의 값을 클릭값으로 업데이트 한다.

클릭된 버튼의 효과 부여를 위해 index State와 현재 li의 인덱스 값을 비교해 클래스를 지정한다.

처음엔 `index === 1 && 'on'` 으로 지정했는데, 콘솔 메세지로 false 상황을 위해 삼항연산자로 undefined를 줄것을 권장해서 삼항연산자로 수정했다.

<br>

