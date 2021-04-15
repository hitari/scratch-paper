![Problem Image](https://raw.githubusercontent.com/hitari/scratch-paper/main/Algorithm-study/Hackerrank/interview-preparation-kit/Warm-up-challenges/Sales-by-match/Problem.png "문제지")

## 코드 #1

```javascript
function sockMerchant(n, ar) {
  let result = 0;
  let arr = [];

  for (let i = 0; i < n; i++) {
    arr[ar[i]] = arr[ar[i]] ? arr[ar[i]] + 1 : 1;
  }

  for (let i = 0; i < arr.length; i++) {
    if (arr[i] > 0) {
      result += Math.floor(arr[i] / 2);
    }
  }

  return result;
}
```

## 코드 #2

```javascript
function sockMerchant(n, ar) {
  let result = 0;
  const pairs = new Set();

  for (let i = 0; i < n; i++) {
    if (pairs.has(ar[i])) {
      result++;
      pairs.delete(ar[i]);
    } else {
      pairs.add(ar[i]);
    }
  }

  return result;
}
```

출처: https://www.hackerrank.com \[[Sales by Match](https://www.hackerrank.com/challenges/sock-merchant/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=warmup)\]
