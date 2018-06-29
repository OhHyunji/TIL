# Ch3. HTTP 정보는 HTTP 메시지에 있다

## 3.1 HTTP 메시지 구조

1. 메시지 헤더
2. 개행문자(CR+LF)
3. 메시지 바디

> 개행문자: 메시지 헤더/바디를 구분한다.
> 
> - CR: carriage return. 16진수.
> - LF: line feed. 16진수.

## 3.2 Request, Response 

### Request 메시지 구조

- 메시지 헤더
	- **리퀘스트 라인**: HTTP Method, URI, HTTP 버전
	- **리퀘스트 헤더필드**
	- 일반 헤더필드
	- 엔티티 헤더필드
- 개행문자
- 메시지 바디 	


### Response 메시지 구조

- 메시지 헤더
	- **상태 라인**: 상태코드+설명, HTTP 버전
	- **리스폰스 헤더필드**
	- 일반 헤더필드
	- 엔티티 헤더필드
- 개행문자
- 메시지 바디 	

## 3.3 인코딩

전송시 인코딩을 하면 

- (장) 전송효율을 높일 수 있다.
- (단) 컴퓨터에서 인코딩 처리해야하기 때문에, CPU등의 리소스는 더 많이 먹는다.

### 메시지, 엔티티 차이점

메시지

- HTTP 통신의 기본 단위.
- 8비트로 구성.
- 통신을 통해 전송된다.

엔티티

- Request, Response의 페이로드(Payload)로 전송되는 정보.
- `엔티티 헤더필드 + 엔티티 바디`로 구성된다.

메시지 바디는 Request, Response에 대한 엔티티 바디를 운반하는 역할을 한다. 기본적으로 메시지 바디와 엔티티 바디는 같지만, 전송 코딩이 적용된 경우 엔티티 바디의 내용이 변화하기 때문에 메시지 바디와 달라진다.

### 압축해서 보내는: 콘텐츠 코딩

- 콘텐츠 코딩은 엔티티에 적용하는 인코딩을 가리킨다.
- 엔티티 정보를 유지한 채로 압축한다.
- 수신한 클라이언트 측에서 디코딩한다.
- 콘텐츠코딩의 예: gzip, compress

### 분해해서 보내는: 청크 전송 코딩

- 청크 전송 코딩은 엔티티 바디를 청크로 분해해서 전송한다.
- 사이즈가 큰 데이터를 전송하는 경우, 데이터를 분할해서 조금씩 표시할 수 있다.
- 수신한 클라이언트 측에서 원래의 엔티티 바디로 디코딩한다.

## 3.4 Multipart

"여러 데이터를 보낸다."

- MIME(Multipurpose Internet Mail Extentions)
- MIME은 이미지 등의 바이너리 데이터를 아스키 문자열에 인코딩하는 방법과 데이터 종류를 나타내는 방법 등을 규정하고있다.
- 하나의 메시지 바디에 엔티티를 여러개 포함시켜 보낼 수 있다. 주로 이미지, 텍스트파일 등을 업로드할 때 사용된다.

```
Content-type: multipart/form-data
```

- web form으로 부터 파일 업로드시 사용된다.

```
Content-type: multipart/byteranges
```

- 206(Partial Content) Response 메시지가 복수 내용을 포함할 때 사용된다.

## 3.5 Range Request

"(범위를 지정해서) 일부분만 받는다."

- 예전에는 대용량 이미지나 데이터를 다운로드하기 힘들었다. 다운로드 중에 커넥션이 끊어지면 처음부터 다시 다운로드 해야했기 때문에.
- 이러한 문제를 해결하기 위해 **resume**이라는 기능이 필요하게 되었다. 

> resume: 이전에 다운로드하고있던 곳으로부터 다운로드를 재개한다.

- 이 기능을 실현하기 위해 엔티티 범위를 지정해서 다운로드해야하는데, 이렇게 **범위를 지정해서 Request**한다. (Range Request)

클라이언트: Request

```
GET tip.jpg HTTP/1.1
Host: www.usagidesign.jp
Range: bytes = 500	1-10000
```

서버: Response

```
HTTP/1.1 206 Partial Content
Date: 2018-06-29 22:50
Content-Range: bytes 5001-10000
Content-Length: 5000
Content-Type: image/jpeg
```

- 복수 범위의 Range Request인 경우, `multipart/byteranges` 로 Response가 온다.
- 서버가 Range Request를 지원하지 않는 경우, `200 OK` 상태코드와 함께 완전한 엔티티가 온다.

## 3.6 Content Negotiation

Example.

```
같은 URI에 엑세스 하더라도 상황에 따라 영어판/한국어판 웹페이지가 보여진다.
```

### Content Negotiation이란?

- 클라이언트에 더욱 적합한 리소스를 제공하기 위해, 클라이언트와 서버가 리소스 내용에 대해 교섭한다.
- Request의 헤더 필드를 참고해서 판단한다.
	- Accept
	- Accept-Charset
	- Accept-Encoding
	- Accept-Language
	- Content-Language

### Content Negotiation 종류

1. Server-driven Negotiation

	- 서버측에서 콘텐츠 네고시에이션을 한다.
	- 서버측에서 Request 헤더를 보고 판단해서 자동으로 처리한다.
	- (단) 브라우저가 보내는 정보를 근거로 하기 때문에, 유저에게 정말로 적절한것이 선택되었다고 할 수 없다.

2. Agent-driven Negotiation

	- 클라이언트 측에서 콘텐츠 네고시에이션을 한다.
	- 브라우저에 표시되는 선택지 중 유저가 수동으로 선택한다.
	- 또는 웹페이지에서 OS/브라우저에 따라 PC/Mobile용 웹페이지를 자동으로 전환한다.

3. Transparent Negotiation

	- Server-driven, Agetn-driven 을 혼합한 것이다.
	- Server, Client가 각각 콘텐츠 네고시에이션을 한다. 

