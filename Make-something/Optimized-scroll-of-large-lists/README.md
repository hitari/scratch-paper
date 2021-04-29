# 대량 리스트 최적화 스크롤

지금 작업에서 부하가 많은 페이지인데, 선택 옵션이 과도하게 많은 케이스들이 발견 되었고 또 앞으로 더 많은 옵션이 들어가는 상황이 생겨서 옵션 목록이 제대로 안뜨거나 DOM 로드가 오래걸림 혹은 버벅거리는 문제들이 발생 되었다. 그래서 몇가지 아이디어를 스케치 한다음에 최적화 될수 있는 기능을 구현해 보고있다.

1. IntersectionObserver를 기반으로 한 Infinite scroll 구현

IntersectionObserver로 구현을 하면은 getBoundingClientRect, offsetTop에서 발생되는 reflow 현상도 일어나지 않고 최적화된 기능을 구현할수 있다.

```javascript
const arr = [];
const $scroll = document.querySelector(".scroll");
const $ul = document.querySelector("ul");

// DATA Setting
for (let i = 0; i < 1000; i++) {
  arr.push({
    title: `Title${i}`,
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

하지만, 1000개의 item 이 있고 100개씩 뿌려준다면 최하단 element를 만나야 다음 100개 출력 되기때문에 기존 한번에 Dom이 있을때 처럼 스크롤을 한번 많이 내려서 중간쯤으로 이동한다던가 바로 끝으로 이동한다던가 해서 빠르게 이동하는 것을 할 수가 없게 되었다.

그래서 기존 scroll Event 방식으로 scroll과 해당 element의 좌표값을 기준으로 해서 작업해 보려고한다.

작업중...
