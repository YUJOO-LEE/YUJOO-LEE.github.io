---
layout: post
title:  타입스크립트 type, interface
date:   2022-12-01 15:32:01 +0900
comments : true
categories: Note
tags: [typescript]
---

### 기본 타입

- 문자열

```typescript
let text: string = 'text';
text = 3; // error
```

- 숫자

```typescript
let num: number = 1;
num = 'text'; // error
```

- 불리언

```typescript
let isBoolean: boolean = true;
isBoolean = 'boolean'; // error
```

- null

```typescript
let empty: null = null;
empty = 'null'; // error
```

- undefined

```typescript
let errors: undefined = undefined;
errors = 'undefined'; // error
```

<br>

### 배열

```typescript
const stringArr: string[] = ['text1', 'text2'];
const numberArr: number[] = [1, 2];

stringArr.push(3);  // error
```

또는 아래와 같이 지정할 수 있다.

```typescript
const stringArr: Array<string> = ['text1', 'text2'];
const numberArr: Array<number> = [1, 2];
```

<br>

### 튜플

배열 내 값을 여러 타입으로 지정할 수 있다.

```typescript
const arr: [string, number] = ['text', 1];

arr[0] = 2; // error
arr[1].toUpperCase(); // error
```

순서를 지켜야 하므로 0번 인덱스의 값에는 string 을 제외한 다른 값이 들어갈 수 없고, 1번 인덱스의 값에는 number 를 제외한 다른 타입의 메서드를 사용할 수 없다.

<br>

### 함수 (return 값)

return 값에 따라 함수의 타입을 지정한다.

<br>

- return 되는 값이 없을 때

```typescript
function noReturn(): void {
  console.log('no return');
}
```

<br>

- return 되는 값이 있을 때

```typescript
function returnNumber(): number {
  return 1;
}
```

<br>

- never

절대 발생될 수 없는 타입을 나타낼 때 사용한다.

에러객체를 반환하거나, 무한루프 함수에서 return 타입을 never 로 지정한다.

```typescript
function returnErr(): never {
  throw new Error();
}
```

<br>

```typescript
function returnErr(): never {
  throw new Error();
}
```

<br>

### interface

내용이 복잡해지는 객체는 interface 로 타입을 지정한다.

```typescript
interface TypeUser = {
  name: string,
  isFemale: boolean,
}

const user: TypeUser = {
  name: 'Yujoo',
  isFemale: true,
}
```

interface 에서 타입을 지정한 key 는 모두 값을 가져야하며, 값이 할당되지 않으면 에러를 반환한다.

만약 해당 값이 없을 수도 있다면 아래와 같이 `?:` 으로 지정해야 한다.


```typescript
interface TypeUser = {
  name: string,
  isFemale?: boolean, // boolean or undefined
}

const user: TypeUser = {
  name: 'Yujoo',
}
```

<br>

### type

타입을 미리 선언해두고 사용할 수 있다.

```typescript
type TypeScore = 'A' | 'B' | 'C' | 'D';

interface TypeUser = {
  name: string,
  score: TypeScore,
}

const user: TypeUser = {
  name: 'Yujoo',
  score: 'E', // error
}
```

위 코드는 user.score 에 type 으로 지정되지 않은 값이 입력되어 에러를 반환한다.

<br>

### 파라미터

일반적인 변수와는 다르게 파라미터는 타입 지정이 필수이다.

```typescript
const add = (num1: number, num2: number): number => {
  return num1 + num2;
}
```

<br>

코드의 가독성을 위해 아래와 같이 interface 로 지정해 사용할 수도 있다.

```typescript
interface TypeCalc {
  (num1: number, num2: number2): number;
}

const add: TypeCalc = (num1, num2) => {
  return num1 + num2;
}
```

<br>
