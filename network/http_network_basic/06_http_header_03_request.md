# Ch6. HTTP헤더(3): Request Header Fields

- Request 메시지에 사용되는 헤더다.
- Request의 부가정보, 클라이언트의 정보, Response의 콘텐츠에 대한 우선순위 등

## Accept

처리할 수 있는 미디어타입과 상대적인 우선순위를 userAgent에 전달하기 위해 사용된다.

```
Accept: text/plain;q=0.3, text/html
```
> 클라이언트: "그 리소스 가능하다면 HTML로 받고싶은데 안되면 plainText라도 괜찮아!"

- 미디어타입은 아래와 같다.
	- 텍스트파일: text/html, text/plain, text/css, application/xhtml+xml, application/xml, ...
	- 이미지파일: image/jpeg, image/gif, image/png, ...
	- 동영상파일: video/mpeg, video/quicktime, ...
	- 어플리케이션용 바이너리파일: application/zip, ...

- 브라우저가 PNG이미지를 표시하지 못하는 경우:  Accept에 처리가능한 이미지타입을 지정한다. (예: image/gif, image/jpeg)
- 표시하는 미디어 타입에 우선순위를 붙이고 싶은 경우: `;q=0.9` 이런식으로 표시할 품질 계수를 명시한다.
	- 품질계수: 0~1 범위의 수치(소수점 3자리까지)
	- default: q=1.0
	- 서버가 여러개의 컨텐츠를 반환할 수 있는 경우, 가장 높은 품질계수의 미디어 타입으로 반환할 필요가 있다.