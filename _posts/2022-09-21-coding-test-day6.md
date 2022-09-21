---
layout: post
title:  프로그래머스 연습문제 Day6
date:   2022-09-21 22:40:53 +0900
comments : true
categories: Note
tags: 
---


오늘은 4개! 재밌어서 더 풀고 싶지만 인프런 봐야해서 4개만..

<br><br><br>
<hr>
<br><br><br>

#### **19.** 수박수박수박수박수박수?

<br>

**문제 설명**

<br>

길이가 n이고, "수박수박수박수...."와 같은 패턴을 유지하는 문자열을 리턴하는 함수, solution을 완성하세요. 예를들어 n이 4이면 "수박수박"을 리턴하고 3이라면 "수박수"를 리턴하면 됩니다.

<br><br>

**제한 조건**

<br>

n은 길이 10,000이하인 자연수입니다.

<br><br>

**입출력 예**

<br>

n | return
- | ------
3 | "수박수"
- | --------
4 | "수박수박"

<br><br>

```javascript
function solution(n) {
    return '수박'.repeat(n / 2) + (n % 2 ? '수' : '');
}
```

<br><br>

홀수만 개별 처리~ 

<br><br><br>
<hr>
<br><br><br>

#### **20.** 가운데 글자 가져오기

<br>

**문제 설명**

<br>

단어 s의 가운데 글자를 반환하는 함수, solution을 만들어 보세요. 단어의 길이가 짝수라면 가운데 두글자를 반환하면 됩니다.

<br><br>

**제한사항**

<br>

s는 길이가 1 이상, 100이하인 스트링입니다.

<br><br>

**입출력 예**

<br>

s | return
- | ------
"abcde" | "c"
------- | ---
"qwer" | "we"

<br><br>

```javascript
function solution(s) {
    let index = s.length / 2;
    return !(s.length % 2) ? s[index - 1] + s[index] : s[index - 0.5];
}
```

<br><br>

다른 방법을 찾아보려고 MDN을 뒤져보다 `substring()`을 배웠다.

시작 인덱스와 종료 인덱스를 인자로 넣으면 시작 부분부터 종료 전 문자까지 반환해주는 메서드라고 한다.

활용해보기.

<br><br>

```javascript
function solution(s) {
    let index = Math.round(s.length / 2);
    return s.substring(index - 1, s.length % 2 ? index : index + 1);
}
```

<br><br>

`substr()`을 사용하면 더 간결해질 것 같지만 MDN 문서에 뭔가 아래와 같이 무시무시하게 새빨간 경고창이 있어서 사용하지 않기로 한다..

<br>

> **경고:** 엄밀히 말해서 String.prototype.substr() 메서드가 더 이상 사용되지 않는, 즉 "웹 표준에서 제거된" 건 아닙니다. 그러나 substr()이 포함된 ECMA-262 표준의 [부록 B](https://262.ecma-international.org/9.0/#sec-additional-ecmascript-features-for-web-browsers)는 다음과 같이 명시하고 있습니다.

> <br>

>> … 본 부록이 포함한 모든 언어 기능과 행동은 하나 이상의 바람직하지 않은 특징을 갖고 있으며 사용처가 없어질 경우 명세에서 제거될 것입니다. …

>> … 프로그래머는 새로운 ECMAScript 코드를 작성할 때 본 부록이 포함한 기능을 사용하거나 존재함을 가정해선 안됩니다. …

<br><br><br>
<hr>
<br><br><br>

#### **21.** 없는 숫자 더하기

<br>

**문제 설명**

<br>

0부터 9까지의 숫자 중 일부가 들어있는 정수 배열 numbers가 매개변수로 주어집니다. numbers에서 찾을 수 없는 0부터 9까지의 숫자를 모두 찾아 더한 수를 return 하도록 solution 함수를 완성해주세요.

<br><br>

**제한사항**

<br>

1 ≤ numbers의 길이 ≤ 9

0 ≤ numbers의 모든 원소 ≤ 9

numbers의 모든 원소는 서로 다릅니다.

<br><br>

**입출력 예**

<br>

numbers | result
------- | ------
[1,2,3,4,6,7,8,0] | 14
----------------- | ---
[5,8,4,0,6,7,9] | 6

<br>

**입출력 예 설명**

<br>

-- 입출력 예 #1

5, 9가 numbers에 없으므로, 5 + 9 = 14를 return 해야 합니다.

<br>

-- 입출력 예 #2

1, 2, 3이 numbers에 없으므로, 1 + 2 + 3 = 6을 return 해야 합니다.

<br><br>

```javascript
function solution(numbers) {
    return 45 - numbers.reduce((a, b) => a + b, 0);
}
```

<br><br>

0~9 까지 모두 더한 수에서 주어진 numbers를 빼면 되겠지?

아, 시작 값을 45로 주는 게 더 낫겠구나

<br><br>

```javascript
function solution(numbers) {
    return numbers.reduce((a, b) => a - b, 45);
}
```

<br><br><br>
<hr>
<br><br><br>

#### **22.** 내적

<br>

**문제 설명**

<br>

길이가 같은 두 1차원 정수 배열 a, b가 매개변수로 주어집니다. a와 b의 내적을 return 하도록 solution 함수를 완성해주세요.

이때, a와 b의 내적은 a[0]*b[0] + a[1]*b[1] + ... + a[n-1]*b[n-1] 입니다. (n은 a, b의 길이)

<br><br>

**제한사항**

<br>

a, b의 길이는 1 이상 1,000 이하입니다.

a, b의 모든 수는 -1,000 이상 1,000 이하입니다.

<br><br>

**입출력 예**

<br>

a | b | result
-- | -- | -----
[1,2,3,4] | [-3,-1,0,2] | 3
-------- | ------- | ---
[-1,0,1] | [1,0,-1] | -2

<br>

**입출력 예 설명**

<br>

-- 입출력 예 #1

a와 b의 내적은 1*(-3) + 2*(-1) + 3*0 + 4*2 = 3 입니다.

<br>

-- 입출력 예 #2

a와 b의 내적은 (-1)*1 + 0*0 + 1*(-1) = -2 입니다.

<br><br>

```javascript
function solution(a, b) {
    for (let i = 0 ; i < a.length ; i++) {
        a[i] *= b[i];
    }
    return a.reduce((a, b) => a + b);
}
```

<br><br>

내적 설명으로 위키디피아를 링크 해 두었는데,

들어가서 봐도 무슨 소린지 모르겠다.

그냥 문제에서 식 써줘서 그대로 품. 핳하

<br><br>
