---
layout: post
title:  프로그래머스 연습문제 Day5
date:   2022-09-20 23:12:53 +0900
comments : true
categories: Note
tags: 
---


레벨 1이니까 매일매일 3개는 풀어야지

<br><br><br>
<hr>
<br><br><br>

#### **16.** 나누어 떨어지는 숫자 배열

<br>

**문제 설명**

<br>

array의 각 element 중 divisor로 나누어 떨어지는 값을 오름차순으로 정렬한 배열을 반환하는 함수, solution을 작성해주세요.

divisor로 나누어 떨어지는 element가 하나도 없다면 배열에 -1을 담아 반환하세요.

<br><br>

**제한사항**

<br>

arr은 자연수를 담은 배열입니다.

정수 i, j에 대해 i ≠ j 이면 arr[i] ≠ arr[j] 입니다.

divisor는 자연수입니다.

array는 길이 1 이상인 배열입니다.

<br><br>

**입출력 예**

<br>

arr | divisor | return
--- | ------- | ------
[5, 9, 7, 10] | 5 | [5, 10]
------------- | - | -------
[2, 36, 1, 3] | 1 | [1, 2, 3, 36]
------------- | - | -------------
[3,2,6] | 0 | [-1]

<br>

**입출력 예 설명**

<br>

-- 입출력 예#1

arr의 원소 중 5로 나누어 떨어지는 원소는 5와 10입니다. 따라서 [5, 10]을 리턴합니다.

<br>

-- 입출력 예#2

arr의 모든 원소는 1으로 나누어 떨어집니다. 원소를 오름차순으로 정렬해 [1, 2, 3, 36]을 리턴합니다.

<br>

-- 입출력 예#3

3, 2, 6은 10으로 나누어 떨어지지 않습니다. 나누어 떨어지는 원소가 없으므로 [-1]을 리턴합니다.

<br><br>

```javascript
function solution(arr, divisor) {
    const answer = [];
    for (let num of arr) {
        if (!(num % divisor)) {
            answer.push(num);
        }
    }
    return !answer.length ? [-1] : answer.sort((a,b) => a-b);
}
```

<br>

```javascript
function solution(arr, divisor) {
    arr = arr.filter(v => !(v % divisor));
    return arr.length ? arr.sort((a, b) => a - b) : [-1];
}
```

<br><br>

다양하게 풀어 보려고 노력했다.

소스는 filter가 간결해보이지만 속도? 효율은 for문이 더 좋은듯?

<br><br><br>
<hr>
<br><br><br>

#### **17.** 제일 작은 수 제거하기

<br>

**문제 설명**

<br>

정수를 저장한 배열, arr 에서 가장 작은 수를 제거한 배열을 리턴하는 함수, solution을 완성해주세요. 단, 리턴하려는 배열이 빈 배열인 경우엔 배열에 -1을 채워 리턴하세요. 예를들어 arr이 [4,3,2,1]인 경우는 [4,3,2]를 리턴 하고, [10]면 [-1]을 리턴 합니다.

<br><br>

**제한 조건**

<br>

arr은 길이 1 이상인 배열입니다.

인덱스 i, j에 대해 i ≠ j이면 arr[i] ≠ arr[j] 입니다.

<br><br>

**입출력 예**

<br>

arr | return
--- | ------
[4,3,2,1] | [4,3,2]
--------- | -------
[10] | [-1]

<br><br>

```javascript
function solution(arr) {
    if (arr.length === 1) return [-1];
    
    arr.splice(arr.indexOf(Math.min(...arr)), 1);
    return arr;
}
```

<br><br>

일단 값이 1개라면 바로 빠져나와서 불필요한 연산을 하지 않도록 했다.

<br>

Math.min으로 최소값을 구해 제거했는데,

Math.min은 배열을 바로 넣으면 계산이 되지 않아서 [...]라고 전개구문을 넣어줘야 했다.

<br>

또, `splice()`를 바로 return 할 경우

제거하고 난 나머지 배열이 아니라 제거한 값이 return 되어서

나머지 값들이 남은 arr 을 별도로 return 해줘야 했다.

<br>

음음, 그렇구나.

<br><br><br>
<hr>
<br><br><br>

#### **18.** 음양 더하기

<br>

**문제 설명**

<br>

어떤 정수들이 있습니다. 이 정수들의 절댓값을 차례대로 담은 정수 배열 absolutes와 이 정수들의 부호를 차례대로 담은 불리언 배열 signs가 매개변수로 주어집니다. 실제 정수들의 합을 구하여 return 하도록 solution 함수를 완성해주세요.

<br><br>

**제한사항**

<br>

absolutes의 길이는 1 이상 1,000 이하입니다.

absolutes의 모든 수는 각각 1 이상 1,000 이하입니다.

signs의 길이는 absolutes의 길이와 같습니다.

signs[i] 가 참이면 absolutes[i] 의 실제 정수가 양수임을, 그렇지 않으면 음수임을 의미합니다.

<br><br>

**입출력 예**

<br>

absolutes | signs | result
--------- | ----- | ------
[4,7,12] | [true,false,true] | 9
-------- | ----------------- | --
[1,2,3] | [false,false,true] | 0

<br>

**입출력 예 설명**

<br>

-- 입출력 예 #1

signs가 [true,false,true] 이므로, 실제 수들의 값은 각각 4, -7, 12입니다.
따라서 세 수의 합인 9를 return 해야 합니다.

<br>

-- 입출력 예 #2

signs가 [false,false,true] 이므로, 실제 수들의 값은 각각 -1, -2, 3입니다.
따라서 세 수의 합인 0을 return 해야 합니다.

<br><br>

```javascript
function solution(absolutes, signs) {
    return absolutes.reduce((a, v, i) => 
                            a + v * (signs[i] ? 1 : -1), 0);
}
```

<br><br>

```javascript
function solution(absolutes, signs) {
    let answer = 0;
    for (let i = 0 ; i < absolutes.length ; i++) {
        answer += absolutes[i] * (signs[i] ? 1 : -1);
    }
    return answer;
}
```

<br><br>

`reduce()`로 먼저 하고 for문으로도 돌려봤는데,

보기에도 그렇고 reduce가 더 나은 것 같다?

<br><br>
