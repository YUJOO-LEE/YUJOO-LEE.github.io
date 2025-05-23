---
layout: post
title:  프로그래머스 연습문제 Day7
date:   2022-09-24 13:16:03 +0900
comments : true
categories: Note
tags: [programmers, algorithm, javascript, sort]
---


이틀동안 블로그 테마 바꿔서 리뉴얼 하느라 못풀었으니까

오늘은 많이 풀어야지.

<br><br><br>
<hr>
<br><br><br>

### **23.** 문자열 내림차순으로 배치하기

<br>

**문제 설명**

문자열 s에 나타나는 문자를 큰것부터 작은 순으로 정렬해 새로운 문자열을 리턴하는 함수, solution을 완성해주세요.

s는 영문 대소문자로만 구성되어 있으며, 대문자는 소문자보다 작은 것으로 간주합니다.

<br>

**제한 사항**

str은 길이 1 이상인 문자열입니다.

<br>

**입출력 예**

s | return
- | ------
"Zbcdefg" | "gfedcbZ"

<br><br>

```javascript
function solution(s) {
    return s.split('').sort().reverse().join('');
}
```

<br>

문제를 잘못봐서 삽질했다.. (오름차순인데 대문자만 뒤로 가야하는 줄 알았음)

프로그래머스 페이지에서 백스페이스랑 탭키 입력이 안돼서 콘솔창에서 만들어다 붙여넣어서 채점하다 깨달았다.

아래가 원래 만들었던 틀린 답...

<br>

```javascript
function solution(s) {
  return s.split('').sort((a, b) => b === b.toUpperCase() ? -1 : 1 ).join('');
}
  //solution('abZbcdefg'); => 'abbcdefgZ'
```

<br>

어쨋든 return값이 +인지 -인지가 중요한걸 이것저것 넣어보면서 숙지했고,

sort의 정렬 조건에 대해 잘 생각해 본 계기가 되었다.

<br>

음... 문제를 잘 읽자!

<br><br><br>
<hr>
<br><br><br>

### **24.** 문자열 다루기 기본

<br>

**문제 설명**

문자열 s의 길이가 4 혹은 6이고, 숫자로만 구성돼있는지 확인해주는 함수, solution을 완성하세요. 예를 들어 s가 "a234"이면 False를 리턴하고 "1234"라면 True를 리턴하면 됩니다.

<br>

**제한 사항**

s는 길이 1 이상, 길이 8 이하인 문자열입니다.

s는 영문 알파벳 대소문자 또는 0부터 9까지 숫자로 이루어져 있습니다.

<br>

**입출력 예**

s | return
- | ------
"a234" | false
------ | ------
"1234" | true

<br><br>

```javascript
function solution(s) {
    return (s.length === 4 || s.length === 6) && !/[a-z]/gi.test(s);
}
```

<br>

처음에는 `!isNaN(Number(s))` 이런식으로 했는데, 영문자 e가 들어가면 지수로 인식해서 문자로 인식을 못해서 문제였다.

그래서 간단한 정규식을 넣어서 해결했다.

근데 나중에 보니 문자열 길이도 정규식으로 넣을 수 있는 것 같다.

정규식 어렵네...

일단은 js가 더 중요하니 정규식은 나중에 공부하는 걸로.

<br><br><br>
<hr>
<br><br><br>

### **25.** 약수의 개수와 덧셈

<br>

**문제 설명**

두 정수 left와 right가 매개변수로 주어집니다. left부터 right까지의 모든 수들 중에서, 약수의 개수가 짝수인 수는 더하고, 약수의 개수가 홀수인 수는 뺀 수를 return 하도록 solution 함수를 완성해주세요.

<br>

**제한사항**

1 ≤ left ≤ right ≤ 1,000

<br>

**입출력 예**

left | right | result
---- | ----- | ------
13 | 17 | 43
-- | -- | --
24 | 27 | 52
-- | -- | --

<br>

**입출력 예 설명**

-- 입출력 예 #1

다음 표는 13부터 17까지의 수들의 약수를 모두 나타낸 것입니다.

수 | 약수 | 약수의 개수
---- | ----- | ------
13 | 1, 13 | 2
---- | ----- | ------
14 | 1, 2, 7, 14 | 4
---- | ----- | ------
15 | 1, 3, 5, 15 | 4
---- | ----- | ------
16 | 1, 2, 4, 8, 16 | 5
---- | ----- | ------
17 | 1, 17 | 2

따라서, 13 + 14 + 15 - 16 + 17 = 43을 return 해야 합니다.

<br>

-- 입출력 예 #2

다음 표는 24부터 27까지의 수들의 약수를 모두 나타낸 것입니다.

수 | 약수 | 약수의 개수
---- | ----- | ------
24 | 1, 2, 3, 4, 6, 8, 12, 24 | 8
---- | ----- | ------
25 | 1, 5, 25 | 3
---- | ----- | ------
26 | 1, 2, 13, 26 | 4
---- | ----- | ------
27 | 1, 3, 9, 27 | 4

따라서, 24 - 25 + 26 + 27 = 52를 return 해야 합니다.

<br><br>

```javascript
function solution(left, right) {
    let answer = 0;
    for (let i = left ; i <= right ; i++) {

        let count = 0;
        for (let j = 1 ; j <= i ; j++) {
            i % j === 0 ? count++ : count;
        }

        count % 2 === 0 ? answer += i : answer -= i;
    }
    return answer;
}
```

