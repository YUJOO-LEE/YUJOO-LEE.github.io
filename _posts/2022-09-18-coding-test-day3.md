---
layout: post
title:  프로그래머스 연습문제 Day3
date:   2022-09-18 08:08:53 +0900
comments : true
categories: Note
tags: 
---


강의 듣고싶은데 에어팟을 두고 왔다 흑흑

연습문제나 풀어야지.

<br><br><br>
<hr>
<br><br><br>

#### **8.** 정수 내림차순으로 배치하기

<br>

- **문제 설명**

<br>

함수 solution은 정수 n을 매개변수로 입력받습니다. n의 각 자릿수를 큰것부터 작은 순으로 정렬한 새로운 정수를 리턴해주세요. 예를들어 n이 118372면 873211을 리턴하면 됩니다.

<br><br>

- **제한 조건**

<br>

n은 1이상 8000000000 이하인 자연수입니다.

<br><br>

- **입출력 예**

<br>

n | return
-- | ------
118372 | 873211

<br><br>

```javascript
function solution(n) {
    return (n+'').split('').sort((a, b) => b - a).join('') * 1;
}
```

<br>

기존에 했던 내용의 연속이라 바로 할 수 있었다.

<br><br><br>
<hr>
<br><br><br>

#### **9.** 하샤드 수

<br>

- **문제 설명**

<br>

양의 정수 x가 하샤드 수이려면 x의 자릿수의 합으로 x가 나누어져야 합니다. 예를 들어 18의 자릿수 합은 1+8=9이고, 18은 9로 나누어 떨어지므로 18은 하샤드 수입니다. 자연수 x를 입력받아 x가 하샤드 수인지 아닌지 검사하는 함수, solution을 완성해주세요.

<br><br>

- **제한 조건**

<br>

x는 1 이상, 10000 이하인 정수입니다.

<br><br>

- **입출력 예**

<br>

arr | return
--- | ------
10 | true
-- | ----
12 | true
-- | ----
11 | false
-- | -----
13 | false

<br><br>

- **입출력 예 설명**

<br>

-- 입출력 예 #1

10의 모든 자릿수의 합은 1입니다. 10은 1로 나누어 떨어지므로 10은 하샤드 수입니다.

<br>

-- 입출력 예 #2

12의 모든 자릿수의 합은 3입니다. 12는 3으로 나누어 떨어지므로 12는 하샤드 수입니다.

<br>

-- 입출력 예 #3

11의 모든 자릿수의 합은 2입니다. 11은 2로 나누어 떨어지지 않으므로 11는 하샤드 수가 아닙니다.

<br>

-- 입출력 예 #4

13의 모든 자릿수의 합은 4입니다. 13은 4로 나누어 떨어지지 않으므로 13은 하샤드 수가 아닙니다.

<br><br>

하샤드라는 말을 처음 들어봤다.

오늘도 하나 배웠다.

<br>

```javascript
function solution(x) {
    const xSum = (x + '')
        .split('')
        .map(item => Number(item))
        .reduce((acc, item) => acc + item, 0);
    return !(x % xSum);
}
```

<br>

매번 배열로 바꾸는 방법만 쓴 것 같아 문자열로만 바꿔서 for문으로 돌려봤다.

<br>

```javascript
function solution(x) {
    let stringX = String(x);
    let sumX = 0;
    for (let i = 0 ; i < stringX.length ; i++) {
        sumX += Number(stringX[i]);
    }
    return !(x % sumX);
}
```

<br><br><br>
<hr>
<br><br><br>

#### **10.** 나머지가 1이 되는 수 찾기

<br>

- **문제 설명**

<br>

자연수 n이 매개변수로 주어집니다. n을 x로 나눈 나머지가 1이 되도록 하는 가장 작은 자연수 x를 return 하도록 solution 함수를 완성해주세요. 답이 항상 존재함은 증명될 수 있습니다.

<br><br>

- **제한사항**

<br>

3 ≤ n ≤ 1,000,000

<br><br>

- **입출력 예**

<br>

n | result
-- | ------
10 | 3
-- | ---
12 | 11
-- | ---

<br>

- **입출력 예 설명**

<br>

-- 입출력 예 #1

10을 3으로 나눈 나머지가 1이고, 3보다 작은 자연수 중에서 문제의 조건을 만족하는 수가 없으므로, 3을 return 해야 합니다.

<br>

-- 입출력 예 #2

12를 11로 나눈 나머지가 1이고, 11보다 작은 자연수 중에서 문제의 조건을 만족하는 수가 없으므로, 11을 return 해야 합니다.

<br><br>

for문만 돌리고 while문은 잘 안쓰는 것 같아서 이번엔 while을 사용했다.

<br>

```javascript
function solution(n) {
    let i = 0;
    while (i < n) {
        if (n % i === 1) break;
        i++
    }
    return i;
}
```

