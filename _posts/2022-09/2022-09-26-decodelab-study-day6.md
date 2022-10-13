---
layout: post
title:  학원 수업 내용 Day6
date:   2022-09-26 18:19:23 +0900
comments : true
categories: Note
tags: [decodelab, javascript]
---


드디어 오늘부터 JS를 시작했다.

실습보다는 이론 위주라서 정리된 내용이 많다.

<br>

### script 연결 방법

1. head 내 script를 적고 안에 스크립트 구문 작성
- 순서상 동작 안하는 경우 발생

2. body가 끝나는 부분에 작성
- 스크립트의 양이 많기 때문에 적합하지 않음

3. 외부 파일에 연결
- 1번과 같은 단점

4. defer를 스크립트 속성으로 쓰는 외부 파일 연결
- 이 방법으로만 사용할것

<br>

### 변수

특정 데이터 값을 임시로 저장하는 공간

#### - 변수를 쓰는 이유

1. 자주 쓰는 데이터 값을 효율적으로 관리하기 위해
2. 한 번 찾은 데이터를 재활용 하기 위해


#### - 선언 방식

1. var  변수를 설정한 후 변경 가능
2. let  변수를 설정한 후 변경 가능
3. const 변수를 선언 한 후 변경 불가
4. **사용 예**
- let 변수명 = 데이터;
- let 변수명  // 처음 선언 시 필수 사용
- 변수명 = 데이터;  // 변경 시 생략

  
#### - 작성시 유의점

1. 숫자로 시작 불가
2. 특수문자 삽입 불가 (예외 : $, _가능)
3. 예약어 사용 불가 (- 등)
4. 대소문자 구분
5. 한글 피하기


#### - 유효 범위 (scope)

1. 지역변수 (local variable) : 블록 안에서 선언된 변수 (해당 블록 안에서만 사용)
2. 전역 변수 (global variable) : 블록 밖에서 선언된 변수 (어디서든지 읽힘)
- 특정 값을 여러 개의 블록, 서로 다른 함수들이 공유해야 할 때


#### - 호이스팅

자바 스크립트는 실행 전 변수들을 조사하게 되는데, 호이스팅 시 변수의 선언과 초기화를 같이 한다.
- 함수가 실행되기 전에 안에 있는 변수들을 범위의 최상단으로 끌어올리는 것
- 블록 안에서 선언된 지역변수 값이 블록 바깥으로 강제로 끌어져 올라가서 전역화 되는 현상
- 대표적으로 일반 함수 블록이 아닌 조건문, 반복문 안에 있는 지역변수는 호이스팅이 발생
- 직접적으로 function으로 시작하는 함수를 제외하고 모두 호이스팅 일어남
- let은 호이스팅은 일어나지만 선언전에는 사용할 수 없도록 데드존으로 만들어서 관리


### 자료형의 구분

#### 원시형 자료 (primitive type)

- 특정 값이 메모리에 저장 (값만 저장)

1. string (문자열) 문자 값을 가지는 자료
2. number (숫자) 숫자 값을 가지는 자료
3. boolean (참거짓) true, false
4. undefined 변수를 만들고 할당하지 않음

#### 참조형 자료 (reference type)

- 값의 참조 주소값만 메모리에 저장되는 것 (관련 내장 함수까지 같이 참조)

5. null(object) 값이 비어있지만 명시적으로 비워둠
6. array (배열) 여러 개의 값들을 그룹으로 저장 (입력, 탐색 시 순서값으로 동작. 순서가 매우 중요)
7. object (객체) 여러 개의 값들을 그룹으로 저장 (값마다 고유의 key를 부여)

#### 형 변환

let num1 = '2';
let num2 = 3;
let result = num1 + num2; // '23'; 
- 연산되는 숫자 num2를 강제로 문자열로 형변환시켜 두개의 문자를 서로 이어준 것

#### 연산자

1. 산술 : + - * / %
2. 증감 : ++ --
3. 대입 : =  *=  /=  +=  -= 
4. 비교 : >  <  >=  <=  == !=
5. 논리
 - ! : not
 - && : and
 - \|\| : or
 - not => and => or


### 조건문

```javascript
if (조건 1) {
  // 조건 1 만족 시 실행 내용
} else if (조건 2){
  // 조건 1 불만족, 조건 2 만족 시 실행 내용
} else {
  // 모두 불만족시 실행 내용
}
```


### 배열

