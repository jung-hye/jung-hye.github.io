---
title: "[JS] sort 메서드에 대해"
date: 2020-06-02 14:30:26 -0400
category: JavaScript
---

# [JavaScript] 배열 메서드 sort()



`sort()` 메서드는 배열의 요소를 정렬한 후 해당 배열을 반환합니다.

정렬한 배열로 따로 복사본이 만들어지는 것이 아니라, 원래 배열을 변경하는 거! **원 배열을 변형시키기 때문에 주의해야한다.**

```javascript
const arr = [1, 22, 12, 2, 45, 4];
arr.sort();

console.log(arr);

// 예상되는 output
[1, 2, 4, 12, 22, 45]

// + 기존의 arr 배열이 바뀐다.
```

이렇게 보통 우리가 생각하는 순서대로 나올 거라고 생각하지만, 결과는

```javascript
// 실제 output
[1, 12, 2, 22, 4, 45]
```

이렇게 괴상하게 나온다. 사실 괴상한게 아니라, 원래 sort() 메서드의 기본 정렬 순서는 문자열의 유니코드 포인트를 따른다고 한다.

[MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)의 예제.

```javascript
const array1 = [1, 30, 4, 21, 100000];
array1.sort();
console.log(array1);

// expected output
Array [1, 100000, 21, 30, 4]
```

21보다 10000이 더 낮은 인덱스라는 것이다! 아마 앞자리 수가 더 크기 떄문인 것 같은데 자세한 것은 유니코드 포인트 규칙을 살펴보면 알겠지?

어쨌든, 내가 하고 싶은 것은 평소의 숫자 크기대로 정렬!
10000은 21보다 크다!!



이렇게 하려면, sort() 메서드에 **정렬 순서를 정의하는 함수**를 넣어주면 된다.

```javascript
arr.sort([compareFunction])
```

이 `compareFunction`은 정렬 순서를 정의하는 함수이다.
이 함수가 있으면 배열의 요소들은 이 함수가 어떤 값을 반환하느냐에 따라 정렬된다. 

```javascript
// 요소 a와 b를 비교했을 때
function compare(a, b) {
  // 1. 어떤 기준에서는 a가 b보다 작음 -> -1 반환
  if (a is less than b by some ordering criterion) {
    return -1;
    // a 다음 b
  }
  // 2. 어떤 기준에서는 a가 b보다 큼 -> 1 반환
  if (a is greater than b by the ordering criterion) {
    return 1;
    // b 다음 a
  }
  // 3. 어떤 기준에서는 a가 b와 같음 -> 0 반환
    return 0;
    // 순서 바뀜
}
```

```javascript
// 기본 오름차순 정렬
var numbers = [4, 2, 5, 1, 3];
numbers.sort(function(a, b) {
  return a - b;
  // 4와 2를 비교했을 때 4 - 2 = 2, 즉 양수니까 b가 먼저 오고 다음 a가 온다.
});
console.log(numbers);

// console output
// [1, 2, 3, 4, 5]
```