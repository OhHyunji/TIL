# 성능테스트 FAQ

> 엄교수님이 주신 가르침을 잊지 않기위해 정리해보았다.

### response time이 빠르면 tps가 높다? NO

하나의 요청이 시스템 리소스를 얼마나 사용하느냐에 따라 달라진다.

- request 하나가 100ms의 응답을 주고 cpu를 100% 쓰면 => 10tps가 나오는데,
- request 하나가 1s 응답을 주고 cpu를 1% 쓰면 => 100tps가 나올 수 있다.


### 서버를 늘리면 tps가 늘어난다? NO

서버를 늘린다고 늘어나는 경우도 있다. 그 서버가 bottle neck 지점인 경우. 

그게 아니라면 서버 늘린다고 tps가 늘어나지는 않는다.

### 성능테스트시 확인하면 좋은 부분

CPU

- ssl을 사용하는 서버라면 암호화/복호화 떄문에 많은 CPU를 사용한다. 
- 500tps를 처리하면 cpu 1개 코어를 full로 쓴다. (nginx 기준)

Network

- 데이터 전달량이 많다면 bandwidth를 확인해보면 좋다.

IO

- 우리팀 서비스의 스타일에서는 발생하기 쉽지 않지만, 로그를 많이 쌓는다면 확인해보면 좋다.

Memory

- JVM의 GC가 계속 일어난다면 높은 성능을 낼 수 없다.

외부API

- 대부분 API 성능이 안나오면 자체성능보다 외부의 연동포인트일 경우가 많다.

DB

- connection pool을 다 사용하거나, slow query가 많이 발생하면 DB가 bottle neck이 된다.
- active connection이 max connection이면 늘려야한다.

### Rule(중요!!)

- 내부저장소, redis, arcus, MySql과 통신하는 WAS이면 **서버 한대에 적어도 300tps**는 나와야한다.
(그 속도가 안나오면 내가 잘못만든거다.)
- 외부와 연동하는 API에 대해 성능테스트할 때, 우리의 트래픽을 그쪽으로 흘리면 안된다.