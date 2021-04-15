![Problem Image](https://raw.githubusercontent.com/hitari/scratch-paper/main/Algorithm-study/Hackerrank/interview-preparation-kit/Warm-up-challenges/Counting-valleys/Problem.png "문제지")

## 코드

```javascript
function jumpingOnClouds(c) {
  let jump = 0;
  for (let i = 0; i < c.length; i++) {
    if (c[i] === 0) {
      if (c[i + 2] === 0) {
        i++;
        jump++;
      } else if (c[i + 1] === 0) {
        jump++;
      }
    }
  }
  return jump;
}
```

출처: https://www.hackerrank.com \[[Counting Valleys](https://www.hackerrank.com/challenges/counting-valleys/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=warmup)\]