<br><br>

for문이 2개가 들어가서 개인적으로 별론데 멋지게 줄이지를 못하겠다.

어렵네..

<br><br><br>
<hr>
<br><br><br>

### **26.** 행렬의 덧셈

<br>

**문제 설명**

행렬의 덧셈은 행과 열의 크기가 같은 두 행렬의 같은 행, 같은 열의 값을 서로 더한 결과가 됩니다. 2개의 행렬 arr1과 arr2를 입력받아, 행렬 덧셈의 결과를 반환하는 함수, solution을 완성해주세요.

<br>

**제한 조건**

행렬 arr1, arr2의 행과 열의 길이는 500을 넘지 않습니다.

<br>

**입출력 예**

arr1 | arr2 | return
---- | ---- | ------
[[1,2],[2,3]] | [[3,4],[5,6]] | [[4,6],[7,9]]
------------ | ------------- | ------------
[[1],[2]] | [[3],[4]] | [[4],[6]]

<br><br>

```javascript
function solution(arr1, arr2) {
    for (let i = 0 ; i < arr1.length ; i++) {
        for (let j = 0 ; j < arr1[i].length ; j++) {
            arr1[i][j] += arr2[i][j];
        }
    }
    return arr1;
}
```

<br>

게임 만들면서 사용해봐서 for문을 중첩으로 돌리는 방법을 제일 먼저 떠올렸는데,

코드 실행하면서 map도 생각났다.

요즘 문제를 배열로 많이 풀다보니 map도 떠오르는 것 같다.

역시 많이 써봐야 머릿속에 들어가는 듯.

<br>

```javascript
function solution(arr1, arr2) {
    return arr1.map((a,i) => arr1[i].map((b,j) => b += arr2[i][j]));
}
```

<br><br><br>
<hr>
<br><br><br>

### **27.** 부족한 금액 계산하기

<br>

**문제 설명**

새로 생긴 놀이기구는 인기가 매우 많아 줄이 끊이질 않습니다. 이 놀이기구의 원래 이용료는 price원 인데, 놀이기구를 N 번 째 이용한다면 원래 이용료의 N배를 받기로 하였습니다. 즉, 처음 이용료가 100이었다면 2번째에는 200, 3번째에는 300으로 요금이 인상됩니다.

놀이기구를 count번 타게 되면 현재 자신이 가지고 있는 금액에서 얼마가 모자라는지를 return 하도록 solution 함수를 완성하세요.

단, 금액이 부족하지 않으면 0을 return 하세요.

<br>

**제한사항**

놀이기구의 이용료 price : 1 ≤ price ≤ 2,500, price는 자연수

처음 가지고 있던 금액 money : 1 ≤ money ≤ 1,000,000,000, money는 자연수

놀이기구의 이용 횟수 count : 1 ≤ count ≤ 2,500, count는 자연수

<br>

**입출력 예**

price | money | count | result
----- | ----- | ----- | ------
3 | 20 | 4 | 10

<br>

**입출력 예 설명**

-- 입출력 예 #1

이용금액이 3인 놀이기구를 4번 타고 싶은 고객이 현재 가진 금액이 20이라면, 총 필요한 놀이기구의 이용 금액은 30 (= 3+6+9+12) 이 되어 10만큼 부족하므로 10을 return 합니다.

<br><br>

오... 서술형...

<br>

```javascript
function solution(price, money, count) {
    return Math.abs(money - (price * (count+1)) * (count / 2));
}
```

<br>

처음에는 위와 같이 풀었다가

'금액이 부족하지 않으면'이라는 조건이 있어서 아래와 같이 수정했다.

<br>

```javascript
function solution(price, money, count) {
    return price * (count + 1) * (count / 2) > money 
        ? price * (count + 1) * (count / 2) - money 
        : 0;
}
```

<br><br><br>
<hr>
<br><br><br>

### **28.** 직사각형 별찍기

<br>

**문제 설명**

이 문제에는 표준 입력으로 두 개의 정수 n과 m이 주어집니다.

별(*) 문자를 이용해 가로의 길이가 n, 세로의 길이가 m인 직사각형 형태를 출력해보세요.

<br>

**제한 조건**

n과 m은 각각 1000 이하인 자연수입니다.

<br>

**예시**

-- 입력

```
5 3
```

-- 출력

```console
*****
*****
*****
```

<br><br>

```javascript
process.stdin.setEncoding('utf8');
process.stdin.on('data', data => {
    const n = data.split(" ");
    const a = Number(n[0]), b = Number(n[1]);
    for (let i = 0 ; i < b ; i++) {
        console.log('*'.repeat(a));
    }
});
```

<br>

첫째줄, 둘째줄의 process.stdin은 뭔지 모르겠지만 어쨋든 미리 a, b값을 뽑아줘서 편하게 할 수 있었다.

트리만들고 모래시계 만들면서 난리를 쳤던 신나는 별찍기...

for문 안돌리고 repeat만 두번 써서도 해봤다.

뭐가 더 나은 건진 잘 모르겠다.

<br>

```javascript
process.stdin.setEncoding('utf8');
process.stdin.on('data', data => {
    const n = data.split(" ");
    const a = Number(n[0]), b = Number(n[1]);
    console.log(`${'*'.repeat(a)}\n`.repeat(b));
});
```

<br><br>

