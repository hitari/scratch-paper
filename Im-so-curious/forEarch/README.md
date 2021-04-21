# forEach는 왜 순차처리가 되지 않는 것인가?

회사 레거시 소스 중에서 forEach 되어있는 부분에서 callback으로 This 배열에 key 값에 맞는 Ajax 호출 부분이 있는데, 이부분이 비동기적으로 돌아가서 추가 로직에서 특정 key에만 동기적으로 적용하기 위해 callback에서 Promise로 반환 받아 async awiat구문으로 받을 수 있을 거라고 생각 했었는데, 이게 왠일 await가 동기적 형태로 작동 되지 않고 비동기적으로 실행 되는 것이었다.  

유사한 형태로 구성하자면 아래와 같다. 아래 구문 실행 할 경우 1초에 console을 1번씩 찍을 거 같지만 결과는 1234가 순식간에 뿌려지게 된다.
``` javascript
[1, 2, 3, 4].forEach(async (i) => {
  await new Promise((resolve) =>
    setTimeout(() => {
      console.log(i);
      resolve(i);
    }, 1000)
  );
});

// 1
// 2
// 3
// 4
```

왜? 그럴까... 고민을 해보았다. 분명 forEach는 순차적으로 되어있고 비동기라면 async await로 처리되어 있는데 그래서 구글링하여서 몇몇 자료를 찾아보니 forEach 내부는 callback 구조로 되어있어서 그렇다고 설명이 되어있었다. 그래서 과연 그게 맞는 것인가 해서 ECMA 262스펙문서를 확인하니 아래와 같이 설명 되어 있었다.

```
1. Let O be ? ToObject(this value).
2. Let len be ? LengthOfArrayLike(O).
3. If IsCallable(callbackfn) is false, throw a TypeError exception.
4. Let k be 0.
5. Repeat, while k < len,
a. Let Pk be ! ToString(𝔽(k)).
b. Let kPresent be ? HasProperty(O, Pk).
c. If kPresent is true, then
i. Let kValue be ? Get(O, Pk).
ii. Perform ? Call(callbackfn, thisArg, « kValue, 𝔽(k), O »).
d. Set k to k + 1.
6. Return undefined.
```
  
저 문서를 읽고 v8엔진 forEach 소스를 확인해 보니, v8엔진에 있는 주석 설명과 동일했다.
그래서 아래에는 v8 엔진 소스에서 해당 부분만 옮겨 보았다.

```
// https://tc39.github.io/ecma262/#sec-array.prototype.foreach
transitioning javascript builtin
ArrayForEach(
    js-implicit context: NativeContext, receiver: JSAny)(...arguments): JSAny {
  try {
    RequireObjectCoercible(receiver, 'Array.prototype.forEach');

    // 1. Let O be ? ToObject(this value).
    const o: JSReceiver = ToObject_Inline(context, receiver);

    // 2. Let len be ? ToLength(? Get(O, "length")).
    const len: Number = GetLengthProperty(o);

    // 3. If IsCallable(callbackfn) is false, throw a TypeError exception.
    if (arguments.length == 0) {
      goto TypeError;
    }
    const callbackfn = Cast<Callable>(arguments[0]) otherwise TypeError;

    // 4. If thisArg is present, let T be thisArg; else let T be undefined.
    const thisArg: JSAny = arguments[1];

    // Special cases.
    let k: Number = 0;
    try {
      return FastArrayForEach(o, len, callbackfn, thisArg)
          otherwise Bailout;
    } label Bailout(kValue: Smi) deferred {
      k = kValue;
    }

    return ArrayForEachLoopContinuation(
        o, callbackfn, thisArg, Undefined, o, k, len, Undefined);
  } label TypeError deferred {
    ThrowTypeError(MessageTemplate::kCalledNonCallable, arguments[0]);
  }
}

transitioning builtin ArrayForEachLoopContinuation(implicit context: Context)(
    _receiver: JSReceiver, callbackfn: Callable, thisArg: JSAny, _array: JSAny,
    o: JSReceiver, initialK: Number, len: Number, _to: JSAny): JSAny {
  // variables {array} and {to} are ignored.

  // 5. Let k be 0.
  // 6. Repeat, while k < len
  for (let k: Number = initialK; k < len; k = k + 1) {
    // 6a. Let Pk be ! ToString(k).
    // k is guaranteed to be a positive integer, hence ToString is
    // side-effect free and HasProperty/GetProperty do the conversion inline.

    // 6b. Let kPresent be ? HasProperty(O, Pk).
    const kPresent: Boolean = HasProperty_Inline(o, k);

    // 6c. If kPresent is true, then
    if (kPresent == True) {
      // 6c. i. Let kValue be ? Get(O, Pk).
      const kValue: JSAny = GetProperty(o, k);

      // 6c. ii. Perform ? Call(callbackfn, T, <kValue, k, O>).
      Call(context, callbackfn, thisArg, kValue, k, o);
    }

    // 6d. Increase k by 1. (done by the loop).
  }
  return Undefined;
}
```
  
  
  
promise는 Microtask 들어간다 그러면.... async await 일땐?
  
### 참조
```
- https://velog.io/@hanameee/%EB%B0%B0%EC%97%B4%EC%97%90-%EB%B9%84%EB%8F%99%EA%B8%B0-%EC%9E%91%EC%97%85%EC%9D%84-%EC%8B%A4%EC%8B%9C%ED%95%A0-%EB%95%8C-%EC%95%8C%EC%95%84%EB%91%90%EB%A9%B4-%EC%A2%8B%EC%9D%84%EB%B2%95%ED%95%9C-%EC%9D%B4%EC%95%BC%EA%B8%B0%EB%93%A4
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for...of
- https://woowacourse.github.io/javable/post/2020-09-01-loop-async/
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/await
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/then
- https://engineering.linecorp.com/ko/blog/dont-block-the-event-loop/

// v8 엔진 -> forEach
- https://github.com/v8/v8/blob/master/src/builtins/array-foreach.tq
// ECMA
- https://tc39.es/ecma262/#sec-array.prototype.foreach
```
