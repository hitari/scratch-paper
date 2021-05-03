# 대량 리스트 최적화 스크롤(2)

대량의 옵션을 처리 하는 방법에 대해 아이디어 스케치 한 것을 빠르게 개발해 보았다.  
그리고 유사 기술에 대해 검색 해 보던중 virtual scroller 라는 기법이 있는 것을 알게 되었다.

사실 최종으로 구현하려고 하던 모습과 비슷해서 놀랐고, 선조사 안하고 만들어 보자 했던 내 모습에  
살짝 바보 같음과 어짜피 더 나은거 있엇으면 이런 시행착오를 격지 않았을 것이기에 만들 고 후 조사 하자를 잘 했다는 기분이 교차하였다.  
(왠만한 기능은 라이브러리로 쉽게 구할 수 있는 시대라, 이런 시행착오 부터의 경험과 과정이 중요하기 때문에 아이디어 부터 시작해서 개발해 보는건 좋은 경험인거 같다.)

![Scroll-01 Image](https://raw.githubusercontent.com/hitari/scratch-paper/main/Make-something/Virtual-scroller/virtual-scroller-01.gif "기존방식 스크롤 GIF")

기존 Dom을 전체 나열하는 방식의 스크롤 구조

![Scroll-02 Image](https://raw.githubusercontent.com/hitari/scratch-paper/main/Make-something/Virtual-scroller/virtual-scroller-02.gif "버추얼장식 스크롤 GIF")

이것은 버추얼방식의 스크롤 구조 전체 Dom을 출력하는것이아니라 보여주는 부분 혹은 위 보이지 않는 일정 영역까지만 Dom 구조를 나타내주고 나머지는 그려주지 않는 스크롤이 내려간다면 상단은 지워주고 하단에는 새로 넣어주 면서 화면에 Dom을 한번에 그려 주는 것을 최소화 하는 방식의 최적화 기법이다.

크롬 데브 서밋 2018에서 보여준 버추얼 스크롤 영상  
[virtual-scroller: Let there be less (DOM) (Chrome Dev Summit 2018)](https://www.youtube.com/watch?v=UtD41bn6kJ0)

작성중...
