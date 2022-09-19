---
layout: post
title:  프로그래머스 연습문제 Day3
date:   2022-09-19 21:22:53 +0900
comments : true
categories: Note
tags: 
---


학원 수강 첫날! 초반은 온라인 수업이라 심심하다. 어서 오프라인 수업날이 왔으면..

<br><br><br>
<hr>
<br><br><br>

#### **11.** x만큼 간격이 있는 n개의 숫자

<br>

**문제 설명**

<br>

함수 solution은 정수 x와 자연수 n을 입력 받아, x부터 시작해 x씩 증가하는 숫자를 n개 지니는 리스트를 리턴해야 합니다. 다음 제한 조건을 보고, 조건을 만족하는 함수, solution을 완성해주세요.

<br><br>

**제한 조건**

<br>

x는 -10000000 이상, 10000000 이하인 정수입니다.

n은 1000 이하인 자연수입니다.

<br><br>

**입출력 예**

<br>

x | n | answer
-- | -- | ------
2 | 5 | [2,4,6,8,10]
-- | -- | ----------
4 | 3 | [4,8,12]
- | - | ---------
-4 | 2 | [-4, -8]

<br><br>

매번 배열로 만들었더니 이제 많이 익숙해졌다.

<br>

```javascript
function solution(x, n) {
    return Array(n).fill().map((item, index) => x * (index + 1));
}
```

<br><br><br>
<hr>
<br><br><br>

#### **12.** 콜라츠 추측

<br>

**문제 설명**

<br>

1937년 Collatz란 사람에 의해 제기된 이 추측은, 주어진 수가 1이 될 때까지 다음 작업을 반복하면, 모든 수를 1로 만들 수 있다는 추측입니다. 작업은 다음과 같습니다.

<br>

1-1. 입력된 수가 짝수라면 2로 나눕니다. 

1-2. 입력된 수가 홀수라면 3을 곱하고 1을 더합니다. 

2. 결과로 나온 수에 같은 작업을 1이 될 때까지 반복합니다. 

<br>

예를 들어, 주어진 수가 6이라면 6 → 3 → 10 → 5 → 16 → 8 → 4 → 2 → 1 이 되어 총 8번 만에 1이 됩니다. 위 작업을 몇 번이나 반복해야 하는지 반환하는 함수, solution을 완성해 주세요. 단, 주어진 수가 1인 경우에는 0을, 작업을 500번 반복할 때까지 1이 되지 않는다면 –1을 반환해 주세요.

<br><br>

**제한 사항**

<br>

입력된 수, num은 1 이상 8,000,000 미만인 정수입니다.

<br><br>

**입출력 예**

<br>

n | result
- | ------
6 | 8
- | --
16 | 4
-- | ---
626331 | -1

<br>

**입출력 예 설명**

<br>

-- 입출력 예 #1

문제의 설명과 같습니다.

<br>

-- 입출력 예 #2

16 → 8 → 4 → 2 → 1 이 되어 총 4번 만에 1이 됩니다.

<br>

-- 입출력 예 #3

626331은 500번을 시도해도 1이 되지 못하므로 -1을 리턴해야 합니다.

<br>

※ 공지 - 2022년 6월 10일 다음과 같이 지문이 일부 수정되었습니다.

주어진 수가 1인 경우에 대한 조건 추가

<br><br>

```javascript
function solution(num) {
    for (let i = 0 ; i < 500 ; i++) {
        if (num === 1) return i;
        
        if (num % 2) {
            num = (num * 3) + 1;
        } else {
            num = num / 2;
        }
    }
    return -1;
}
```

<br>

626331은 언제쯤 1이 될까?

<br><br><br>
<hr>
<br><br><br>

#### **13.** 두 정수 사이의 합

<br>

**문제 설명**

<br>

두 정수 a, b가 주어졌을 때 a와 b 사이에 속한 모든 정수의 합을 리턴하는 함수, solution을 완성하세요.

