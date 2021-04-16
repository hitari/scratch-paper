![Problem Image](https://raw.githubusercontent.com/hitari/scratch-paper/main/Algorithm-study/Hackerrank/interview-preparation-kit/String-manipulation/Strings-making-anagrams/Problem.png "문제지")

## 코드

```javascript
function makeAnagram(a, b) {
  let result = 0;
  let num = 0;
  const arr = b.split("");

  for (let i = 0; i < a.length; i++) {
    const index = arr.indexOf(a[i]);
    if (index >= 0) {
      arr[index] = null;
      num++;
    }
  }

  result = a.length + b.length - num * 2;

  return result;
}
```

처음에 문제를 봤을때, 2중 for문 구성하거나 for문 2개로 구성 할까 잠시 생각 했다가, 겹치는 단어 개수만 알게 되면 " a,b 전체 길이 개수 - 겹치는 개수"로 구할 수 있을 거 같아 b는 String에서 배열로 변환하고 indexOf로 a[i]가 arr에 존재 하면 해당 번째는 null 처리고 겹치는 개수를 +1 해주고 위 공식을 기반으로 중복되지 않은 문자 수를 반환하였다.

출처: https://www.hackerrank.com \[[Strings: Making Anagrams](https://www.hackerrank.com/challenges/ctci-making-anagrams/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=strings)\]
