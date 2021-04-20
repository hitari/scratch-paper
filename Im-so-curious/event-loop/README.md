javascript 궁금한 부분 파고들기 하다보면 계속 도달하게 되는 부분이 Event Loop다.  
그런데, 조금 더 파고들다보니 내가 알고 있던 Event Loop가 부족한 단편적인 지식이라는걸 알게 되었다.  
요것도 파고 들기 위해 자료를 모아 놓자. 왜냐?! 이렇게 중간에 막히는거 중간중간 콜스택 처럼 처럼 공부하니 이전에 공부하던걸 잊어먹거나 흐름을 놓치게되서 자꾸 다시 공부하게 된다... 일단 하던거 하면서 해야겠다.


(function() {
function delayFn(){
    console.log('this is a dealyFn');
    setTimeout(function cb1() {
        console.log('this is a msg from call setTimeout');
  }, 0);
}

  console.log('this is the start');

  setTimeout(function cb() {
    console.log('this is a msg from call back');
  });

  console.log('this is just a message');
  delayFn();

  setTimeout(function cb1() {
    console.log('this is a msg from call back1');
  }, 0);

  console.log('this is the end');

})();


```
- https://developer.mozilla.org/ko/docs/Web/JavaScript/EventLoop
- https://meetup.toast.com/posts/89
- https://fireburger.tistory.com/3
- https://developer.mozilla.org/ko/docs/Web/API
- https://evan-moon.github.io/2019/08/01/nodejs-event-loop-workflow/#%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84%EB%8A%94-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%97%94%EC%A7%84-%EB%82%B4%EB%B6%80%EC%97%90-%EC%9E%88%EB%8B%A4
```