예를 들어 a = 3, b = 5인 경우, 3 + 4 + 5 = 12이므로 12를 리턴합니다.

<br><br>

**제한 조건**

<br>

a와 b가 같은 경우는 둘 중 아무 수나 리턴하세요.

a와 b는 -10,000,000 이상 10,000,000 이하인 정수입니다.

a와 b의 대소관계는 정해져있지 않습니다.

<br><br>

**입출력 예**

<br>

a | b | return
-- | -- | -----
3 | 5 | 12
-- | -- | ---
3 | 3 | 3
-- | -- | --
5 | 3 | 12

<br><br>

```javascript
function solution() {
    let arr = Array.from(arguments);
    arr.sort((a,b) => a - b);
    
    for (let i = arr[0] ; i < arr[1] ; i++) {
        arr[0] += i + 1;
    }
    return arr[0];
}
```

<br><br>

작은 숫자를 구해야지.

나는 참 배열을 좋아하는 것 같다...

<br>

왜 생각을 못했을까?

js에는 `Math.min`과 `Math.max`라는 편리한 메서드가 있는데!

이미 알고 있는데!

<br><br>

```javascript
function solution(a, b) {
    let answer = Math.min(a,b);
    for (let i = answer ; i < Math.max(a,b) ; i++) {
        answer += i + 1;
    }
    return answer;
}
```

<br><br>

배열 그만~

<br><br><br>
<hr>
<br><br><br>

#### **14.** 서울에서 김서방 찾기

<br>

**문제 설명**

<br>

String형 배열 seoul의 element중 "Kim"의 위치 x를 찾아, "김서방은 x에 있다"는 String을 반환하는 함수, solution을 완성하세요. seoul에 "Kim"은 오직 한 번만 나타나며 잘못된 값이 입력되는 경우는 없습니다.

<br><br>

**제한 사항**

<br>

seoul은 길이 1 이상, 1000 이하인 배열입니다.

seoul의 원소는 길이 1 이상, 20 이하인 문자열입니다.

"Kim"은 반드시 seoul 안에 포함되어 있습니다.

<br><br>

**입출력 예**

<br>

seoul | return
----- | -------
["Jane", "Kim"] | "김서방은 1에 있다"

<br><br>

아는 거 나왔다 히히.

<br><br>

```javascript
function solution(seoul) {
    return `김서방은 ${seoul.indexOf('Kim')}에 있다`;
}
```

<br><br><br>
<hr>
<br><br><br>

#### **15.** 핸드폰 번호 가리기

<br>

**문제 설명**

<br>

프로그래머스 모바일은 개인정보 보호를 위해 고지서를 보낼 때 고객들의 전화번호의 일부를 가립니다.

전화번호가 문자열 phone_number로 주어졌을 때, 전화번호의 뒷 4자리를 제외한 나머지 숫자를 전부 *으로 가린 문자열을 리턴하는 함수, solution을 완성해주세요.

<br><br>

**제한 조건**

<br>

phone_number는 길이 4 이상, 20이하인 문자열입니다.

<br><br>

**입출력 예**

<br>

phone_number | return
------------ | -------
"01033334444" | "*******4444"
------------- | -------------
"027778888" | "*****8888"

<br><br>

처음에 보고 `replace()`를 떠올렸는데 솔직히 정규식은 전혀 몰라서 찾아봤다.

히히. 검색 최고.

```javascript
function solution(phone_number) {
    return phone_number.replace(/\d(?=\d{4})/g, "*");
}
```

<br><br>

근데 `repeat()`과 `slice()`를 쓰는 쪽이 더 직관적인 것 같다.

<br><br>

보면 알겠는데 막상 생각하려면 생각이 안나는 건 아직 많이 사용해보지 않아서 그렇겠지.

문자열 가지고 노는 메서드들을 더 많이 써봐야겠다.

<br><br>

```javascript
function solution(phone_number) {
    return '*'.repeat(phone_number.length - 4) + phone_number.slice(-4);
}
```

<br><br>
