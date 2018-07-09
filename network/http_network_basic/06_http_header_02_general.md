# Ch6. HTTP헤더(2): General Header Fields

## Cache-Control

디렉티브를 사용해서 캐싱동작을 지정한다.

Example.

```
Cache-Control: private, max-age=0, no-cache
```

- Request/Response 디렉티브 Summary (참고: p114, p115)

#### 자세히

```
Cache-Control: public		
Cacne-Control: private		
```

- public: 공용 response
- private: 특정 유저만 대상으로 하는 response
	- 다른 유저로터 같은 request가 와도 그 캐시를 반환하지 않는다.
	
```
Cache-Control: no-cache			
```

- 캐시 안탄다. origin 서버 가서 받아온다.
- 클라이언트의 request에 사용된 경우: 중간 캐시버는 origin 서버까지 request를 전송해야한다.
- 서버의 response에 사용된 경우: 캐시서버는 리소스를 저장할 수 없다. origin 서버에 리소스 유효성 확인 필수.

```
Cache-Control: no-cache=Location
```

- no-cache 필드 값에 헤더필드명이 지정된 경우: 이 헤더필드만 캐시할 수 없다.
- 이 파라미터는 response 디렉티브만 사용할 수 있다.

```
Cache-Control: no-store
```

- 캐시로 보존 가능한 것/보존 불가능한 것을 제어한다.
- response, request에 기밀정보가 포함되어있음을 나타낸다.
- 캐시는 response, request의 일부분을 로컬스토리이제 보존하지 못하게 한다.

```
Cache-Control: max-age=604800 (단위: 초)
```

- 클라이언트의 request에 사용된 경우: 지정한 값이 0이면 캐시서버는 리퀘스트를 항상 origin 서버에 넘긴다.
- 서버의 respons에 사용된 경우: 캐시서버가 origin 서버에 유효성을 확인하지 않고 리소스를 캐시에 보존해두는 기간을 나타낸다. (캐시의 유효기간을 나타낸다.)
- 우선순위
	- HTTP/1.1: s-maxage > max-age > Expires(헤더필드)
	- HTTP/1.0: s-maxage > Expires > max-age

```
Cache-Control: s-maxage=604800 (단위: 초)
```

- `max-age`와 기능은 동일한데 `s-maxage`는 공유 캐시서버에만 적된다.
- 우선순위: s-maxage > expires, max-age (무시된다.)

```
Cache-Control: min-fresh=60 (단위: 초)
```

- 캐시된 리소스가 적어도 지정된 시간에는 최신상태의 것을 반환하도록 캐시서버에 요구한다. 
- 예를들어 60초로 설정된 경우, 캐시서버는 60초 이내에 유효기간이 끝나는 리소스를 rseponse로 반환할 수 없다.

```
Cache-Control: max-stale=3600 (단위: 초)
```

- 캐시된 리소스의 유효기간이 끝났더라도 받아들일 수 있음을 나타낸다.
- 값이 지정되어있지 않는 경우: 클라이언트는 아무리 시간이 경과했더라도 response를 받아들인다.
- 값이 지정되어있는 경우: 유효기간이 지났어도 지정 시간 이내이면 받아들인다.

```
Cache-Control: only-if-cached
```

- 클라이언트는 필요한 리소스가 로컬 캐시에 있는 경우만 response를 반환하도록 요구한다.
- 캐시서버가 로컬캐시로부터 응답할 수 없는 경우에는 `504, Gateway Timeout`을 반환한다.

```
Cache-Control: must-revalidate
```

- response의 cache가 현재도 유효한지 origin 서버에 확인하도록 한다.
- proxy가 origin서버에 도달할 수 없고, 리소스를 다시 요구할 수 없는 경우: 캐시는 클라이언트에 `504, Gateway Timeout`을 반환한다.
- 우선순위: must-revalidate > max-stale

```
Cache-Control: proxy-revalidate
```

- 모든 캐시서버에 대해서 이후 request로 해당 response를 반환할 때는 반드시 유효성을 재확인하도록 한다.

```
Cache-Control: no-transform
```

- request, response 어느 쪽에 있어서도 캐시가 entity 바디의 미디어타입을 변경하지 않는다.
- 캐시서버 등에 의해 이미지가 압축되는 것을 방지한다.

```
Cache-Control: private, community="UCI"
```

- Cache-Control 헤더필드는 cache-extention 토큰을 사용해서 디렉티브를 확장할 수 있다.
- community라는 디렉티브는 Cache-Control 헤더필드에는 없지만 cache-extension 토큰으로 추가해서 사용할 수 있다.
- 만약 캐시서버가 community 디렉티브를 이해하지 못하면 그냥 무시된다.
- extension token을 이해할 수 있는 캐시서버에 대해서만 의미가 있다.

## Connection

두가지 역할을 한다.

1. 프록시에 어디상 전송하지 않는 헤더필드를 지정한다.
2. 지속적 접속을 관리한다.

#### 자세히

```
Connection: Upgrade

//클라이언트 → 프록시서버
GET / HTTP/1.1 
Upgrade: HTTP/1.1
Connection: Upgrade

//프록시서버 → origin서버
GET / HTTP/1.1
```

- 더이상 전송하지 않을 헤더필드(hop-by-hop 헤더)를 지정할 수 있다.
- hop-by-hop 헤더: Connection, Keep-Alive, Proxy-Authenticate, Proxy-Authorization, Trailer, TE, Transfer-Encoding, Upgrade

```
Connection: Close
```

