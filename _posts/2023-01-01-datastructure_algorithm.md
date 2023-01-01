---
layout: post
title:  그림으로 쉽게 배우는 자료구조와 알고리즘 (기본편)
date:   2023-01-01 20:53:05 +0900
comments : true
categories: Review
tags: [datastructure, algorithm]
---

인프런 강의 [그림으로 쉽게 배우는 자료구조와 알고리즘 (기본편)](https://www.inflearn.com/course/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EA%B8%B0%EB%B3%B8) 수강내용 정리

<br>

### 시간복잡도

특정 알고리즘이 어떤 문제를 해결하는 데 걸리는 시간을 표현하는 방법이다.

반복문은 코드 성능에 많은 영향을 준다.

반복문이 여러 번 반복될수록 실행시간이 길어진다.

따라서 특정 알고리즘의 성능을 평가할 때 해당 알고리즘의 반복문을 보고 성능을 평가한다.

<br>

#### Big-Ω (빅 오메가)

알고리즘의 최선의 시간 복잡도를 나타낸다.

즉, 알고리즘의 수행 시간이 가장 짧은 경우를 표기한다.

<br>

#### Big-O (빅 오)

알고리즘의 최악의 시간복잡도를 나타낸다.

즉, 알고리즘의 수행 시간이 가장 오래 걸리는 경우를 표기한다. 

<br>

#### Big-θ (빅 세타)

특정 알고리즘의 범위를 나타낸다.

알고리즘의 수행 시간이 A보다는 크고 B보다는 작다고 표기하는 표현법이다.

빅세타는 일종의 평균값과 유사한데, 평균값이 높은 정확성을 얻기 위해서는 많은 경우의 수가 필요하다.

또한 때로는 구하기 어려울 수도 있으며 공학적인 신뢰도는 평균보다는 최악의 값이 더 높기 때문에 빅세타보다 빅오를 가장 흔하게 사용한다.

<br>

### 배열

일반적으로 프로그래밍 언어에서 배열을 선언할 때에는 배열의 크기를 알려주어야 한다.

```c
int arr[10] = {1, 2, 3, 4, 5};
```

위와 같이 선언하면 운영체제는 메모리에서 숫자 10개가 들어갈 연속된 빈 공간을 찾아서 1, 2, 3, 4, 5 를 할당하고, 배열의 시작 주소만 기억한다.

프로그램에서 배열의 세 번째 원소에 접근하고 싶다면 `arr[2]` 와 같이 인덱스로 한번에 접근한다.

인덱스 참조는 길이에 상관 없이 한 번에 가져오기 때문에 O(1) 의 성능을 가진다.

따라서 배열은 참조에서 좋은 성능을 보인다.

하지만 데이터 삽입, 삭제의 성능은 좋지 않다.

운영체제는 처음에 요청한 길이 만큼의 메모리만 할당했기 때문에, 데이터가 추가된다면 추가된 길이를 포함한 전체 길이 만큼의 메모리를 다시 찾아서 새로 할당해야 하기 때문이다.

하지만 자바스크립트에서 배열은 상황에 따라서 연속적, 불연속적으로 메모리를 할당한다.

불연속적으로 메모리를 할당한 후 내부적으로 연결해서 사용자에게 연속된 것처럼 보이도록 한다.

<br>

### 연결리스트

배열의 단점을 개선하기 위해 나온 방법으로, 저장하려는 데이터를 메모리 공간에 분산해서 할당하고 이 데이터들을 서로 연결해주는 방법이다.

데이터를 담는 변수와 다음 데이터를 가리키는 노드(Node) 를 만들어서 수행한다.

연결리스트는 첫 노드의 주소만 알고 있으면 다른 모든 노드에 접근할 수 있다.

또한 데이터를 삽입할 때 빈 메모리 공간 아무 곳에 추가되는 데이터를 생성하고 기존 리스트에 연결해주면 되기 때문에 삽입, 삭제의 성능이 좋다.

하지만 배열처럼 메모리에 연속적으로 저장되어 있지 않기 때문에 데이터 참조 시에는 첫번째 노드에서 참조 해당 노드까지 하나씩 찾아야 하기 때문에 O(n) 의 성능을 가진다.

<br>

### 스택

먼저 들어간 데이터가 나중에 나오는 규칙이 있는 자료구조이다.

head 에만 삽입, 제거한다.

<br>

### 큐

먼저 들어간 데이터가 먼저 나오는 규칙이 있는 자료구조이다.

head 에서 삽입하고 tail 에서 제거한다.

<br>

### 덱

데이터의 삽입, 제거를 head 와 tail 에서 선택해 자유롭게 할 수 있는 자료구조이다.

<br>

### 해시테이블

해시(Hash), 맵(Map), 해시맵(HashMap), 딕셔너리(Dictionary) 라고도 불린다.

일정한 계산을 하는 해시 함수를 거쳐서 테이블에 Key, Value 의 형태로 데이터를 저장하는 자료구조이다.

저장할때 동일 Key 에 여러개의 Value 가 들어가 충돌이 날 것을 대비해 Value 를 연결리스트로 구성한다.

Key 만 알면 Value 에 O(1)의 성능으로 읽기, 삽입, 수정, 삭제가 가능하다.

자바스크립트의 객체는 해시테이블이다.

<br>

### 셋

데이터의 중복을 허용하지 않는 자료구조이다.

해시테이블을 이용해서 구현하기 때문에 해시셋이라고도 한다.

셋 구현시에는 해시테이블의 Key 만 사용하고 Value 는 사용하지 않는다.

Key 가 Key 임과 동시에 Value 의 역할도 하기 때문이다.

<br>

### 재귀함수

자기 자신을 참조하는 함수이다.

무한반복 되지 않도록 기저 조건(탈출 조건)을 반드시 설정한다.

```javascript
function recursion(number) {
  if (number > 10) return;  // 기저 조건

  console.log(number);
  recursion(number + 1);  // 자기 자신 호출
}

recursion(1);
```

위 코드 실행 시 함수를 1 으로 시작해 해당 숫자를 콘솔에 표시하고, 해당 숫자에 1을 더해 반복한 후, 숫자가 10 보다 커지면 종료한다.

재귀는 두가지 패턴을 가진다.

첫번째는 위와 같이 단순히 반복 실행을 하는 것이다.

이 경우 일반적인 반복문과 비교해 이점이 되는 부분이 없고 오히려 성능은 for 문보다 좋지 않다.

<br>

두번째 패턴은 하위 문제의 결과를 기반으로 현재 문제를 계산하는 것이다.

재귀의 위력은 이러한 하향식 계산에 있다.

아래 팩토리얼을 구하는 함수로 하향식 계산을 이해할 수 있다.

```javascript
function factorial(number) {
  if (number === 1 || number === 0) return 1;
  
  return number * factorial(number - 1);
}

console.log(factorial(5));
```

위 코드의 `number * factorial(number - 1)` 이 하위 문제인 `factorial(number - 1)` 부분이 이미 계산되었음을 예상하고 해당 값으로 현재 문제를 계산하는 부분이다.

<br>

### 정렬

배열에 무작위로 섞인 숫자를 정렬하는 방법은 여러가지가 있다.

<br>

#### 버블 정렬

서로 인접한 두 원소를 검사하여 정렬하는 알고리즘으로, 인접한 2개의 데이터를 비교하여 크기가 순서대로 되어 있지 않으면 서로 교환한다.

이해와 구현이 간단하지만 성능이 O(n2) 으로 좋지 않다.

<br>

#### 선택 정렬

정렬되지 않은 영역의 첫 번째 원소를 시작으로, 마지막 원소까지 비교 후, 가장 작은 값을 첫 번째 원소로 가져오는 것을 반복하는 알고리즘이다.

버블 정렬과 마찬가지로 이해와 구현이 간단하지만 성능이 O(n2) 으로 좋지 않다.

<br>

#### 삽입 정렬

정렬되지 않은 영역에서 데이터를 하나씩 꺼내서 정렬된 영역 내 알맞은 위치에 삽입해서 정렬하는 알고리즘이다.

버블 정렬, 선택 정렬과 마찬가지로 이해와 구현이 간단하지만 성능이 O(n2) 으로 좋지 않다.

<br>

#### 병합 정렬

하나의 리스트를 반으로 쪼개고 분할된 부분 리스트를 정렬한 다음, 두 개의 정렬된 부분 리스트를 병합하여 전체가 정렬된 리스트가 되게 하는 방법으로, 재귀를 통해 구현하는 정렬 알고리즘이다.

이해와 구현이 다소 어려울 수 있지만, O(nlogn) 으로 성능이 좋다.

```javascript
function merge(arr, leftIndex, midIndex, rightIndex) {
  let leftAreaIndex = leftIndex;
  let rightAreaIndex = midIndex + 1;
  const tempArr = Array(rightIndex + 1).fill(0);
  let tempArrIndex = leftIndex;

  while (leftAreaIndex <= midIndex && rightAreaIndex <= rightIndex) {
    if (arr[leftAreaIndex] <= arr[rightAreaIndex]) {
      tempArr[tempArrIndex] = arr[leftAreaIndex++];
    } else {
      tempArr[tempArrIndex] = arr[rightAreaIndex++];
    }
    tempArrIndex++;
  }

  if (leftAreaIndex > midIndex) {
    for (let i = rightAreaIndex; i <= rightIndex; i++) {
      tempArr[tempArrIndex++] = arr[i];
    }
  } else {
    for (let i = leftAreaIndex; i <= midIndex; i++) {
      tempArr[tempArrIndex++] = arr[i];
    }
  }

  for (let i = leftIndex; i <= rightIndex; i++) {
    arr[i] = tempArr[i];
  }
}

function mergeSort(arr, leftIndex, rightIndex) {
  if (leftIndex < rightIndex) {
    const midIndex = parseInt((leftIndex + rightIndex) / 2);
    mergeSort(arr, leftIndex, midIndex);
    mergeSort(arr, midIndex + 1, rightIndex);
    merge(arr, leftIndex, midIndex, rightIndex);
  }
}

const arr = [3, 5, 2, 4, 1, 7, 8, 6];

console.log('===== 정렬 전 =====');
console.log(arr);

mergeSort(arr, 0, arr.length - 1);
console.log('==== 정렬 후 =====');
console.log(arr);
```

<br>

#### 퀵 정렬

병합 정렬과 같이 재귀를 사용하는 분할 정복 알고리즘이다.

정렬하기 전, 배열에 있는 숫자 중 하나를 피벗으로 설정한다.

그 피벗을 기준으로 두 개의 비균등한 크기로 분할하고 분할된 부분 리스트를 정렬한 다음, 두 개의 정렬된 부분 리스트를 합하여 전체가 정렬된 리스트가 되게 한다.

고른 피벗의 값에 따라서 성능이 달라질 수 있다.

평균적으로 θ(nlogn), 최악의 경우 O(n2)의 성능을 갖는데, 실제로 병합 정렬과 비교하면 퀵정렬이 더 적은 비교와 메모리 공간을 차지하기 때문에 더 좋은 알고리즘으로 평가된다.

```javascript
function quickSort(arr, left, right) {
  if (left <= right) {
    const pivot = divide(arr, left, right);
    quickSort(arr, left, pivot - 1);
    quickSort(arr, pivot + 1, right);
  }
}

function divide(arr, left, right) {
  let pivot = arr[left];
  let leftStartIndex = left + 1;
  let rightStartIndex = right;

  while (leftStartIndex <= rightStartIndex) {
    while (leftStartIndex <= right && pivot >= arr[leftStartIndex]) {
      leftStartIndex++;
    }

    while (rightStartIndex >= (left + 1) && pivot <= arr[rightStartIndex]) {
      rightStartIndex--;
    }

    if (leftStartIndex <= rightStartIndex) {
      swap(arr, leftStartIndex, rightStartIndex);
    }
  }

  swap(arr, left, rightStartIndex);
  return rightStartIndex;
}

function swap(arr, index1, index2) {
  const temp = arr[index1];
  arr[index1] = arr[index2];
  arr[index2] = temp;
}

const arr = [3, 5, 2, 4, 1, 7, 8, 6];

console.log('===== 정렬 전 =====');
console.log(arr);

quickSort(arr, 0, arr.length - 1);
console.log('==== 정렬 후 =====');
console.log(arr);
```

참고로 [베스트셀러모던 자바스크립트 Deep Dive](https://search.shopping.naver.com/book/catalog/32472713016) 에 의하면 자바스크립트의 `Array.sort()` 메서드는 퀵정렬을 사용했었다.

하지만 중복 값이 있을 때 불안정하다는 점이 있어서 ECMAScript 2019(ES10) 에서 timsort 알고리즘을 사용하도록 변경되었다고 한다.

<br>

#### 팀 정렬

강의 내용에는 없었지만 자바스크립트에서 사용한다는 알고리즘이라고 하니 개인적으로 궁금해져서 찾아봤다.

소프트웨어 엔지니어 Tim Peters에 의해 만들어진 알고리즘이다.

삽입 정렬과 병합 정렬을 결합하여 병합 정렬을 최적화했다.

팀 정렬은 일정 사이즈의 부분 리스트를 얻고(divide), 각각의 부분리스트들에 대해 삽입정렬을 수행하며 정렬을 하며(sort), 정렬 된 부분리스트들을 다시 합치는 방식(conqure)을 사용한다.

<iframe width="560" height="315" src="https://www.youtube.com/embed/NVIjHj-lrT4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<br>

### 메모이제이션

하향식 계산 시 동일한 연산의 반복으로 연산 속도가 느려지는 것을 방지하기 위해 연산을 한번 실행한 값을 저장해두고 재연산을 하지 않고 저장값을 사용하는 방법이다.

중복 계산을 하지 않아서 속도가 빠르며, 하향식 계산 방식의 재귀 처리로 어려운 문제를 간단하게 해결할 수 있다.

속도를 위해 메모리를 사용하는 방법으로, 대표적으로 메모리와 CPU의 속도차이를 극복하기 위해 만들어진 캐시가 있다.

아래 두 함수는 피보나치 수열의 특정 위치의 값을 구하는 함수인데, 첫번째는 모든 연산을 진행하고, 두번째는 중복되는 연산은 저장값을 사용하는 방식이다.

```javascript
function fibonacci1(n) {
  if (n === 0 || n === 1) return n;
  return fibonacci1(n - 2) + fibonacci1(n - 1);
}

function fibonacci2(n, memo) {
  if (n === 0 || n === 1) return n;

  if (!memo[n]) {
    memo[n] = fibonacci2(n - 2, memo) + fibonacci2(n - 1, memo);
  }
  return memo[n];
}

let start = new Date();
console.log(fibonacci1(40));
let end = new Date();
console.log(`fibonacci1 실행시간 : ${end - start}ms`);

start = new Date();
console.log(fibonacci2(40, {}));
end = new Date();
console.log(`fibonacci2 실행시간 : ${end - start}ms`);
```

<br>

### 타뷸레이션

메모이제이션과 반대로 상향식 계산 방식을 통해 계산에 필요한 모든 값을 전부 계산 후 테이블에 저장해두고, 계산된 값 중에서 필요한 값을 사용해 계산하는 방식이다.

아래는 위의 피보나치 함수를 타뷸레이션 방식으로 구현한 것이다.

```javascript
function fibonacci3(n) {
  if (n <= 1) return n;

  let table = [0, 1];

  for (let i = 2; i <= n; i++) {
    table[i] = table[i - 2] + table[i - 1];
  }

  return table[n];
}

start = new Date();
console.log(fibonacci3(40, {}));
end = new Date();
console.log(`fibonacci3 실행시간 : ${end - start}ms`);
```

<br>
