---
layout: post
title:  모던 자바스크립트 Deep Dive 스터디 인프런 8,9
date:   2022-10-11 23:04:02 +0900
comments : true
categories: Review
tags: [inflearn, javascript]
---

몰랐던, 헷갈렸던, 확실하게는 알지 못했던 것만 정리해두기

<br><br>
<hr>
<br><br>

### 8. 제어문

#### 블록문

0개 이상의 문을 중괄호로 묶은 것.

```javascript
{
  let a = 1;
}
```

문의 끝에는 세미클론을 붙이는 것이 일반적이나, 블록문은 그 자체로 종결성을 가져서 쓰지 않는다.

<br>

#### 조건문

- 폴스루 fall through

switch문은 switch문이 끝날때까지 이후의 모든 case문과 default문을 실행한다.

다음으로 넘어가지 않으려면 case 내에 break를 사용해서 코드 블록에서 탈출해야 한다.

<br>

#### 반복문

```javascript
for (let i = 0; i < 2; 증감식) {
  i++;
}
```

for문 내 어딘가에 증감식이 있다면 증감식 자리는 생략 가능하다. 

<br>

#### 레이블문

식별자가 붙은 문으로 프로그램의 실행 순서를 제어하는 데 사용한다.

```javascript
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i + j === 3) break outer;
  }
}
```

break만 실행하면 내부 for문만 탈출하고 외부 for문은 끝까지 돌지만, 레이블을 지정해서 break 시 사용하면 외부에 있는 for문까지 탈출한다.

```javascript
function a() {
  for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
      if (i + j === 3) return;
    }
  }
}
```

그러나 레이블문은 가독성이 떨어지므로 (객체와 표기법의 유사성 때문에) 사용하지 않는 것이 좋다.

위의 코드와 같이 함수로 처리해서 나가는 편이 가독성이 좋다.

<br>

### 9. 타입 변환

#### 타입 캐스팅 

```javascript
let x = 10;
let str = x.toString();
// str = '10', x = 10
```

x 변수의 값이 변경되는 것은 아니다.

<br>

#### 타입 강제 변환

```javascript
let x = 10;
let str = x + '';
// str = '10', x = 10
```

x 변수의 값이 변경되는 것은 아니다.

`(10).toString()` 보다 `10 + ''` 이 더욱 간결하고 이해하기 쉽다.

```javascript
1 + '2' // '12' 피연산자 모두 문자열 타입이어야 하는 문맥
5 * '10'  // 50 피연산자 모두 숫자 타입이어야 하는 문맥
!0  // true 피연산자 또는 표현식이 불리언 타입이어야 하는 문맥
```

`-`, `*`, `/` 는 모두 숫자로 형변환 한다.

`'1' > 0` 은 불리언 값을 만들기 위해 숫자로 형변환 한다.

`+` 단항 연산자는 피연산자가 숫자 타입이 아니라면 숫자 타입으로 형변환 한다.

```javascript
+'' // 0
+[] // 0
+false  // 0
+null // 0

+undefined  // NaN
+{} // NaN
+[10, 20] // NaN
```

<br>

#### falsy Data

`0`, `''`, `null`, `undefined`, `false`, `NaN`

<br>

#### 명시적 타입 변환

`String(123)` 생성자 함수를 new 없이 호출하면 문자로 형변환 한다.

`Number('123')` 생성자 함수를 new 없이 호출하면 숫자로 형변환 한다.

`Boolean(1)` 생성자 함수를 new 없이 호출하면 불리언으로 형변환 한다.

<br>

#### 단축 평가

`A && B` A가 true라면 B를 실행한다.

`A || B` A가 true라면 B를 는 볼 것도 없이 A실행, A가 false라면 B실행한다.

```javascript
let done = false;
let message = done && '완료' || '미완료';
// message = 미완료
```

삼항 연산자처럼 사용 가능하다.

`||`는 falsy data를 모두 false 처리하지만, ??를 사용하면 좌항이 null, undefined 일때만 우항의 피연산자를 반환한다.

<br>

