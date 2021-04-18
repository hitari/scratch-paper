# forEach는 왜 순차처리가 되지 않는 것인가?

회사 레거시 소스 중에서 $.each로 되어있는 부분에서 callback 부분에 Promise로 반환하면 async awiat구문으로 받을 수 있을 거라고 생각 했었는데, 이게 왠일 await가 되지 않고 비동기적으로 실행 되는 것이었다.  

대략, 구성하자면 아래와 같은 구문 실행 할 경우 1초에 console을 1번씩 찍을 거 같지만 결과는 1234가 순식간에 뿌려지게 된다. 이게 왜 그러냐 하면,

``` javascript
[1, 2, 3, 4].forEach(async (i) => {
  await new Promise((resolve) =>
    setTimeout(() => {
      console.log(i);
      resolve(i);
    }, 1000)
  );
});
```

### 참조
```
- https://velog.io/@hanameee/%EB%B0%B0%EC%97%B4%EC%97%90-%EB%B9%84%EB%8F%99%EA%B8%B0-%EC%9E%91%EC%97%85%EC%9D%84-%EC%8B%A4%EC%8B%9C%ED%95%A0-%EB%95%8C-%EC%95%8C%EC%95%84%EB%91%90%EB%A9%B4-%EC%A2%8B%EC%9D%84%EB%B2%95%ED%95%9C-%EC%9D%B4%EC%95%BC%EA%B8%B0%EB%93%A4
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for...of
- https://woowacourse.github.io/javable/post/2020-09-01-loop-async/
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/await
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/then
- https://engineering.linecorp.com/ko/blog/dont-block-the-event-loop/
```
