![Problem Image](https://raw.githubusercontent.com/hitari/scratch-paper/main/Algorithm-study/Hackerrank/Arrays/2D-array-DS/Problem.png "문제지")

## 코드

```javascript
function rotLeft(a, d) {
  for (let i = 0; i < d; i++) {
    a.push(a.shift());
  }
  return a;
}
```

출처: https://www.hackerrank.com \[[Arrays: Left Rotation](https://www.hackerrank.com/challenges/ctci-array-left-rotation/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=arrays)\]
