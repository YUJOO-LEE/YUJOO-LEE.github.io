---
layout: post
title:  타입스크립트 overload, this, 접근제한자, generic
date:   2022-12-02 00:05:43 +0900
comments : true
categories: Note
tags: [typescript]
---

### Overload

동일한 매개변수이지만 갯수나 타입에 따라 다른 타입의 return 값이 반환될 때 overload 처리한다. 

```typescript
interface User {
  name: string;
  age: number;
}

function memberDB(name: string, age: number | string): User | string  {
  if (typeof age === 'number') return {name, age};
  else return 'age 는 숫자 형식이야 합니다.'
}

const David: User = memberDB('David', 20);
const Emily: string = memberDB('Emily', '30');
```

위의 코드 작성 시 함수를 호출하는 변수에 타입을 지정하면 함수에서 출력되는 타입과 다를 수도 있기 때문에 에러가 표시된다.

아래와 같이 함수 선언 전에 인자값과 출력값에 따른 타입을 별도로 지정해주면 에러를 표시하지 않는다.

```typescript
interface User {
  name: string;
  age: number;
}

function memberDB(name: string, age: number): User;
function memberDB(name: string, age: string): string;
function memberDB(name: string, age: number | string): User | string {
  if (typeof age === 'number') return {name, age};
  else return 'age 는 숫자 형식이야 합니다.'
}

const David: User = memberDB('David', 20);
const Emily: string = memberDB('Emily', '30');
```

<br>

### this

this 를 파라미터같이 작성하고 타입을 할당하면 this 값이 바인딩 될 때의 타입을 지정할 수 있다.

```typescript
interface User {
  name: string;
}

const member = { name: 'Yujoo'};

function test(this: User) { // this 의 타입 지정
  console.log(`I am ${this.name}!`);
}

test.bind(member)();
```

<br>

### Union Type

파라미터로 전달받는 값의 타입이 여러 종류라면 `|` 연산자로 복수개의 타입이 오도록 지정한다.

```typescript
interface iphone {
  name: 'iphone';
  color: string;
  apple(): void;
}

interface galaxy {
  name: 'galaxy';
  color: string;
  samsung(): void;
}

function buyCellphone(product: iphone | galaxy) {
  console.log(product.color);
  product.name === 'iphone' && product.apple();
  product.name === 'galaxy' && product.samsung();
}
```

<br>

### Intersection Type

여러 타입을 모두 만족하는 타입을 지정하고 싶으면 `&` 연산자로 복수개의 타입이 오도록 지정한다.

```typescript
interface Action {
  name: string;
  play(): void;
}

interface Rpg {
  name: string;
  price: number;
}

const actionRpg: Action & Rpg = {
  name: 'Diablo',
  play() {
    console.log('Play');
  }
}
```

위 코드의 객체는 지정한 두개의 타입을 모두 만족해야 하는데 price 값이 없으므로 에러를 반환한다.

<br>

### class 접근 제한자

접근 제한자로 class 에서 만든 프로퍼티나 메서드의 접근 범위를 지정할 수 있다.

<br>

#### public

`public` 은 해당 프로퍼티 또는 메서드를 모든 범위에서 호출할 수 있도록 지정한다.

```typescript
class Phone {
  name: string = 'phone';
  color: string;

  constructor(color: string) {
    this.color: color;
  }

  info() {
    console.log('phone');
  }
}
```

위와 같은 상위 class 가 있을 때, 아래와 같이 하위 class 에서 상속받는다.

이때 접근 제한자를 지정하지 않으면 기본값은 `public` 으로 모든 범위에서 호출할 수 있게 된다.

```typescript
class Iphone extends Phone {
  constructor(color: string) {
    super(color); // 상위 class의 color 값 상속
  }
  showInfo() {
    console.log(super.name);
  }
}
```

따라서 위의 코드는 에러를 반환하지 않는다.

<br>

#### private

하지만 아래와 같이 `private` 로 접근을 제한하면 선언된 class 가 아닌 외부 class 에서 호출했을 때 에러를 표시한다.

```typescript
class Phone {
  private name: string = 'phone';
  color: string;

  constructor(color: string) {
    this.color: color;
  }

  info() {
    console.log('phone');
  }
}

class Iphone extends Phone {
  constructor(color: string) {
    super(color);
  }
  showInfo() {
    console.log(super.name);  // Error
  }
}
```

`private` 는 `#` 으로도 대체 가능하다.

```typescript
class Phone {
  #name: string = 'phone';  // private 와 동일
  ...
}
```

<br>

#### protected

선언된 class 와 상속받는 하위 class 에서는 호출할 수 있지만, 외부에서는 호출할 수 없다.

```typescript
class Phone {
  protected name: string = 'phone';  // private 와 동일
  ...
}

class Iphone extends Phone {
  ...
  showInfo() {
    console.log(super.name);  // 호출 가능
  }
}

const a = new Iphone('blue');
console.log(a.name);  // Error
```

<br>

#### readonly

값을 읽을 수는 있지만 변경은 불가하도록 지정한다.

```typescript
class Phone {
  readonly name: string = 'phone';
  ...
}

class Iphone extends Phone {
  ...
  showInfo() {
    console.log(super.name);
  }
}

const a = new Iphone('blue');
console.log(a.name);  // 호출 가능
a.name = 'TV';  // Error
```

<br>

#### static

static 으로 지정하면 생성한 모든 인스턴스에 공통으로 값이 유지된다.

static 으로 지정한 변수는 클래스가 메모리에 올라갈 때 자동으로 생성되므로 인스턴스 생성없이 호출할 수 있다.

또한 인스턴트에 생성된 것이 아닌 class 자체에 생성되었으므로 this 로 호출할 수 없다.

static 과 public / protected / private 은 함께 지정할 수 있다.

```typescript
class Phone {
  readonly name: string = 'phone';
  static cameraNum = 2;
  ...
}

class Iphone extends Phone {
  ...
  showInfo() {
    console.log(super.name);
    console.log(Phone.cameraNum); // this.cameraNum 은 error
  }
}
```

<br>

### Generic

파라미터로 전달되는 타입의 경우의 수가 많아지면 Generic 으로 지정할 수 있다.

함수 선언 시 `<>` 으로 전달받아 적용할 타입을 변수로 지정하고 호출 시 상황에 맞게 타입을 적용시킨다.

```typescript
function func(arr: number[] | string[] | boolean[]): number {
  return arr.length;
}

test([1, 2, 3]);
test(['a', 'b', 'c']);
test([true, false]);
```

위의 코드는 아래와 같이 수정할 수 있다.

```typescript
function func<custom>(arr: custom[]): number {
  return arr.length;
}

test<number>([1, 2, 3]);
test<string>(['a', 'b', 'c']);
test<string | number | boolean>(['a', 2, true]);
```

Generic 은 배열 보다는 객체값을 전달할 때 유용하게 쓰인다.

```typescript
interface Vehicle<t> {
  name: string;
  price: number;
  option: t;
}

const sonata: Vehicle<string> = {
  name: 'sonata',
  price: 4000,
  option: 'good',
}

const genesis: Vehicle<{hud: boolean, cameraNum: number}> = {
  name: 'genesis',
  price: 9000,
  option: {
    hud: true,
    cameraNum: 5,
  },
}
```

<br>

