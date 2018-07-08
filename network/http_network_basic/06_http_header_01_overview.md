# Ch6. HTTP 헤더

## 6.1  HTTP 메시지 헤더

### HTTP 메시지

```
메시지 헤더 + 개행문자 + 메시지 바디
```

메시지 헤더

- HTTP 프로토콜의 request, response 에는 반드시 메시지 헤더가 포함되어있다.
- 메시지 헤더에는 클라이언트/서버가 request, response를 처리하는 데 필요한 정보들이 들어있다.

### request 메시지 헤더

- HTTP 메소드, URI, HTTP 버전
- HTTP 헤더필드

```
GET / HTTP/1.1
Accept: text/html, application/xml
Accept-Encoding: gzip, deflate,sdch
...
```

### response 메시지 헤더

- HTTP 버전, 상태코드
- HTTP 헤더필드

```
HTTP/1.1 304 Not Modified
Connection: close
Date: Tue, 08 July 2018 14:12:08
...
```

## 6.2 HTTP 헤더필드

request, response시 브라우저/서버에 부가적인 정보를 전달한다.

### 구조: Key, Value

```
Content-Type: text/html
Keep-Alive: timeout=15, max=100
```

- 하나의 HTTP 헤더 필드가 여러개의 값을 가질 수도있다.

### 종류

Hearders can be grouped according to **their contexts**:

1. **General** Header: request, response
2. **Request** Header: request
3. **Response** Header: response
4. **Entity** Header: request, response

	> more information about the body of entity, like its content length or its MIME-type.

Hearders can be grouped according to **how proxies handle them**:

1. **End-to-end** headers: 이 그룹의 헤더들은 request, response의 최종 수신자에게 전송된다.

	> - These headers must be transmitted to the final recipient of the message. (that is, the server for a request or the client for a response)
	> - Intermediate proxies must retransmit end-to-end headers unmodified and caches must store them.
	
2. **Hop-by-hop** headers: 이 그룹의 헤더들은 한번 전송에 대해서만 유효하다. 캐시, 프록시에 의해 전송되지 않는것도 있다.

	> - These headers are meaningful only for a single transport-level connection and must not be retransmitted by proxies or cache.
	> - hop-by-hop headers may be set using the Connection(in General Header Field)
	> - hop-by-hop headers: Connection, Keep-Alive, Proxy-Authenticate, Proxy-Authorization, Trailer, TE, Transfer-Encoding, Upgrade. (이 8개 이외 헤더필드들은 모두 end-to-end header로 분류된다.)
	
### 종류(자세히)

#### 1. General Header Fields

|Header Field|설명|
|---|---|
|Cache-Control|캐싱 동작 지정|
|Connection|hop-by-hop 헤더, 커넥션 관리.|
|Date|메시지 생성 날짜|
|Pragma|메시지 제어|
|Trailer|메시지 끝에 있는 헤더|
|Transfer-Encoding|메시지 바디의 전송 코딩형식 지정|
|Upgrade|다른 프로토콜에 업그레이드|
|Via|프록시서버 정보|
|Warning|에러 통지|


#### 2. Request Header Fields

- request 메시지 헤더에 들어가는 내용이다.

|Header Field|설명|
|---|---|
|Accept|userAgent가 처리 가능한 미디어타입|
|Accept-Charset|CharSet 우선순위|
|Accept-Encoding|Contents Encoding 우선순위|
|Accept-Language|언어(자연어) 우선순위|
|Authorization|웹 인증을 위한 정보|
|Expect|서버에 특정 동작을 기대|
|From|유저의 메일 주소|
|Host|요구된 리소스의 호스트|
|If-Match|엔티티 태그 비교|
|If-None-Match|엔티티 태그 비교|
|If-Modified-Since|리소스 갱신시간 비교|
|If-Unmodified-Since|리소스 갱신시간 비교|
|If-Range||
|Max-Forwards||
|Proxy-Authorization||
|Range||
|Referer||
|TE||
|User-Agent||


#### 3. Response Header Fields

- response 메세지 헤더에 들어가는 내용이다.

|Header Field|설명|
|---|---|
|Accpet-Range||
|Age||
|Etag||
|Location||
|Proxy-Authenticate||
|Retry-After||
|Server||
|Vary||
|WWW-Authenticate||

#### 4. Entity Header Fields

- request, response 메시지에 포함된 엔티티에 관한 부가정보다.

|Header Field|설명|
|---|---|
|Allow||
|Content-Encoding||
|Content-Language||
|Content-Length||
|Content-Location||
|Content-MD5||
|Content-Range||
|Content-Type||
|Expires||
|Last-Modified||

#### 5. Etc

HTTP/1.1 표준으로 정의된 헤더 필드 외에 비표준 헤더 필드가 있다.

- 예: Set-Cookie, Content-Desposition 등

