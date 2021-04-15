![Problem Image](https://raw.githubusercontent.com/hitari/scratch-paper/main/Algorithm-study/Hackerrank/interview-preparation-kit/Warm-up-challenges/Sales-by-match/Problem.png "문제지")

## 코드

```javascript
function repeatedString(s, n) {
  const len = n % s.length;
  let count = 0;
  let lastCount = 0;

  for (let i = 0; i < s.length; i++) {
    if (i < len) {
      lastCount += s[i] === "a" ? 1 : 0;
    }
    count += s[i] === "a" ? 1 : 0;
  }

  return Math.floor(n / s.length) * count + lastCount;
}
```

출처: https://www.hackerrank.com \[[Sales by Match](https://www.hackerrank.com/challenges/sock-merchant/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=warmup)\]
