![Problem Image](https://raw.githubusercontent.com/hitari/scratch-paper/main/Algorithm-study/Hackerrank/interview-preparation-kit/Greedy-Algorithms/Minimum-Absolute-Difference-in-an-Array/Problem.png "문제지")

## 코드

```javascript
function minimumAbsoluteDifference(arr) {
  let result;
  arr.sort();
  for (let i = 0; i < arr.length - 1; i++) {
    const num = Math.abs(arr[i] - arr[i + 1]);
    if (result === undefined || result > num) result = num;
  }
  return result;
}
```

출처: https://www.hackerrank.com \[[Minimum Absolute Difference in an Array](https://www.hackerrank.com/challenges/minimum-absolute-difference-in-an-array/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=greedy-algorithms)\]
