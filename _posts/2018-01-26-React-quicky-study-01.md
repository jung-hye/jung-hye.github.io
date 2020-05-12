---
title: "[책] React Quickly 공부하기 (1)"
date: 2020-05-12 17:37:18 -0400
category: ReactQuickly
---

## [1. React 살펴보기] 명령형 스타일과 선언형 스타일의 차이

> 이 글은 지은이 **아자트 마르단**의 책 **리액트 교과서**에 나온 내용을 바탕으로 하고 있습니다.


React에서는 **명령형** 대신 **선언형** 스타일을 채택했다.

1. **명령형** 스타일은 개발자가 순서대로 무엇을 해야할지 작성한다.
2. **선언형** 스타일은 실행 결과가 어떻게 되어야 할지를 작성한다.


### React가 선언형 스타일을 채택한 이유?

선언형으로 작성하면 복잡도를 줄여줄 뿐만 아니라 코드에 대한 이해도와 가독성을 높일 수 있기 때문이다.

### 예제

```javascript

// 명령형 스타일
var profile = {account: '47574416'};
var profileDeep = {account : { number : 47574416 }};

var getNestedValueImperatively = function getNestedValueImperatively(object, propertyName) {

  var currentObject = object;
  var propertyNamesList = propertyName.split('.');
  var maxNestedLevel = propertyNamesList.length;
  var currentNestedLevel;

  for (currentNestedLevel = 0; currentNestedLevel < maxNestedLevel; currentNestedLevel++) {
    if (!currentObject || typeof currentObject === 'undefined') return undefined;
    currentObject = currentObject[propertyNamesList[currentNestedLevel]];
    // propertyName으로 넘어온 인자를 쪼개, 더이상 중첩된 내부가 없을 때까지 for문으로 돌면서 값을 가져온다. 
  }

  return currentObject
}

console.log(getNestedValueImperatively(profile, 'account') === '47574416')  // output: true
console.log(getNestedValueImperatively(profileDeep, 'account.number') === 47574416); // output: true
```

```javascript
// 선언형 스타일(결괏값에 좀 더 집중, 지역 변수가 줄어들고 논리도 단순해졌다.)
var getValue = function getValue(object, propertyName) {
  return typeof object === 'undefined' ? undefined : object[propertyName]
}

var getNestedValueDeclaratively = function getNestedValueDeclaratively(object, propertyName) {
  return propertyName.split('.').reduce(getValue, object)
}
console.log(getNestedValueDeclaratively({bar: 'baz'}, 'bar') === 'baz'); // true
console.log(getNestedValueDeclaratively({bar: { baz: 1 }}, 'bar.baz') === 1); // true

```

선언형 스타일의 경우 javascript의 내장 배열 메서드를 많이 사용하는 것 같다.

아무래도 확실히 결괏값을 도출해내는 일련의 과정들을 이미 정리해놓은 느낌이라서 일반 for문을 사용하는 것보다 더 그 용도가 정확하게 드러나기 때문이 아닐까 싶다. '이 친구는 이 일만 해!' 라고 그 용도를 정해놓았으니?

