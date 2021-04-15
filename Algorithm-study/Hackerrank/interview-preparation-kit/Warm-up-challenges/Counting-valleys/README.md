![Problem Image](https://raw.githubusercontent.com/hitari/scratch-paper/main/Algorithm-study/Hackerrank/interview-preparation-kit/Warm-up-challenges/Counting-valleys/Problem.png "문제지")

## 코드

```javascript
function countingValleys(steps, path) {
  let result = 0;
  let status = 0;
  for (let i = 0; i < steps; i++) {
    status += path[i] === "D" ? -1 : 1;
    if (path[i] === "U" && status === 0) result++;
  }
  return result;
}
```

출처: https://www.hackerrank.com \[[Counting Valleys](https://www.hackerrank.com/challenges/counting-valleys/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=warmup)\]
