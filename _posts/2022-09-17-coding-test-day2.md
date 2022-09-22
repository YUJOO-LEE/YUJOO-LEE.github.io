---
layout: post
title:  프로그래머스 연습문제 Day2
date:   2022-09-17 10:38:53 +0900
comments : true
categories: Note
tags: [programmers, algorithm, javascript]
---


문제 푸는 거 너무 재밌어..

<br><br><br>
<hr>
<br><br><br>

### **4.** 평균 구하기

<br>

- **문제 설명**

<br>

정수를 담고 있는 배열 arr의 평균값을 return하는 함수, solution을 완성해보세요.

<br><br>

- **제한사항**

<br>

arr은 길이 1 이상, 100 이하인 배열입니다.

arr의 원소는 -10,000 이상 10,000 이하인 정수입니다.

<br><br>

- **입출력 예**

<br>

arr | return
--- | ------
[1,2,3,4] | 2.5
--------- | ---
[5,5] | 5

<br>

평균 구하는 건 [반응 속도 측정기](/js_games/reaction_time/) 만들면서 써본 적 있어서 금방 했다.

<br><br>

```javascript
function solution(arr) {
    return arr.reduce((a, b) => a + b, 0) / arr.length;
}
```

<br><br><br>
<hr>
<br><br><br>

#### **5.** 정수 제곱근 판별

<br>

- **문제 설명**

<br>

임의의 양의 정수 n에 대해, n이 어떤 양의 정수 x의 제곱인지 아닌지 판단하려 합니다.

n이 양의 정수 x의 제곱이라면 x+1의 제곱을 리턴하고, n이 양의 정수 x의 제곱이 아니라면 -1을 리턴하는 함수를 완성하세요.

<br><br>

- **제한 사항**

<br>

n은 1이상, 50000000000000 이하인 양의 정수입니다.

<br><br>

- **입출력 예**

<br>

n | return
-- | ------
121 | 144
--- | ----
3 | -1

<br>

- **입출력 예 설명**

<br>

--입출력 예#1

121은 양의 정수 11의 제곱이므로, (11+1)를 제곱한 144를 리턴합니다.

<br>

--입출력 예#2

3은 양의 정수의 제곱이 아니므로, -1을 리턴합니다.

<br><br>

처음에 문제 설명을 이해를 못했다.

예시표를 만들어 둔 게 그때문인듯.

국어 어려워...

<br><br>

```javascript
function solution(n) {
    return Number.isInteger(Math.sqrt(n)) ? (Math.sqrt(n) + 1) ** 2 : -1;
}
```

<br><br>

제곱근을 구해서 그 값이 정수라면 1을 더한 값을 내놓으라는 것 같은데,

제곱근 구하는 `Math.sqrt()` 까지는 알겠는데, 정수를 구하는 법을 몰라서 헤맸다.

그리고 찾아낸 [`Number.isInteger()` 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/isInteger). 해당 값이 정수인지 판별해준다.

오늘도 새로운 걸 하나 배웠다.

<br><br><br>
<hr>
<br><br><br>

### **6.** 자연수 뒤집어 배열로 만들기

<br>

- **문제 설명**

<br>

자연수 n을 뒤집어 각 자리 숫자를 원소로 가지는 배열 형태로 리턴해주세요. 예를들어 n이 12345이면 [5,4,3,2,1]을 리턴합니다.

<br><br>

- **제한 조건**

<br>

n은 10,000,000,000이하인 자연수입니다.

<br><br>

- **입출력 예**

<br>

n | return
-- | -------
12345 | [5,4,3,2,1]

<br><br>

**숫자를 문자로 바꾼다 -> 하나씩 자른다 -> 자른 문자를 다시 숫자로 바꿔서 -> 배열의 앞으로 집어넣는다.**

라는 생각으로 나온 아래 코드.

<br><br>

```javascript
function solution(n) {
    let newN = [];
    for (let i = 0 ; i < String(n).length ; i++) {
        newN.unshift(Number(String(n)[i]));
    }
    return newN;
}
```

<br><br>

쓸데없이 복잡해보이니 줄여보기로 한다.

<br>

숫자에 빈 문자열 `''`을 더해서 문자열로 형변환을 하고,

`split('')` 으로 하나씩 나눠서 배열에 담는 것 까지는 하겠는데,

순서를 뒤집을 줄 몰라서 헤맸다.

내가 기억하는 정렬은 `sort()` 밖에 없었으니까..

그래서 덕분에 또 하나 배웠다.  배열의 순서를 반전시키는 [`reverse()` 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)!

<br>

동작 순서도 바꿔보았다.

**숫자를 문자열로 바꾼다 -> 하나씩 잘라 배열에 넣는다 -> 역순으로 재정렬한다 -> 배열값을 숫자로 변환한다.**

<br><br>

```javascript
function solution(n) {
    return (n+'').split('').reverse().map((n) => parseInt(n));
}
```

<br><br><br>
<hr>
<br><br><br>

#### **7.** 문자열 내 p와 y의 개수

<br>

- **문제 설명**

<br>

대문자와 소문자가 섞여있는 문자열 s가 주어집니다. s에 'p'의 개수와 'y'의 개수를 비교해 같으면 True, 다르면 False를 return 하는 solution를 완성하세요. 'p', 'y' 모두 하나도 없는 경우는 항상 True를 리턴합니다. 단, 개수를 비교할 때 대문자와 소문자는 구별하지 않습니다.

예를 들어 s가 "pPoooyY"면 true를 return하고 "Pyy"라면 false를 return합니다.

<br><br>

- **제한사항**

<br>

문자열 s의 길이 : 50 이하의 자연수

문자열 s는 알파벳으로만 이루어져 있습니다.

<br><br>

- **입출력 예**

<br>

s | answer
-- | ------
"pPoooyY" | true
--------- | -----
"Pyy" | false

<br>

- **입출력 예 설명**

<br>

-- 입출력 예 #1

'p'의 개수 2개, 'y'의 개수 2개로 같으므로 true를 return 합니다.

<br>

-- 입출력 예 #2

'p'의 개수 1개, 'y'의 개수 2개로 다르므로 false를 return 합니다.

<br><br>

글자를 하나씩 돌면서 p or P, y or Y 가 있는지 살펴본다.

<br>

```javascript
function solution(s){
    let countP = 0;
    let countY = 0;
    for (let i = 0 ; i < s.length ; i++) {
        if (s[i] === 'p' || s[i] === 'P') countP++;
        if (s[i] === 'y' || s[i] === 'Y') countY++;
    }
    return countP === countY;
}
```

<br>

더 간결한 방법이 있지 않을까?

<br>

찾아보다가 정규식으로 문자열 내에서 검색해서 배열로 반환하는 [`match()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/match)를 배웠다.

배열로 반환되는 일치값이 없으면 null을 반환하는데, 그럼 length에서 에러가 나서 [옵셔널 체이닝 `?.`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Optional_chaining) 을 추가해줬다.

<br>

```javascript
function solution(s){
    return s.match(/P/gi)?.length === s.match(/Y/gi)?.length;
}
```

