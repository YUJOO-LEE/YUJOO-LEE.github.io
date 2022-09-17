---
layout: post
title:  프로그래머스 연습문제 Day1
date:   2022-09-16 23:19:53 +0900
comments : true
categories: Note
tags: 
---

책만 읽기 심심해서 프로그래머스 사이트에 있는 연습문제들을 풀면서 공부하기로 했다.

<br>

<br>

<br>

<br>

<hr>

<br>

<br>

<br>

<br>

#### **1.** 짝수와 홀수

<br>

- **문제 설명**

<br>

정수 num이 짝수일 경우 "Even"을 반환하고 홀수인 경우 "Odd"를 반환하는 함수, solution을 완성해주세요.

<br>

<br>

- **제한 조건**

<br>

num은 int 범위의 정수입니다.

0은 짝수입니다.

<br>

<br>

- **입출력 예**

<br>

num	| return
--- | -------
3 | "Odd"
- | ------
4 | "Even"

<br>

<br>

```javascript
function solution(num) {
    let answer = '';
    num = Number(num);
    absNum = Math.abs(num);
    if (!isNaN(absNum) && absNum % 2 === 0) { // 짝수일 경우
        answer = 'Even';
    } else if (!isNaN(absNum) && absNum % 2 === 1) { //홀수일 경우
        answer = 'Odd';
    } else {
        answer = 'Error!'
    }
    return answer;
}
```

<br>

처음엔 위와 같이 작성했는데, 제한 조건이 있는데 예외를 왜 넣었나 싶다.

그래서 아래와 같이 수정함.

<br>

```javascript
function solution(num) {
    return Math.abs(num) % 2 ? 'Odd' : 'Even';
}
```

<br>

Math.abs를 쓴건 음수때문에.

<br>

<br>

<br>

<br>

<hr>

<br>

<br>

<br>

<br>

#### **2.** 자릿수 더하기

<br>

- **문제 설명**

<br>

자연수 N이 주어지면, N의 각 자릿수의 합을 구해서 return 하는 solution 함수를 만들어 주세요.

예를들어 N = 123이면 1 + 2 + 3 = 6을 return 하면 됩니다.

<br>

<br>

- **제한사항**

<br>

N의 범위 : 100,000,000 이하의 자연수

<br>

<br>

- **입출력 예**

<br>

N | answer
-- | ------
123 | 6
--- | ---
987 | 24


- **입출력 예 설명**

<br>

-- 입출력 예 #1

문제의 예시와 같습니다.

<br>

-- 입출력 예 #2

9 + 8 + 7 = 24이므로 24를 return 하면 됩니다.

<br>

<br>

```javascript
function solution(n)
{
    let answer = 0;
    for (const element of String(n)) {
        answer += Number(element);
    }
    return answer;
}
```

<br>

처음에는 위와 같이 작성했다.

당장 생각나는 게 for문밖에 없어서..

그리고 다시 아래와 같이 줄여보았다.

<br>

```javascript
function solution(n)
{
    return (n + '').split('').reduce((acc, item) => acc + Number(item), 0);
}
```

<br>

숫자를 문자로, 문자를 배열로 만들고 배열의 각 값을 더해줬다.

나중에 다른 답을 보니 spread 구문 [...(n + '')] 으로 배열로 만들고

Number나 ParseInt 대신 item에 * 1 해서 형변환을 하기도 하더라.

새로운 방법을 배웠다.

<br>

<br>

<br>

<br>

<hr>

<br>

<br>

<br>

<br>

#### **3.** 약수의 합

<br>

- **문제 설명**

<br>

정수 n을 입력받아 n의 약수를 모두 더한 값을 리턴하는 함수, solution을 완성해주세요.

<br>

<br>

- **제한 사항**

<br>

n은 0 이상 3000이하인 정수입니다.

<br>

<br>

- **입출력 예**

<br>

n | return
-- | ------
12 | 28
-- | ----
5 | 6


- **입출력 예 설명**

<br>

-- 입출력 예 #1

12의 약수는 1, 2, 3, 4, 6, 12입니다. 이를 모두 더하면 28입니다.

<br>

-- 입출력 예 #2

5의 약수는 1, 5입니다. 이를 모두 더하면 6입니다.

<br>

<br>

약수...? 약수우...?

얼마만에 들어보는 단어인지.

약수가 뭔지 찾아보기까지 했다.

<br>

> 약수 : 어떤 자연수를 나누어떨어지게 하는 수. 어떤 수의 약수에는 1과 자기 자신이 항상 포함된다.

<br>

음... 숫자를 돌면서 나눈 나머지가 0이 되면 될 것 같다.

<br>

```javascript
function solution(n) {
    let answer = 0;
    for (let i = 1 ; i <= n ; i++) {
        if (n % i === 0) {
            answer += i;
        }
    }
    return answer;
}
```

<br>

아래는 한줄이라도 줄여보려고 노력한 흔적...

<br>

```javascript
function solution(n) {
    let answer = 0;
    for (let i = 1 ; i <= n ; i++) {
        if (!(n % i)) answer += i;
    }
    return answer;
}
```

<br>

