---
layout: post
title:  스킬 체크 테스트 Level.1, 2
date:   2022-12-07 17:10:02 +0900
comments : true
categories: Note
tags: [programmers, algorithm, javascript]
---

집중용..

시간제한이 있어서 얼마나 했는지 시간보기 좋다.

<br><br><br>
<hr>
<br><br><br>

### 햄버거 만들기

**문제 설명**

햄버거 가게에서 일을 하는 상수는 햄버거를 포장하는 일을 합니다. 함께 일을 하는 다른 직원들이 햄버거에 들어갈 재료를 조리해 주면 조리된 순서대로 상수의 앞에 아래서부터 위로 쌓이게 되고, 상수는 순서에 맞게 쌓여서 완성된 햄버거를 따로 옮겨 포장을 하게 됩니다. 상수가 일하는 가게는 정해진 순서(아래서부터, 빵 – 야채 – 고기 - 빵)로 쌓인 햄버거만 포장을 합니다. 상수는 손이 굉장히 빠르기 때문에 상수가 포장하는 동안 속 재료가 추가적으로 들어오는 일은 없으며, 재료의 높이는 무시하여 재료가 높이 쌓여서 일이 힘들어지는 경우는 없습니다.

예를 들어, 상수의 앞에 쌓이는 재료의 순서가 [야채, 빵, 빵, 야채, 고기, 빵, 야채, 고기, 빵]일 때, 상수는 여섯 번째 재료가 쌓였을 때, 세 번째 재료부터 여섯 번째 재료를 이용하여 햄버거를 포장하고, 아홉 번째 재료가 쌓였을 때, 두 번째 재료와 일곱 번째 재료부터 아홉 번째 재료를 이용하여 햄버거를 포장합니다. 즉, 2개의 햄버거를 포장하게 됩니다.

상수에게 전해지는 재료의 정보를 나타내는 정수 배열 ingredient가 주어졌을 때, 상수가 포장하는 햄버거의 개수를 return 하도록 solution 함수를 완성하시오.

<br>

**제한사항**

- 1 ≤ ingredient의 길이 ≤ 1,000,000
- ingredient의 원소는 1, 2, 3 중 하나의 값이며, 순서대로 빵, 야채, 고기를 의미합니다.

<br>

**입출력 예**

ingredient | result
--- | ---
[2, 1, 1, 2, 3, 1, 2, 3, 1] | 2
[1, 3, 2, 1, 2, 1, 3, 1, 2] | 0

<br>

**입출력 예 설명**

- 입출력 예 #1

문제 예시와 같습니다.

<br>

- 입출력 예 #2

상수가 포장할 수 있는 햄버거가 없습니다.

<br>

```javascript
function solution(ingredient) {
    let count = 0;
    let index = 0;
    while (index < ingredient.length - 3) {
        if (ingredient[index] === 1 
           && ingredient[index + 1] === 2 
           && ingredient[index + 2] === 3 
           && ingredient[index + 3] === 1 ) {
            ingredient.splice(index, 4);
            count++;
            index = Math.max(0, index - 4);
        } else {
            index++;
        }
    }
    return count;
}
```

<br>

순차적으로 반복하다가 일부 값을 지워내고 다시 앞에서부터 확인해야 한다.

ingredient 의 길이로 올수있는 수가 커서 그냥 반복을 돌리면 너무 오래 걸려서 타임아웃이 발생한다.

효율이 중요한 것 같다.

<br><br><br>
<hr>
<br><br><br>

### 영어 끝말잇기

**문제 설명**

1부터 n까지 번호가 붙어있는 n명의 사람이 영어 끝말잇기를 하고 있습니다. 영어 끝말잇기는 다음과 같은 규칙으로 진행됩니다.


1. 1번부터 번호 순서대로 한 사람씩 차례대로 단어를 말합니다.    
2. 마지막 사람이 단어를 말한 다음에는 다시 1번부터 시작합니다.    
3. 앞사람이 말한 단어의 마지막 문자로 시작하는 단어를 말해야 합니다.    
4. 이전에 등장했던 단어는 사용할 수 없습니다.    
5. 한 글자인 단어는 인정되지 않습니다.    
6. 다음은 3명이 끝말잇기를 하는 상황을 나타냅니다.

> tank → kick → know → wheel → land → dream → mother → robot → tank

위 끝말잇기는 다음과 같이 진행됩니다.

- 1번 사람이 자신의 첫 번째 차례에 tank를 말합니다.    
- 2번 사람이 자신의 첫 번째 차례에 kick을 말합니다.    
- 3번 사람이 자신의 첫 번째 차례에 know를 말합니다.    
- 1번 사람이 자신의 두 번째 차례에 wheel을 말합니다.    
- (계속 진행)    

끝말잇기를 계속 진행해 나가다 보면, 3번 사람이 자신의 세 번째 차례에 말한 tank 라는 단어는 이전에 등장했던 단어이므로 탈락하게 됩니다.

