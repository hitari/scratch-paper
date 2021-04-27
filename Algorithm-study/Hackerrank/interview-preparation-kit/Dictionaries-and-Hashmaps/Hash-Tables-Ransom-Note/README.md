![Problem Image](https://raw.githubusercontent.com/hitari/scratch-paper/main/Algorithm-study/Hackerrank/interview-preparation-kit/Dictionaries-and-Hashmaps/Hash-Tables-Ransom-Note/Problem.png "문제지")

## 코드

```javascript
function checkMagazine(magazine, note) {
  const map = {};
  let result = true;

  for (let i = 0; i < magazine.length; i++) {
    map[magazine[i]] = (map[magazine[i]] || 0) + 1;
  }

  for (let j = 0; j < note.length; j++) {
    map[note[j]] = (map[note[j]] || 0) - 1;
  }

  for (const item in map) {
    if (map[item] < 0) {
      result = false;
      break;
    }
  }

  console.log(result ? "Yes" : "No");
}
```

출처: https://www.hackerrank.com \[[Hash Tables: Ransom Note](https://www.hackerrank.com/challenges/ctci-ransom-note/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=dictionaries-hashmaps)\]
