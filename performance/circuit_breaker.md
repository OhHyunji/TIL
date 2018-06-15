# Circuit-Breaker

finagle 안에 여러가지 모듈들이 있다. 

 - LB, CircuitBreaker 등등
 - on/off 할 수 있다.

## close → open → half-open 반복

- 원래는 close 상태.
- 에러가 발생하면 open 상태가 된다.
- open 상태일 때는 요청 보내봐야 에러발생하니까, 알아서 요청 안보내고 fail-fast 처리한다.
- open 상태일 때 half-open 해서 아직도 에러나고있는지? 확인한다. 
- 그러다가 이제 에러 안나면 open → close 상태가 된다.

finagle 외에 akka, hystrix(java), netty 에도 비슷한 모듈이 있다. 

## Example. 분산서비스 환경에 대한 Circuit Breaker 적용

원문: [분산서비스 환경에 대한 Circuit Breaker 적용](https://engineering.linecorp.com/ko/blog/detail/76)

### Circuit Breaker란?

- 최근의 web, app 벡엔드 서버 시스템은 여러개의 서비스가 api, rpc로 연결된 네트워크로 구성되어있다.
- 만약 이 네트워크 중 하나가 갑자기 고장나면?
- 동작하지 않는 서비스 접속시 타임아웃될 때까지 차단되어, 의존성이 있는 서비스들이 연쇄적으로 멈출 수 있다..!!

우리는 이런 상황을 방지하기 위해 **장애가 발생한 서비스에 대한 접속차단**이 필요하다.

이 시스템을 자동화한 것이 **Circuit Breaker**다.

- 원격 접속의 성공/실패를 카운트하여 에러율(faliure rate)이 임계치를 넘어서면 자동적으로 접속을 차단하는 시스템이다.

각각의 상태와 이동조건은 아래와 같다.

#### Closed

초기상태. 모든 접속은 평소와 같이 실행된다.

#### Open

에러율이 임계치를 넘어서면 open상태가 된다. 모든 접속은 차단(fail fast)된다.

#### HalfOpen

Open 후 일정시간이 지나면 HalfOpen상태가 된다. 

- 접속을 시도해서 성공하면 Closed,
- 실패하면 Open으로 되돌아간다.




