# LINE Developers Meetup: Server tech

## Armeria를 이용한 비동기 마이크로서비스 개발, 이희승

Armeria는 Java, Netty, HTTP/2, RPC, Thrift에 기반한 LINE의 오픈소스 리액티브 비동기 마이크로 서비스 프레임워크다.

### Why Armeria

비동기여야 하는 이유?

- 샤딩을 4개로 해놔도 한개 뻑나면 밀릴 수 밖에 없다. (요청하는 스레드가 쌓일 수 밖에 없다.)
- 그래서 응답 안기다려도 되게, 비동기가 필요하다!

Reactive Stream

- 10만개의 레코드를 한방에 읽어와야한다면? 
- 버퍼 두고 처리한다고해도 순간적으로 메모리에 부담이 간다. 

기존의 웹 프레임워크들은 RPC 지원이 미흡하다.

- 특히 디버깅 콘솔에 대한 갈증이 엄청나다.

> 우리 팀에서는 thrift를 사용하고있는데, 디버깅콘솔의 필요성! 완전공감했다..!ㅠㅠ

### Armeria 소개1. REST

#### http&https, http1&http2 전부 다 지원

```java
.https(8443)
```

- https 인증서 필요한데, self-sign 인증서 설정할 수 있다.
- 기존 레거시들도 쉽게 붙일 수 있도록 netty, jetty, tomcat 등도 지원한다.
- tomcat은 http2를 지원하지 않지만 Armeria를 사용하면 tomcat을 붙이고 http2도 쓸 수 있다..!

#### future를 사용해서 비동기를 쉽게 처리

```java
future
	.thenApply(...)
	.exceptionally(...)
```

#### 10만건! 1000개 단위로 비동기로 처리할 떄

```java
.onDemand(() -> stremNunbers(res, nextStart))
```

- 클라이언트가 어느정도 처리하고 나면 onDemand에 넘겨준 callback을 처리한다.
- 클라이언트가 어느정도 처리하지도 않았는데 다음 순서 하라고 해 봐야, 어차피 바빠서 못하니까.

### Armeria 소개2. RPC

#### TText

- 기존 thrift의 JSON파일은 사람이 알아보기 어렵다.
- 이걸 사람이 읽을 수 있게 변환된 파일이 TText다. Apache Thrift는 제공하고있지 않고, finagle에서만 제공하고있다.
- Armeria는 제공한다.

#### Decorator

- 요청 시작전/중간/끝나고 intercepting 할 수 있다.
	- 요청 시작전마다 Params Validation을 한다던지,
	- Request Params를 로그에 찍는다던지, 등등
- 기존에는 access.log에는 200,OK만 남아서 application.log에 위 내용들(예: Request에 대한 Params) 일일이 남기게 작성해야했다.
- Armeria에서 다양한 데코레이터들을 지원하고 있기 때문에, 서비스는 핵심적인 부분만 구현하고 이런 공통적인 부분은 데코레이터가 처리하게 하면 된다.

> 이것은 finagle에서 사용하고있던 기능들 + 우리 셀에서 쓰고있는 durian 기능들이랑 비슷한것같다.

#### Etc.

- zipkin tracer 지원
- 디버깅 콘솔 지원(RPC용 포스트맨)
- Prometheus

> 네티아버지 궁금했는데 실제로 봐서 인상적이었다. 조근조근한 말투였지만 재치있으셔서 발표가 지루하지 않았고, 쉽게 설명해주셔서 완전 몰입해서 봤다. 
> 
> 최근에 팀에서 이야기하고있는 개념들이 많이 나와서 이해하는데 많은 도움이 되었다.
> 아쉬웠던 점은 팀에서 이야기할 때 "찾아봐야징ㅋ"에 쌓아두고 미뤄둔 부분들이 오늘 고스란히.. 돌아와서ㅠ 질문하는 사람들의 내용을 100% 이해하기 어려운 부분이 있었다. 
>
> 이런 서버 구조, 개념 탄탄히 공부해서 다시 들으면 훠어얼씬 재미있게 들을 것 같다.

## Redis at LINE

LINE에서는 메시징 서비스를 위한 Storage system으로 Redis, HBase, Kafka를 사용하고있다. 
