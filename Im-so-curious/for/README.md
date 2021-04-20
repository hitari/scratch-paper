forEach에 대해서 알아보다가 V8엔진과 Event Loop 쪽 관려해서 넘어가다 문득 보게된 부분인데, 신기해서 keep 해놨다. forEach 끝나면 알아봐야겠다.

``` javascript
var s = new Date().getMilliseconds();

a =new Array();
for (var b = 0; b < 1000000; b++) {
  a[0] |= b; // 안 좋아요!
}
console.log("Ran after " + (new Date().getMilliseconds() - s) + " seconds");

s = new Date().getMilliseconds();
a =new Array();
a[0] = 0;
for (var b = 0; b < 1000000; b++) {
  a[0] |= b; // 훨씬 좋습니다. 2배 더 빨라요.
}
console.log("Ran after " + (new Date().getMilliseconds() - s) + " seconds");
````
  
정말로 속도 차이가 난다.... 왤까?
```
Ran after 5 seconds
Ran after 7 seconds
```


출처 - https://fireburger.tistory.com/3