사람의 수 n과 사람들이 순서대로 말한 단어 words 가 매개변수로 주어질 때, 가장 먼저 탈락하는 사람의 번호와 그 사람이 자신의 몇 번째 차례에 탈락하는지를 구해서 return 하도록 solution 함수를 완성해주세요.

<br>

**제한 사항**

- 끝말잇기에 참여하는 사람의 수 n은 2 이상 10 이하의 자연수입니다.    
- words는 끝말잇기에 사용한 단어들이 순서대로 들어있는 배열이며, 길이는 n 이상 100 이하입니다.    
- 단어의 길이는 2 이상 50 이하입니다.    
- 모든 단어는 알파벳 소문자로만 이루어져 있습니다.    
- 끝말잇기에 사용되는 단어의 뜻(의미)은 신경 쓰지 않으셔도 됩니다.    
- 정답은 [ 번호, 차례 ] 형태로 return 해주세요.    
- 만약 주어진 단어들로 탈락자가 생기지 않는다면, [0, 0]을 return 해주세요.    

<br>

**입출력 예**

n | words| result
--- | --- | ---
3 | ["tank", "kick", "know", "wheel", "land", "dream", "mother", "robot", "tank"] | [3,3]
5 | ["hello", "observe", "effect", "take", "either", "recognize", "encourage", "ensure", "establish", "hang", "gather", "refer", "reference", "estimate", "executive"] | [0,0]
2 | ["hello", "one", "even", "never", "now", "world", "draw"] | [1,3]

<br>

**입출력 예 설명**

- 입출력 예 #1

3명의 사람이 끝말잇기에 참여하고 있습니다.    
-- 1번 사람 : tank, wheel, mother    
-- 2번 사람 : kick, land, robot    
-- 3번 사람 : know, dream, tank    
와 같은 순서로 말을 하게 되며, 3번 사람이 자신의 세 번째 차례에 말한 tank라는 단어가 1번 사람이 자신의 첫 번째 차례에 말한 tank와 같으므로 3번 사람이 자신의 세 번째 차례로 말을 할 때 처음 탈락자가 나오게 됩니다.

<br>

- 입출력 예 #2

5명의 사람이 끝말잇기에 참여하고 있습니다.

- 1번 사람 : hello, recognize, gather    
- 2번 사람 : observe, encourage, refer    
- 3번 사람 : effect, ensure, reference    
- 4번 사람 : take, establish, estimate    
- 5번 사람 : either, hang, executive    
와 같은 순서로 말을 하게 되며, 이 경우는 주어진 단어로만으로는 탈락자가 발생하지 않습니다. 따라서 [0, 0]을 return하면 됩니다.

<br>

- 입출력 예 #3

2명의 사람이 끝말잇기에 참여하고 있습니다.

- 1번 사람 : hello, even, now, draw    
- 2번 사람 : one, never, world    
와 같은 순서로 말을 하게 되며, 1번 사람이 자신의 세 번째 차례에 'r'로 시작하는 단어 대신, n으로 시작하는 now를 말했기 때문에 이때 처음 탈락자가 나오게 됩니다.

<br>

```javascript
function checkWord(index, arr) {
    // 인덱스 0번이면 확인없이 통과
    if (!index) return true;

    // 현재 단어 첫자리와 이전 단어 끝자리 확인
    const curWord = arr[index];
    const prevWord = arr[index - 1];
    if (curWord[0] !== prevWord[prevWord.length - 1]) return false;

    // 단어 중복 여부 확인
    for (let i = 0; i < index; i++) if (arr[index] === arr[i]) return false;

    // 문제 없으면 통과
    return true;
}

function solution(n, words) {
    let turn = 1;
    for (let i = 0; i < words.length ; i++) {
        if (!checkWord(i, words)) return [turn, Math.ceil((i + 1) / n)];
        turn = turn < n ? turn + 1 : 1;
    }
    return [0, 0];
}
```

<br>

30분 걸림..

어떻게 할지 꽤 고민했다.

<br><br><br>
<hr>
<br><br><br>

### 최소값, 최대값

**문제 설명**

문자열 s에는 공백으로 구분된 숫자들이 저장되어 있습니다. str에 나타나는 숫자 중 최소값과 최대값을 찾아 이를 "(최소값) (최대값)"형태의 문자열을 반환하는 함수, solution을 완성하세요.

예를들어 s가 "1 2 3 4"라면 "1 4"를 리턴하고, "-1 -2 -3 -4"라면 "-4 -1"을 리턴하면 됩니다.

<br>

**제한 조건**

s에는 둘 이상의 정수가 공백으로 구분되어 있습니다.

<br>

**입출력 예**

s | return
--- | ---
"1 2 3 4" | "1 4"
"-1 -2 -3 -4" | "-4 -1"
"-1 -1" | "-1 -1"

<br>

```javascript
function solution(s) {
    const arr = s.split(' ').sort((a, b) => a - b);
    return `${arr[0]} ${arr[arr.length - 1]}`;
}
```

<br>

이건 보자마자 풀긴 했는데...

방금 전 문제랑 같은 레벨로 나온거면 더 효율적인 방법이 있는걸까?

아니면 윗문제를 너무 어렵게 푼걸까..

<br>