- HTTP/1.1에서는 지속적 접속이 default로 되어있다. 그래서 request를 송신했던 클라이언트는 접속을 계속 유지하면서 추가 request를 송신한다.
- 서버측에서 명시적으로 접속을 끊고 싶을 경우 헤더에 위와같이 `Connection: Close`를 지정한다.

```
Connection: Keep-Alive

//클라이언트 → 서버
GET / HTTP/1.1
Connection: Keep-Alive

//서버 → 클라이언트
HTTP/1.1 200 OK
Keep-Alive: timeout=10, max=500
Connection: Keep-Alive
```

- HTTP/1.1 이전 버전의 HTTP에서는 지속적 접속이 default가 아니었다.
- 그래서 오래된 버전의 HTTP에서 지속적 접속을 하고싶으면 위와같이 `Connection: Keep-Alive`를 지정한다.
- 클라이언트가 `Connection: Keep-Alive`붙여서 보내면 서버는`Connection: Keep-Alive`, `Keep-Alive` 붙여서 response한다.

## Date

HTTP 메시지를 생성한 날짜를 나타낸다.

```
Date: Tue, 03 Jul 2014 04:01:39 GMT
```

- HTTP 버전에 따라 Date Format이 조금씩 다르다.

## Pragma

HTTP/1.1 보다 오래된 버전의 흔적으로, 구버전 호환성을 위해 정의되어있는 헤더필드다.

```
Pragma: no-cache
``` 

- 지정할 수 있는 형식은 이거 하나뿐이다.
- 이 헤더필드는 General 헤더필드지만, 클라이언트의 request에서만 사용된다.
- 클라이언트는 캐시된 리소스의 response를 원하지 않는다! (클라이언트는 캐시 안타길 원한다!)를 나타낸다.

모든 중간서버가 HTTP/1.1을 기준으로 구성되어있다면 `Cache-Control:no-cache`를 사용하면 되지만, 모든 서버의 HTTP 버전을 일일이 확인하기가 어려우므로 아래와 같이 두개 다 보내기도 한다.

```
Cache-Control: no-cache
Pragma: no-cache
```

## Trailer

메시지 바디 뒤에 기술되어있는 헤더필드를 미리 전달할 수 있다.

- 이 헤더필드는 HTTP/1.1에 구현되어있는 청크전송코딩을 사용하는 경우에 사용할 수 있다.

## Transfer-Encoding

메시지 바디의 전송코딩형식을 지정한다.

- HTTP/1.1 전송코딩형식에는 청크전송만 정의되어있다. 

```
HTTP/1.1 200 OK
...
Transfer-Encoding: chunked
...
```
## Upgrade

HTTP 및 다른 프로토콜의 새로운 버전이 통신에 이용되는 경우 사용된다.

```
//클라이언트→서버
GET /index.html HTTP/1.1
Upgrade: TLS/1.0
Connection: Upgrade

//서버→클라이언트
HTTP/1.1 101 Switching Portocols
Upgrade: TLS/1.0, HTTP/1.1
Connection: Upgrade
```

- 클라이언트, 서버 양쪽 모두에 Connection 헤더필드가 지정되어있다는 점에 주의하자.
- Upgrade 헤더필드에 의해 업그레이드 되는 대상은 클라이언트와 인접한 서버뿐이므로, 더 뒤에 있는 depth에서는 무시되도록 `Connection:Upgrade`도 지정할 필요가 있다.
- Upgrade 헤어필드가 달린 request에 대해서 서버는 `101, Switching Protocols` respons로 응답할 수 있다.

## Via

Via 헤더필드는 클라이언트-서버 간의 request, response 메세지 경로를 알기 위해 사용된다.

- 프록시(or 게이트웨이)는 자신의 서버정보를 Via 헤더필드에 추가한 뒤 메시지를 전송한다.
- Via 헤더필드는 전송된 메시지의 추적, request 루프의 회피 등에 사용되므로 => 프록시를 경유하는 경우 명시할 필요가 있다.

Example. 클라이언트 → 프록시A → 프록시B → origin서버

```
//클라이언트 → 프록시A
GET / HTTP/1.1

//프록시A → 프록시B
GET / HTTP/1.1
Via: 1.0 gw.hackr.jp(Squid/3.1)

//프록시B → origin서버
GET / HTTP/1.1
Via: 1.0 gw.hackr.jp(Squid/3.1), 1.1 a1.example.com(Squid/2.7) 
```

- 각각의 프록시서버가 자기 자신의 정보를 Via 헤더필드에 추가한다.

## Warning

Respons에 관한 추가 정보를 전달한다.

- HTTP/1.0 Response Header (Retry-After)가 HTTP/1.1에서 변형된 것이다. 
- 기본적으로 캐시에 대한 경고를 유저에게 전달한다.

```
Warning: 113 gw.hackr.jp:8080 "Heuristic expiration" Tue, 03 Jul => 2012 05:09:44 GMT

Warning: [경고코드] [경고한 호스트:포트 번호] [경고문] ([날짜])
```

HTTP/1.1에는 7개의 경고코드가 있다.

|코드|경고문|설명|
|---|---|---|
|110|Response is state|프록시가 유효기간이 지난 리소스를 반환했다.|
|111|Revalidation failed|프록시가 리소스 유효성 재확인에 실패했다.|
|112|Disconnection operation|프록시가 네트워크로부터 고의로 끊겨있다.|
|113|Heuristic expiration|response가 24시간 이상 경과하고 있는 경우(=캐시 유효기간을 24시간 이상으로 설정하고 있는 경우)|
|199|Miscellaneous warning|임의의 경고문|
|214|Transformation applied|프록시가 인코딩과 미디어 타입 등에 대응해서 무언가 처리 한 경우|
|299|Miscellaneous persistent warning|임의의 경고문|


