---
layout: post
title:  프로그래머스 연습문제 Day11
date:   2022-09-30 21:27:03 +0900
comments : true
categories: Note
tags: [programmers, algorithm, javascript]
---


척척하고 잘 하고 싶다.

언제쯤 그렇게 될까?

<br><br>
<hr>
<br><br>

### 문자열 내 마음대로 정렬하기

<br>

**문제 설명**

문자열로 구성된 리스트 strings와, 정수 n이 주어졌을 때, 각 문자열의 인덱스 n번째 글자를 기준으로 오름차순 정렬하려 합니다. 예를 들어 strings가 ["sun", "bed", "car"]이고 n이 1이면 각 단어의 인덱스 1의 문자 "u", "e", "a"로 strings를 정렬합니다.

<br>

**제한 조건**

- strings는 길이 1 이상, 50이하인 배열입니다.

- strings의 원소는 소문자 알파벳으로 이루어져 있습니다.

- strings의 원소는 길이 1 이상, 100이하인 문자열입니다.

- 모든 strings의 원소의 길이는 n보다 큽니다.

- 인덱스 1의 문자가 같은 문자열이 여럿 일 경우, 사전순으로 앞선 문자열이 앞쪽에 위치합니다.

<br>

**입출력 예**

strings | n | return
---- | ---- | ----
["sun", "bed", "car"] | 1 | ["car", "bed", "sun"]
---- | ---- | ----
["abce", "abcd", "cdx"] | 2 | ["abcd", "abce", "cdx"]

<br>

**입출력 예 설명**

0 입출력 예 1

"sun", "bed", "car"의 1번째 인덱스 값은 각각 "u", "e", "a" 입니다. 이를 기준으로 strings를 정렬하면 ["car", "bed", "sun"] 입니다.

<br>

- 입출력 예 2

"abce"와 "abcd", "cdx"의 2번째 인덱스 값은 "c", "c", "x"입니다. 따라서 정렬 후에는 "cdx"가 가장 뒤에 위치합니다. "abce"와 "abcd"는 사전순으로 정렬하면 "abcd"가 우선하므로, 답은 ["abcd", "abce", "cdx"] 입니다.

<br><br>

```javascript
function solution(strings, n) {
  strings.sort(function(a, b) {
    if (a[n] > b[n]) {
      return +1;
    } else if(a[n] < b[n]) {
      return -1;
    } else if (a > b) {
      return +1;
    } else if(a < b) {
      return -1;
    }
    return 0;
  });
  return strings;
}
```

<br>

이렇게 귀엽게 풀었는데 차암나

<br>

```javascript
function solution(strings, n) {
  return strings.sort().sort((a, b)=> a[n] >= b[n] ? 1 : -1 );
}
```

<br>

이거랑 똑같더라.

<br><br>
<hr>
<br><br>

### K번째수

<br>

**문제 설명**

배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.

예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면

1. array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.    
2. 1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.    
3. 2에서 나온 배열의 3번째 숫자는 5입니다.    
4. 배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

<br>

**제한사항**

- array의 길이는 1 이상 100 이하입니다.

- array의 각 원소는 1 이상 100 이하입니다.

- commands의 길이는 1 이상 50 이하입니다.

- commands의 각 원소는 길이가 3입니다.

<br>

**입출력 예**

array | commands | return
---- | ---- | ----
[1, 5, 2, 6, 3, 7, 4] | [[2, 5, 3], [4, 4, 1], [1, 7, 3]] | [5, 6, 3]

<br>

**입출력 예 설명**

1. [1, 5, 2, 6, 3, 7, 4]를 2번째부터 5번째까지 자른 후 정렬합니다. [2, 3, 5, 6]의 세 번째 숫자는 5입니다.    
2. [1, 5, 2, 6, 3, 7, 4]를 4번째부터 4번째까지 자른 후 정렬합니다. [6]의 첫 번째 숫자는 6입니다.    
3. [1, 5, 2, 6, 3, 7, 4]를 1번째부터 7번째까지 자릅니다. [1, 2, 3, 4, 5, 6, 7]의 세 번째 숫자는 3입니다.

<br><br>

```javascript
function solution(array, commands) {
  return commands.map((v,i)=> 
    array.slice(v[0]-1, v[1])
    .sort((a,b)=>a-b)
    .at(v[2]-1) );
}
```

<br>

음.. 문제에 쓰여진 말 그대로 푼 거라 할 말이 없다.

<br><br>
<hr>
<br><br>

### 숫자 문자열과 영단어

**문제 설명**

네오와 프로도가 숫자놀이를 하고 있습니다. 네오가 프로도에게 숫자를 건넬 때 일부 자릿수를 영단어로 바꾼 카드를 건네주면 프로도는 원래 숫자를 찾는 게임입니다.

다음은 숫자의 일부 자릿수를 영단어로 바꾸는 예시입니다.

- 1478 → "one4seveneight"

- 234567 → "23four5six7"

- 10203 → "1zerotwozero3"

이렇게 숫자의 일부 자릿수가 영단어로 바뀌어졌거나, 혹은 바뀌지 않고 그대로인 문자열 s가 매개변수로 주어집니다. s가 의미하는 원래 숫자를 return 하도록 solution 함수를 완성해주세요.

참고로 각 숫자에 대응되는 영단어는 다음 표와 같습니다.

숫자 | 영단어
--- | ---
0 | zero
--- | ---
1 | one
--- | ---
2 | two
--- | ---
3 | three
--- | ---
4 | four
--- | ---
5 | five
--- | ---
6 | six
--- | ---
7 | seven
--- | ---
8 | eight
--- | ---
9 | nine

<br>

**제한사항**

- 1 ≤ s의 길이 ≤ 50

- s가 "zero" 또는 "0"으로 시작하는 경우는 주어지지 않습니다.

- return 값이 1 이상 2,000,000,000 이하의 정수가 되는 올바른 입력만 s로 주어집니다.

<br>

**입출력 예**

s | result
--- | ---
"one4seveneight" | 1478
--- | ---
"23four5six7" | 234567
--- | ---
"2three45sixseven" | 234567
--- | ---
"123" | 123

<br>

**입출력 예 설명**

- 입출력 예 #1    
문제 예시와 같습니다.

- 입출력 예 #2    
문제 예시와 같습니다.

- 입출력 예 #3    
"three"는 3, "six"는 6, "seven"은 7에 대응되기 때문에 정답은 입출력 예 #2와 같은 234567이 됩니다.    
입출력 예 #2와 #3과 같이 같은 정답을 가리키는 문자열이 여러 가지가 나올 수 있습니다.

- 입출력 예 #4    
s에는 영단어로 바뀐 부분이 없습니다.

<br>

**제한시간 안내**

정확성 테스트 : 10초

<br><br>

```javascript
function solution(s) {
  const words = { 'zero':0, 'one':1, 'two':2, 'three':3, 'four':4, 'five':5, 'six':6, 'seven':7, 'eight':8, 'nine':9}
  for (const key in words) {
    s = s.replaceAll(key, words[key]);
  }
  return Number(s);
}
```

<br>

배열로 할까 하다가 객체로 해봤다.

음. `replaceAll()`은 처음 써봤는데 편하네

<br><br>

