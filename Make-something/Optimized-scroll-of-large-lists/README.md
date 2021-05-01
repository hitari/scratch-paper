# 대량 리스트 최적화 스크롤

최근에 작업하는 영역에서 초기 로딩 하는데 문제가 있는 페이지가 있어 최적화 작업을 하려고한다. 그 페이지는 기본적으로 이미지나 DOM이 많은 페이지인데, 문제되는 곳이 최근에 선택 옵션이 과도하게 많은 케이스들이 발견 되었고 또 앞으로 더 많은 옵션이 들어가는 상황이 생겨서 옵션 목록이 제대로 안뜨거나 DOM 로드가 오래걸림 혹은 버벅거리는 문제들이 발생 되었다. 그래서 몇가지 아이디어를 스케치 한다음에 최적화 될수 있는 기능을 연구 하고 구현해 보고자 한다.

첫번째, 요즘 가장 많이 사용한다는 IntersectionObserver로 작업해 보았다.

1. IntersectionObserver를 기반으로 한 Infinite scroll 구현

IntersectionObserver로 구현을 하면은 getBoundingClientRect, offsetTop에서 발생되는 reflow 현상도 일어나지 않고 최적화된 기능을 구현할수 있다.

### HTML

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Parcel Sandbox</title>
    <meta charset="UTF-8" />
  </head>

  <body>
    <div id="app">
      <div class="scroll">
        <div class="wrap">
          <ul id="ul"></ul>
          <!-- <p class="end"></p> -->
        </div>
      </div>
    </div>
    <script src="src/scroll.js"></script>
  </body>
</html>
```

### CSS

```css
.scroll {
  width: 150px;
  height: 216px;
  border: 1px solid #000;
  overflow-y: scroll;
  overflow-x: hidden;
}

ul {
  list-style: none;
  margin: 0;
  padding-left: 0;
}

li {
  min-height: 29px;
  border-bottom: 1px solid #222;
  text-indent: 10px;
}
```

### JS

```javascript
const arr = [];
const $scroll = document.querySelector(".scroll");
const $ul = document.querySelector("ul");

// DATA Setting
for (let i = 0; i < 2000; i++) {
  arr.push({
    title: `Title${i + 1}`,
  });
}

// Infinite scroll
let currentPage = 0;
const DATA_PER_PAGE = 10;
const lastPage = arr.length / DATA_PER_PAGE;

const addData = (currentPage) => {
  if (currentPage > lastPage) return console.log("end");

  const data = arr.slice(
    (currentPage - 1) * DATA_PER_PAGE,
    currentPage * DATA_PER_PAGE
  );

  data.map((item) => {
    const li = document.createElement("li");
    li.textContent = `${item.title}`;
    $ul.appendChild(li);
  });

  intersectionObserver.observe(
    document.querySelectorAll("ul li:last-child")[0]
  );
};

const intersectionObserver = new IntersectionObserver(
  (entries, observer) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        addData(++currentPage);
        observer.unobserve(entry.target);
      }
    });
  },
  {
    root: $scroll,
  }
);

addData(++currentPage);
```

![Slow Scroll Image](https://raw.githubusercontent.com/hitari/scratch-paper/main/Make-something/Optimized-scroll-of-large-lists/slowScroll.gif "느린 스크롤 GIF")

하지만, 2000개의 item 이 있고 10개씩 뿌려준다면 최하단 element를 만나야 다음 10개 출력 되기때문에 기존 한번에 Dom이 있을때 처럼 스크롤을 한번 많이 내려서 중간쯤으로 이동한다던가 바로 끝으로 이동한다던가 해서 빠르게 이동하는 것을 할 수가 없게 되었다. 그래서 기존 scroll Event 방식으로 scroll과 해당 element의 좌표값을 기준으로 해서 작업을 해 보았다.

HTML, CSS는 위와 동일하다.

### JS

```javascript
const arr = [];
const $scroll = document.querySelector(".scroll");
const $wrap = document.querySelector(".wrap");
const $ul = document.querySelector("ul");

