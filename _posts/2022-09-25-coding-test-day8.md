---
layout: post
title:  프로그래머스 연습문제 Day8
date:   2022-09-25 22:56:03 +0900
comments : true
categories: Note
tags: [programmers, algorithm, javascript]
---


30개가 넘어가니 앞에 숫자 붙이기도 힘들다.

숫자 없이 올려야지.

그건 그렇고 왜 맥북으로 프로그래머스에서 타이핑 하려면 백스페이스, 화살표 키가 안먹힐까?

나만 이상한건지 검색해도 안나오고, 재부팅을 아무리 해도 카라비너를 지워봐도 안된다.

걍 포기...

<br><br><br>
<hr>
<br><br><br>

### 최대공약수와 최소공배수

<br>

**문제 설명**

두 수를 입력받아 두 수의 최대공약수와 최소공배수를 반환하는 함수, solution을 완성해 보세요. 배열의 맨 앞에 최대공약수, 그다음 최소공배수를 넣어 반환하면 됩니다. 예를 들어 두 수 3, 12의 최대공약수는 3, 최소공배수는 12이므로 solution(3, 12)는 [3, 12]를 반환해야 합니다.

<br>

**제한 사항**

두 수는 1이상 1000000이하의 자연수입니다.

<br>

**입출력 예**

n | m | return
-- | ---- | ----
3 | 12 | [3, 12]
-- | ---- | ----
2 | 5 | [1, 10]

<br>

**입출력 예 설명**

-- 입출력 예 #1

위의 설명과 같습니다.

<br>

-- 입출력 예 #2

자연수 2와 5의 최대공약수는 1, 최소공배수는 10이므로 [1, 10]을 리턴해야 합니다.

<br><br>

```javascript
function solution(n,m){
  let maxNum = Math.max(n,m);
  let minNum = Math.min(n,m);
  let divisor;
  let multiple;
  for (let i = 1 ; i <= maxNum ; i++) {
    if (n % i === 0 && m % i === 0) {
      divisor = i;
    }
  }
  for (let i = 1 ; i <= n * m ; i++) {
    if ((maxNum * i) % minNum === 0) {
      multiple = maxNum * i;
      return [divisor, multiple]
    }
  }
}
```

<br>

이렇게 하나하나 돌면서 구한 나와...

정말... 진짜... 대단한 유클리드 선생님...

> A * B === 최대공약수 * 최대공배수

이제 알았으면 됐지!

기억하자~

<br>

```javascript
function solution(n,m){
  let maxNum = Math.max(n,m);
  let divisor;
  for (let i = 1 ; i <= maxNum ; i++) {
    if (n % i === 0 && m % i === 0) {
      divisor = i;
    }
  }
  return [divisor, (n * m) / divisor]
}
```

<br><br><br>
<hr>
<br><br><br>

### 같은 숫자는 싫어

<br>

**문제 설명**

배열 arr가 주어집니다. 배열 arr의 각 원소는 숫자 0부터 9까지로 이루어져 있습니다. 이때, 배열 arr에서 연속적으로 나타나는 숫자는 하나만 남기고 전부 제거하려고 합니다. 단, 제거된 후 남은 수들을 반환할 때는 배열 arr의 원소들의 순서를 유지해야 합니다. 예를 들면,

-- arr = [1, 1, 3, 3, 0, 1, 1] 이면 [1, 3, 0, 1] 을 return 합니다.

-- arr = [4, 4, 4, 3, 3] 이면 [4, 3] 을 return 합니다.

배열 arr에서 연속적으로 나타나는 숫자는 제거하고 남은 수들을 return 하는 solution 함수를 완성해 주세요.

<br>

**제한사항**

배열 arr의 크기 : 1,000,000 이하의 자연수

배열 arr의 원소의 크기 : 0보다 크거나 같고 9보다 작거나 같은 정수

<br>

**입출력 예**

arr | answer
--- | ------
[1,1,3,3,0,1,1] | [1,3,0,1]
--------------- | ---------
[4,4,4,3,3] | [4,3]

<br><br>

```javascript
function solution(arr)
{
  return arr.filter((v, i) => v !== arr[i-1]);
}
```

<br>

filter는 많이 안써봤는데 덕분에 다시 공부했다.

<br>

```javascript
function solution(arr)
{
  const answer = [arr[0]];
  for (let i = 1 ; i < arr.length ; i++){
    if (arr[i] !== arr[i-1]){
      answer.push(arr[i]);
    }
  }
  return answer;
}
```

<br>

이건 for문으로 돌린건데, 호율성 테스트는 이쪽이 더 높게 나오는데 그게 뭔지 모르겠다..

<br><br><br>
<hr>
<br><br><br>

### 이상한 문자 만들기

<br>

**문제 설명**

문자열 s는 한 개 이상의 단어로 구성되어 있습니다. 각 단어는 하나 이상의 공백문자로 구분되어 있습니다. 각 단어의 짝수번째 알파벳은 대문자로, 홀수번째 알파벳은 소문자로 바꾼 문자열을 리턴하는 함수, solution을 완성하세요.

<br>

**제한 사항**

문자열 전체의 짝/홀수 인덱스가 아니라, 단어(공백을 기준)별로 짝/홀수 인덱스를 판단해야합니다.

첫 번째 글자는 0번째 인덱스로 보아 짝수번째 알파벳으로 처리해야 합니다.

<br>

**입출력 예**

s | return
-- | ----
"try hello world" | "TrY HeLlO WoRlD"

<br>

**입출력 예 설명**

"try hello world"는 세 단어 "try", "hello", "world"로 구성되어 있습니다. 각 단어의 짝수번째 문자를 대문자로, 홀수번째 문자를 소문자로 바꾸면 "TrY", "HeLlO", "WoRlD"입니다. 따라서 "TrY HeLlO WoRlD" 를 리턴합니다.

<br><br>

```javascript
function solution(s) {
  return s.split(' ')
    .map((v) => {
      return [...v].map((vv, i) => {
        return i % 2 ? vv.toLowerCase() : vv.toUpperCase();
      }).join('');
    }).join(' ');
}
```

<br>

이게.. 이게 맞나?

map을 두번 돌리는게 맞나? 이래도 되는 건가?

괜찮은 건가..?

<br><br>
