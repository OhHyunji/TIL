# HTTP 

TIL 

```
1. 서버가 "3xx 리다이렉트"로 응답
2. 클라이언트는 이 코드를 받으면 응답메시지의 Location 헤더를 보고 새 리소스에 접근한다. 

- 2xx 같은 응답들은 헤더에 상태코드+설명이 써있고, 그 밑에 html 문서가 오던지 요청한 데이터들이 오던지 한다.
- 하지만 3xx 응답들은 redirect 될 주소만 온다. 데이터를 담을 곳이 없다.
```

## HTTP 기본

- 클라이언트가 서버에 접속해서 Request 보내고 Response 받는다.
- 동기형 프로토콜: 서버에서 응답 올 때까지 대기한다.

## Stateful, Stateless

stateful: 클라이언트의 주문내역을 기억한다.

```
client: 햄버거 세트 하나 주세요.
client: 포테이토도요.
client: 콜라도요.
``` 

stateless: 매번 모든 주문을 반복한다.

```
client: 햄버거 세트 하나 주세요.
client: 햄버거 세트 하나랑 포테이토 주세요.
client: 햄버거 세트 하나랑 포테이토랑 콜라 주세요.
```

### stateful

특징: 서버가 클라이언트의 상태를 기억한다.

 - session 상태를 유지해야한다.
 - FTP가 대표적인 stateful 프로토콜이다. 

단점: 클라이언트 수가 늘어날수록 어려워진다.

- 하나의 서버가 동시에 상대할 수 있는 클라이언트 수는 한계가 있다.
- 서버가 늘어날 경우 클라이언트별로 상대할 서버를 지정할 수 없으므로, 상태 동기화가 필요하다.

### stateless

특징: 서버가 클라이언트의 상태를 기억하지 않는다.

장점: 클라이언트가 요청메시지에 필요한 정보를 모두 넣는다.

- 지난 대화를 기억해두지 않아도 현재 상태를 바로 알 수 있다.
- 서버를 늘리기만 해도 바로 확장 가능하다. 클라이언트의 요청이 매번 다른 서버로 가도 문제없다.

단점: 퍼포먼스 저하.

- 매번 필요한 정보를 모두 송신하기 때문에, 송신할 데이터의 양이 많아진다.
- 사용자인증 등 서버에 부하가 걸리는 처리를 반복한다.

## Status Code

- 1xx. 처리중
- 2xx. 성공
- 3xx. 리다이렉트
	- 다른 리소스로 리다이렉트.
	- **클라이언트는 이 코드를 받으면 응답메시지의 Location 헤더를 보고 새 리소스에 접근한다.**
- 4xx. 클라이언트 에러
- 5xx. 서버 에러

자주 사용되는 상태코드

> - 200, OK: 요청성공
> - 201, Created: 리소스 작성 성공. *Location 헤더에 새 URI.*
> - 301, Moved Permanently: 리소스가 새 URI로 (영원히)이동했음. *Location 헤더에 새 URI.*
> - 302, Moved temporarily: 리소스가 새 URI로 (잠시)이동했음. *Location 헤더에 새 URI.*
> - 303, See Other: 다른 URI 참조.
> - 400, Bad Request: 요청구문, 파라미터 오류.
> - 401, Unauthorized: 접근권한없음. 인증실패. *WWW-Authenticate 헤더*
> - 404, Not Found: 리소스 없음.
> - 500, Internal Server Error: 서버 내부 에러
> - 503, Service Unavailable: 서버가 점검 등으로 일시적 정지. *Retry-After 헤더에 재개 기간.*
> 

참고: [301, 302 Redirect 차이](http://nsinc.tistory.com/168)

참고: [웹을 지탱하는 기술](http://xguru.net/1033)

참고: [redirect 시에 데이터를 넘기고 싶어요(feat. HTTP)](https://milooy.wordpress.com/2016/03/03/pass-data-through-redirect-in-django/)