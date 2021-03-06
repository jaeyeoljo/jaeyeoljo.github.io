# [Algorithm] 선형 보간법 (Linear interpolation)

Range 타입의 Input 을 다루다 난감한 상황이 발생했다. 드래그 가능한 2개의 포인트가 존재하는 range input인데 클릭이나 드래그가 잘 되다가 페이지를 새로고침하면 해당 값을 인식못하는 현상이 발생했다. 원인은 html에 고정된 style 위치값으로 인해 실제 사용자가 이동시킨 값과의 좌표차이에서 발생한 문제였다. 

해결은 고정된 style의 위치값을 동적으로 부여해서 실제 사용자의 값을 트래킹하도록 수정했다.

예를들어 고정된 RANGE위치값은 0%부터 100%까지인데 2개의 포인트값은 -100%부터 200%까지를 가리키게 만들어야 하는 경우다.

즉 -100%부터 200%의 포인트위치값을 0%부터 100%까지 존재하는 RANGE위치값과 매칭을 시켜야한다. 언뜻보면 쉬워보이지만 값들이 고정되어 있지 않고 조건이 바뀐다면 그냥 만들어지지 않는다. 특히 사용자가 드래그할 영역은 0에서 100까지가 아니라 -100에서 200까지였다.

찾아보니 선형 보간법을 사용하면 될 듯 하여 공식을 구했다.

![https://kin-phinf.pstatic.net/20220720_118/1658284557355lqc7D_JPEG/1658284557339.jpg](https://kin-phinf.pstatic.net/20220720_118/1658284557355lqc7D_JPEG/1658284557339.jpg)

<aside>
💡 A와 B의 관계가 시작~끝 안에서 전부 선형이냐 하는 점이 중요하다. 쉽게 설명하면 X축에 A, Y축에 B 를 넣고 테이블에 있는 점들을 그렸을때 한 직선 안에 데이터가 모두 들어가는지, 아닌지에 따라 달라진다.

예를들어 A가 {-100, -50, 0, 50, 100} 에서 {-100, -50, 0, 50, 200} 으로 바뀌었다. 그런데 B의 값은 현재와 동일하다고 하면, A가 -100~50 까지의 그래프 기울기와 50~200 까지의 그래프 기울기가 달라질 것이다.

아래 그림에서 파란색 실선이 현재의 데이터이다. 빨간색 실선과 같이 A, B 를 찍어보면 중간에 그래프 기울기가 바뀌는 부분이 있다면 선형 보간법에 해당하는 알고리즘을 사용해야 한다. 간단하게 말해 각 데이터 사이사이마다 직선을 그어서 그 안에서 함수값을 찾아야 한다는 뜻이다.

하지만 그림의 점선처럼 A의 최대값이 바뀌면서 A 중간 데이터들도 같이 바뀌어서 전체적으로 직선이 된다고 하면 좀 쉽게 수식을 유도할 수 있다.

즉, A = (-100, A_MAX) 와 B = (0, 100) 만 가지고 중간값을 찾는 문제로 바뀌어서,

B = 100 * (A+100) / (A_MAX+100) 으로 공식을 구할 수 있다.

</aside>

- 알고리즘을 적용한 공식

```html
RANGE위치값 = 100 * (사용자가 움직인 최종 포인트값+100) / (포인트 최대값+100)
```

선형 보간법 알고리즘을 사용해 공식을 적용하니 새로고침 시에도 위치를 놓치지 않고 드래깅도 잘된다. 

요즘은 어떨지 몰라도 사실 CRUD 작업만 하던 과거 백엔드 개발자들과 학원에서 양산형으로 찍어내는 퍼블리싱 작업만 하는 프론트엔드 개발자들은 이러한 알고리즘과 문제해결에 능숙하지 못하다. 특히 개발자 붐으로 인해  문과에서 이과로 넘어온 기초가 부실한 시중의 개발자들은 더할 것이다. 

한가지 팁을 주자면 내가 천재일 필요는 없다. 세상엔 천재들이 잘 만들어놓은 것들이 많으니깐 그걸 가져다 적절히 변형하여 잘 사용하기만해도 중간 이상은 간다. 하지만 그걸 잘하기 위해서는 경험에서 얻을수 있는 ‘짬바이브’와 어느정도의 실력도 겸비해야하는 건 부정할 수 없는 사실이다.