const MAX_DATA_COUNT = 2000; // 총 개수
const DATA_PER_PAGE = 10; // 페이지당 개수

const listMap = new Map();

let currentPageNum = 1;

for (let i = 0; i < MAX_DATA_COUNT; i++) {
  arr.push({
    title: `Title${i + 1}`,
  });
}

// wrap 최소 높이값 설정
function setMinHeight() {
  const $lastLi = document.querySelector("ul li:last-child");
  const lastLiHight =
    $lastLi.clientHeight + heightWithBorderBommtomWidth($lastLi, 3);
  $wrap.style.minHeight = `${MAX_DATA_COUNT * lastLiHight}px`;
}

// 마지막 li가 scroll 영역에 들어온지 체크
const isCameLi = () => {
  const $lastLi = document.querySelector("ul li:last-child");

  return (
    $scroll.getBoundingClientRect().top + $scroll.clientHeight >
    $lastLi.getBoundingClientRect().top
  );
};

// li를 화면에 그려준다.
const render = (pageNum) => {
  const list = arr.slice(
    (pageNum - 1) * DATA_PER_PAGE,
    pageNum * DATA_PER_PAGE
  );

  list.forEach((item) => {
    const li = document.createElement("li");
    li.textContent = item.title;
    $ul.appendChild(li);
  });
};

// element의 border Bootom Width 과 Clientheight 의 합친 값을 받는다.
// offsetHeight로 처리할 경우 1px 맞지 않는 케이스 발생
const heightWithBorderBommtomWidth = (el, num = 0) => {
  // element 인지 체크
  // https://hianna.tistory.com/412
  if (!el.nodeType) return;
  const borderBottomWidth = window.getComputedStyle(el).borderBottomWidth;
  let rerult = parseFloat(
    borderBottomWidth.substring(0, borderBottomWidth.length - 2)
  );
  return num > 0 ? parseFloat(rerult.toFixed(num)) : rerult;
};

// 스크롤된 현재 위치가 몇번째 페이지 인지 계산해준다.
const getPageNum = () => {
  const $lastLi = document.querySelector("ul li:last-child");
  const scrollHeight =
    $scroll.getBoundingClientRect().top +
    heightWithBorderBommtomWidth($scroll, 2) -
    $wrap.getBoundingClientRect().top.toFixed(2);
  const lastLiHeight =
    $lastLi.clientHeight + heightWithBorderBommtomWidth($lastLi, 2);
  const countLi = scrollHeight / lastLiHeight;
  const pageNum = Math.ceil(countLi / DATA_PER_PAGE) + 1;

  return pageNum;
};

// 스크롤 될때 이벤트 발생
const onScroll = (e) => {
  if (!isCameLi()) return;
  const pageNum = getPageNum();

  for (; currentPageNum < pageNum; ) {
    if (listMap.has(pageNum)) return;
    currentPageNum += 1;
    listMap.set(currentPageNum);
    render(currentPageNum);
  }
};

// 스크롤 이벤트
document.querySelector(".scroll").addEventListener("scroll", onScroll);

// 초기 실행
listMap.set(currentPageNum);
render(currentPageNum);
setMinHeight();
```

![Fast Scroll Image](https://raw.githubusercontent.com/hitari/scratch-paper/main/Make-something/Optimized-scroll-of-large-lists/fastScroll.gif "빠른 스크롤 GIF")

화면 그려질때 2000개의 돔을 바로 그려준 것처럼 빠르게 마지막 까지 이동할 수 있다. 그리고 이동 시점의 페이지를 계산해서 인피니티 스크롤 처럼 페이지 단위로 그려주기 때문에 초기 랜더 부하도 많이 줄여준다.

[테스트 페이지로 이동](https://codesandbox.io/s/youthful-river-hd46t?file=/src/styles.css:0-36)
