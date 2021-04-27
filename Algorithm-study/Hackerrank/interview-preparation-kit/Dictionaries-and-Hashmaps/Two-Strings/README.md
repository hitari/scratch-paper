![Problem Image](https://raw.githubusercontent.com/hitari/scratch-paper/main/Algorithm-study/Hackerrank/interview-preparation-kit/Dictionaries-and-Hashmaps/Two-Strings/Problem.png "문제지")

## 코드

```javascript
function twoStrings(s1, s2) {
  return s1.split("").find((char) => s2.includes(char)) ? "YES" : "NO";
}
```

출처: https://www.hackerrank.com \[[Two Strings](https://www.hackerrank.com/challenges/two-strings/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=dictionaries-hashmaps)\]