reduce 메서드를 정리하면서 다시 상단의 코드를 하나하나 파헤쳐 보았다. 
이 함수를 먼저 보자.
``` javascript
var getNestedValueDeclaratively = function getNestedValueDeclaratively(object, propertyName) {
  return propertyName.split('.').reduce(getValue, object)
}
```
`return propertyName.split('.').reduce(getValue, object)`에 인자를 하나씩 대입해보면서 풀어보자.

  1. `split` 먼저 해보자. `propertyName.split('.') = ['bar', 'baz']`
  2. 그러면 `['bar', 'baz'].reduce(getValue, object)`가 된다. 여기서 `reduce` 메서드의 두번째 인자인 `object`는 **초깃값**으로, 이 값을 설정할 경우에는 콜백 함수를 처음 호출할 때 첫번째 인자로 넘어가게 된다. 초깃값이 없으면 배열의 첫 번째 원소를 사용한다. 여기서 설정한 초깃값 `object`는 실행문의 인자로 넘어온 `{bar: { baz: 1 }`이다. 
  3. `['bar', 'baz'].reduce(callback, {bar: { baz: 1 })`가 된다. 여기서 `reduce` 메서드의 첫 번째 인자인 콜백함수 `getValue`를 살펴보자.
  4. `getValue` 함수를 인자에 그대로 넣어보면 이렇게 생겼다.
  ``` javascript
  ['bar', 'baz'].reduce(function getValue(object, propertyName) {
    return typeof object === 'undefined' ? undefined : object[propertyName]
    }, {bar: { baz: 1 })
  ```

  5. 헷갈려서 `getValue`의 인자 이름을 `x`, `y`로 바꿔본다.
  ``` javascript
  ['bar', 'baz'].reduce(function getValue(x, y) {
    return typeof x === 'undefined' ? undefined : x[y]
    }, {bar: { baz: 1 })
  ```
  6. `reduce` 메서드에서 첫번째 인자인 `x`는 누적값이고, 두번째 인자인 `y`는 현재값을 의미한다.
  7. 앞서 초기값 *(2번 참고)* 으로 객체 `{bar: { baz: 1 }}`을 받았으니, 처음 `x`의 값은 `{bar: { baz: 1 }}`이고, `y`값은 `reduce` 메서드를 적용한 초기 배열의 첫번째 원소인 `'bar'`가 된다.
  8. `return` 문을 살펴보자. `x`의 값의 `type`이 `'undefined'` 일 경우 `undefined`를 반환하고, 아니면 `x[y]`를 반환한다.
  ```javascript
  return typeof x === 'undefined' ? undefined : x[y]
  ``` 
  > `typeof` 는 문자열을 반환하므로, `undefined`가 아니라 `'undefined'`를 조건문으로 해야한다.
  9. 여기서 `x`는 `{bar: { baz: 1 }}`이므로 `type`이 `undefined`가 아니다`(object)`. 그래서 `x[y]`를 실행, 이는 곧 `{bar: { baz: 1}}['bar']`와 같다. `{bar: { baz: 1 }}` 객체에서 `"bar"` 프로퍼티의 값을 가져오는 것이다. `-> return 값: {baz: 1}`
  ```javascript
  function getValue({bar: { baz: 1 }}, 'bar') {
    return typeof {bar: { baz: 1 }} === 'undefined' ? undefined : {bar: { baz: 1 }}['bar'] 
    // return {baz: 1}
  }
  ``` 
  10. 아직 계산하지 않은 배열의 원소가 남았으므로 도출된 `return` 값을 가지고 다시 `getValue` 함수를 실행한다.
  11. 이제 `x`는 전 단계에서 `return`한 값인 `{baz: 1}`이다(`reduce` 메서드 콜백함수의 첫번째 인자는 누적된 값을 가리킨다). `y`는 `reduce` 메서드를 적용한 초기 배열(`['bar', 'baz']`)의 1번째 원소인 `'baz'`이다.
  12. 이번 `x`의 `type`도 `'undefined'`가 아닌 `'object'` 이므로, `x[y]`를 실행한다. 이는 곧 `{baz: 1}["baz"]`와 같다. 
  13. `{baz: 1}` 객체의 `"baz"` 프로퍼티 값을 가져와서 `return`한다. `-> return 값: 1`
  ```javascript
  function getValue({baz: 1}, 'baz') {
    return typeof {baz: 1} === 'undefined' ? undefined : {baz: 1}['baz']
    // return 1
  }
  ```
  14. 더 이상 가져올 원소가 없으므로 실행을 종료한다. `-> 최종 return 값: 1`

이러한 선언형 스타일은 React에서 UI를 구성할 때 사용되며, 뷰에 변경이 발생하게 되면 가상DOM을 사용하여 차이점을 찾아내고, React가 알아서 뷰를 갱신한다. 이것을 **내부상태(inner state)** 변화라고 부른다.

---

나만 해도 보통은 명령형 프로그래밍에 익숙하고, 지금까지 그렇게 코드를 짜왔다. 어느 상황에서든 재사용이 가능한 모듈화된 코드를 만들기 위해서는 **선언형** 프로그래밍 하는 법을 익혀야겠다. 확실히 가독성도 좋고(물론 각종 내장 메서드들에 익숙하다는 전제하에)