```javascript
fruits.push('grape'); // 배열의 마지막에 값 추가
fruits.pop(); // 배열의 마지막 값을 제거

fruits.unshift('grape'); // 배열의 가장 앞에 값 추가
fruits.shift(); // 배열의 가장 앞의 값을 제거

fruits.splice(1,1); // (인덱스, 삭제 갯수, 삽입 요소)
fruits.splice(1); // (인덱스부터 뒤의 모든 값 삭제)

const fruits1 = ['strawberry', 'orange'];
const fruits2 = ['apple', 'melon', 'banana'];
const newfruits = fruits1.concat(fruits2);  // 두개의 배열 합쳐서 반환

const fruits5 = ['apple', 'melon', 'banana', 'apple'];
fruits5.indexOf('apple'); // 첫번째 발견된 값 인덱스 0 반환
fruits5.indexOf('orange'); // 없으면 -1 반환
fruits5.lastIndexOf('apple'); // 마지막부터 발견된 값 인덱스 3 반환

fruits5.includes('apple');  // 배열에 포함되어 있으면 true
fruits5.includes('orange');  // 배열에 포함되어 있으면 false

fruits5.length; // 배열의 길이 4
fruits5[fruits5.length-1];  // 마지막값 = 배열의 길이 - 1
```


### 반복문

```javascript
const fruits = ['apple', 'melon', 'banana'];

for (시작 값; 종료 값; 진행 값) {
  // 시작 값으로 코드 진행 후 진행 값을 실행해서
  // 해당 값이 종료값이 되기 전까지 반복
}
for (let 값 of 배열) {
  // 배열을 하나씩 돌며 값을 가져와서 코드 진행 반복
}
fruits.forEach(함수);
fruits.map(함수); 
// 둘다 배열용이지만 forEach는 단순히 배열 길이만큼 실행,
// map은 함수 결과를 모아 새로운 배열으로 반환

return; // 정지 후 반환
break;  // 정지 후 빠져나옴
continue; // 멈추고 다음 반복으로 진행 (특정조건만 빼고 반복할때)
```


### 함수

- 미리 function 키워드로 자주 쓰는 코드들을 묶어주는 행위
- 위치와 상관 없어서 함수끼리 모아두기 가능

- 호출 : 미리 정의되어 있는 함수를 호출해야 실행됨
- 인수 (파라미터 = 매개변수) : 외부에서 특정 값을 함수 내부로 전달해주는 통로 이름
- 함수의 반환값 (return) : 함수 내부의 값을 함수 외부로 반환


### DOM요소 선택

```javascript
const ul = document.getElementsByTagName('ul')[0];
// ul 태그를 모두 불러와 0번 인덱스(첫번째 ul)를 값으로 가져옴

const item = document.getElementsByClassName('item1');
// class 로 해당 값을 가진 요소를 모두 불러옴

const item3 = document.getElementById('item3');
// 해당 id만 1개 가져옴

const _ul = document.querySelector('ul');
const _item = document.querySelectorAll('.item');

for (let el of _item) {
  el.addEventListener('click', () => {
    console.log('clicked!');
  })
}

const _itemli = document.querySelectorAll('ul li');
for (let i = 0; i < _itemli.length; i++) {
  _itemli[i].addEventListener('click', ()=>{
    console.log(`clicked! ${_itemli[i].textContent}`);
  })
}

// 부모요소 선택
console.log(item3.parentElement); // li의 부모인 ul선택
item3.closest('main'); // 가장 가까운 상위 요소 선택

// 형제요소 선택
item3.previousElementSibling; // 이전 요소
item3.nextElementSibling; // 다음 요소
```

### 이벤트

```javascript
const link = document.querySelector('a');
link.addEventListener('click', (e)=>{ // 클릭 시 함수 실행
  e.preventDefault(); // 해당 요소 기본 기능 제거
  console.log('Clicked');
})

const box = document.querySelector('#box');
box.addEventListener('mouseenter', ()=>{ // 마우스 IN시 함수 실행
  box.style.backgroundColor = 'pink';
})
box.addEventListener('mouseleave', ()=>{ // 마우스 OUT시 함수 실행
  box.style.backgroundColor = 'lightblue';
})
// 굳이 js말고 css :hover로 처리할 것

const list = document.querySelectorAll('ul li');
for (let el of list) {  // 여러 개의 요소는 하나하나 돌면서 이벤트 부여
  el.addEventListener('click', (e)=>{
    e.preventDefault();
    e.currentTarget.innerText = 'Hi!';
  })
}

const btnPlus = document.querySelector('.btnPlus');
const btnMinus = document.querySelector('.btnMinus');
let num = 0;  // 전역 변수로 전역으로 값 공유

btnPlus.addEventListener('click', (e)=>{
  e.preventDefault();
  num++;
  console.log(num);
})

btnMinus.addEventListener('click', (e)=>{
  e.preventDefault();
  num--;
  console.log(num);
})
```

<br>
