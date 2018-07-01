# Ch4. HTTP 상태코드

## 상태코드 클래스

| 코드 | 클래스 | 설명 | 
|---|---|---|
| 1xx | Informational | Request를 받아들여 처리중이다. |
| 2xx | Success | 정상 |
| 3xx | Redirection | Request를 완료하기 위해 추가동작이 필요하다. |
| 4xx | Client Error | 서버는 Request를 이해할 수 없다. |
| 5xx | Server Error | 서버가 Request를 처리하다가 실패했다. |

## 2xx, Success

### 200, OK

- GET: Request 된 리소스에 대응하는 엔티티가 Response로 보내진다.
- HEAD: Request 된 리소스에 대응하는 엔티티의 헤어필드만 Response로 보내진다.

> HEAD 메소드: GET과 같은 기능이지만 메시지 바디는 안온다. 헤더만 온다. URI 유효성이나 리소스 갱신시간을 확인하는 목적으로 사용된다.

### 204, No Content

- 서버가 Request 받아서 처리하는 데 성공했지만, Response에 엔티티 바디를 포함하지 않는다. 
- 사용예: 클라이언트에서 서버에 정보를 보내는 것으로 끝이고, 클라이언트가 새로운 정보를 받을 필요가 없는 경우 사용한다.

### 206, Partial Content

- 서버가 범위가 지정된 GET Request를 받았다.
- Response에는 Content-Range로 지정된 범위의 엔티티가 포함된다.

## 3xx, Rediredtion

Request를 정상적으로 처리하기 위해서는 브라우저측에서 뭔가 해줘야한다.

## 4xx, Client Error

클라이언트의 원인으로 에러가 발생했다.

### 400, Bad Request

- Request 구문이 잘못되었다. 
- 해결: Request 내용을 재검토하고나서 재송신하자.

### 401, Unauthorized

- 송신한 Request에 HTTP 인증정보가 필요하다.
- 이미 한번 Request가 이루어진 경우, 유저인증에 실패했음을 나타낸다.

### 403, Forbidden

- Request 된 리소스에 Access가 거부되었따.
- 원인
	1. **파일시스템의 Permission 문제**
	2. **Access 권한에 문제**(허가되지 않은 송신IP주소의 Access 등)

### 404, Not Found

- Request한 리소스가 서버상에 없다.

## 5xx, Server Error

### 500, Internal Server Error

- 서버에서 Request를 처리하는 도중 에러가 발생했다.
- 웹애플리케이션에 에러가 발생한 경우.

### 503, Service Unavailable

- 일시적으로 서버가 과부하 상태이거나 점검중이어서, 현재 Request를 처리할 수 없다.