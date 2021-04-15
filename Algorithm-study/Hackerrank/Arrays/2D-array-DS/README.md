![Problem Image](https://github.com/hitari/scratch-paper/blob/main/Algorithm-study/Hackerrank/Arrays/2D-array-DS/Problem.png "문제지")

## 코드

```javascript
function hourglassSum(arr) {
  const totalArr = [];
  for (let i = 0; i < arr.length - 2; i++) {
    for (let j = 0; j < arr[i].length - 2; j++) {
      let total =
        arr[i][j] +
        arr[i][j + 1] +
        arr[i][j + 2] +
        arr[i + 1][j + 1] +
        arr[i + 2][j] +
        arr[i + 2][j + 1] +
        arr[i + 2][j + 2];
      if (!isNaN(total)) totalArr.push(total);
    }
  }
  return Math.max(...totalArr);
}
```

출처: https://www.hackerrank.com \[[2D Array - DS](https://www.hackerrank.com/challenges/2d-array/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=arrays){:target="\_blank"}\]
