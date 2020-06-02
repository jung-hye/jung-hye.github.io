---
title: "[JavaScript] 배열 메서드에 대해"
date: 2020-06-02 18:30:26 -0400
category: JavaScript
---

# [JavaScript] 배열 메서드를 알아보자()


1. push(elem)

    `배열에 새 엘리먼트 추가한다.`
    
    - 변경시키고 싶은 배열.push(추가하고 싶은 엘리먼트)
    - 파라미터값이 꼭 있어야한다!
    - 기존 배열을 변형시킨다.

    ```javascript
    const arr = ['a', 'b', 'c'];
    arr.push('d');
    console.log(arr);

    // output
    ['a', 'b', 'c', 'd'];
    ```
2. pop(elem)

    `배열의 가장 마지막 엘리먼트를 제거한다.`

    - 변경시키고 싶은 배열.pop()
    - 배열에 가장 마지막 엘리먼트를 제거한다.
    - 기존 배열을 변형시킨다.
    - 파라미터 값이 없다! 그냥 쓰면 된다.

    ```javascript
    const arr = ['a', 'b', 'c'];
    arr.pop();
    console.log(arr);

    // output
    ['a', 'b'];
    ```
3. pop(elem)

    `배열에 가장 마지막 엘리먼트를 제거한다.`

    - 변경시키고 싶은 배열.pop()
    - 기존 배열을 변형시킨다.
    - 파라미터 값이 없다! 그냥 쓰면 된다.

    ```javascript
    const arr = ['a', 'b', 'c'];
    // 마지막 파라미터 반환
    console.log(arr.pop());
    // 위 콘솔 로그로 이미 pop 실행되고 나머지 배열 출력
    console.log(arr);

    // output
    'c';
    // output
    ['a', 'b'];
    ```
4. shift()

    `배열에 가장 처음 엘리먼트를 제거한다.`

    - 변경시키고 싶은 배열.shift()
    - 기존 배열을 변형시킨다.
    - 파라미터 값이 없다! 그냥 쓰면 된다.

    ```javascript
    const groceryList = ['orange juice', 'bananas', 'coffee beans', 'brown rice'];

    // 첫 번째 항목 삭제
    groceryList.shift();

    console.log(groceryList);

    //output (orange juice가 빠졌다)
    [ 'bananas', 'coffee beans', 'brown rice']
    ```

5. unshift(elem)

    `배열에 가장 처음자리에 엘리먼트를 삽입한다.`

    - 변경시키고 싶은 배열.unshift()
    - 기존 배열을 변형시킨다.

    ```javascript
    const groceryList = ['orange juice', 'bananas', 'coffee beans', 'brown rice'];

    // 첫 번째 항목 삭제
    groceryList.unshift('popcorn');

    console.log(groceryList);

    //output (orange juice가 빠졌다)
    ['popcorn', 'orange juice', 'bananas', 'coffee beans', 'brown rice']
    ```

5. slice(index, index)

    `원하는 구간의 엘리먼트만 슬라이스한다.`

    - 변경시키고 싶은 배열.slice(index, index)
    - 기존 배열읇 변형시키지 않고, 새로운 배열을 반환한다.

    ```javascript
    const groceryList = [ 'popcorn', 'bananas', 'coffee beans', 'brown rice']
    // 0번부터 2번 항목까지(2번 항목 포함) 추출
    // 두번쨰 인자의 인덱스는 포함하지 않는다. 그 전까지만 추출해줌
    console.log(groceryList.slice(0, 3));

    //output (orange juice가 빠졌다)
    ['popcorn', 'bananas', 'coffee beans']
    ```

