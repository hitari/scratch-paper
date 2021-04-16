![Problem Image](https://raw.githubusercontent.com/hitari/scratch-paper/main/Algorithm-study/Hackerrank/interview-preparation-kit/String-manipulation/Alternating-characters/Problem.png "문제지")

## 코드

```javascript
function alternatingCharacters(s) {
  let result = 0;
  let cur = "";

  for (let i = 0; i < s.length; i++) {
    if (cur === s[i]) {
      result++;
    } else {
      cur = s[i];
    }
  }
  return result;
}
```

출처: https://www.hackerrank.com \[[Alternating Characters](https://www.hackerrank.com/challenges/alternating-characters/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=strings)\]
