---
title: "[Javascript] String()과 toString()의 차이는?"
date: 2020-06-16 16:50:26 -0400
category: Javascript
---

알고리즘 문제를 풀다가 문득 궁금해져서 적어본다.

## `String()`과 `toString()`의 차이는 뭘까?

일단 둘 다 문자열로 변환해준다.
```javascript
  String(78); // '78'
  (78).toString(); // '78'
```

[reddit](https://www.reddit.com/r/javascript/comments/5m2e8j/string_vs_tostring/)에 같은 질문이 있다.

답변은 이렇다.

> The big one is for `toString()` a value has to exist. With `String()`, you can use it on values that are `undefined` or `null`. Attempting a `toString()` from an `undefined` or `null` value will produce an error. String will often call an object's `toString()` anyway, so its a safer way of doing that.

1. 가장 큰 차이점은 `toString()`의 값은 존재해야만 한다는 것이다. 
2. `String()`은 `undefined`나 `null`에도 사용할 수 있다.
3. `toString()`에 `undefined`나 `null` 값을 사용한다면 에러를 발생한다. 
4. 문자열은 종종 객체의 `toString()`을 call할 것이므로, 더 안전한 방법이다. -> 이건 무슨 말인지 잘 모르겠다.

코드로 확인해보자면

```javascript
  (null).toString(); 
  // TypeError: Cannot read property 'toString' of null

  String(null); 
  // 'null'
```

`null`은 `toString()` 메서드가 없기 때문에 property를 읽을 수 없다고 나온다.
그러나 `String()`은 잘 변환됨.


또 다른 답변.


> Everybody's getting it close but not quite right.
`String(x)` is what you want to use. It's a built-in function that's guaranteed to return a string representation of `x`.
One feature of `String(x)` is that it invokes `toString` on Objects then tries to use the return value. This is useful for making objects products more useful strings than `'[object Object]'`.
There's nothing else special about `toString`. It's entirely possible that the value of `obj.toString` is `2`, and calling `obj.toString()` will throw an error. It's also possible that obj.toString is a function that returns `2`, and your code that assumes it got a string throws an error. Or `obj` could be something that doesn't support properties, like `null` or `undefined`, and `obj.toString` throws an error.
Each of those cases can be fixed with a little bit of code. That code is in `String()`. Use it. `String(x)` is for turning `x` into a string, `toString` is for customizing `String()`, and `x.toString()` is a mistake.

### `String(x)`이야말로 본래 '문자열 변환'의 목적을 충실히 이행하고 있는 빌트인 함수이므로 이를 사용하라고 한다. 

그렇다면, 

### `toString()`은 언제?

알고리즘에서도 사용했었지만, 이진수 변환시에는 
```javascript
  (78).toString(2); // 1001110
```
이렇게 숫자에 `toString()` 메서드 사용을 통해 2진수 옵션을 주어서 변환할 수 있다.
