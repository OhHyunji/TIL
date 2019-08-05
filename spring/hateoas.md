# HATEOAS

**Hypermedia** As The Engine Of Application **State**



## HATEOAS

* rest 애플리케이션 아키텍처 컴포넌트 중 하나로,
* 클라이언트가 하이퍼미디어를 통해 서버와 동적으로 정보를 주고받을 수 있는 방법이다.
  * 어떤 상태에 따라 다음 액션으로 가능한 url이 내려가고
  * 클라이언트는 이 url을 통해 동적으로 정보를 얻는다.

* 클라이언트가 서버한테 어떤 요청을 하면 서버는 다음 요청에 필요한 url을 응답에 포함시켜서 내려준다.
  

이게 무슨말이냐하면

example. 아래와 같은 request를 보냈을 때

```html
GET /accounts/12345/ HTTP/1.1
Host: bank.example.com
Accept: application/xml
```



잔액이 100달러인 경우, 아래와같은 response를 받는다.

```html
HTTP/1.1 200 OK
Content-Type: application/xml
Content-Length: ...

<?xml version="1.0"?>
<account>
    <account_number>12345</account_number>
    <balance currency="usd">100.00</balance>
    <link rel="deposit" href="/accounts/12345/deposit" />
    <link rel="withdraw" href="/accounts/12345/withdraw" /> 
    <link rel="transfer" href="/accounts/12345/transfer" />
    <link rel="close" href="/accounts/12345/close" />
</account>
```

* 다음 액션으로 deposit, withdraw, transfer, close 액션을 할 수 있다.

  

잔액이 마이너스인 경우, 아래와같은 response를 받는다.

```html
HTTP/1.1 200 OK
Content-Type: application/xml
Content-Length: ...

<?xml version="1.0"?>
<account>
    <account_number>12345</account_number>
    <balance currency="usd">-25.00</balance>
    <link rel="deposit" href="/accounts/12345/deposit" />
</account>
```

* 다음 액션으로 deposit만 가능하다. 

참고: https://en.wikipedia.org/wiki/HATEOAS



### 정리하면

resource의 state에 따라 가능한 action이 달라지고, 이 action을 linkUrl로 제공한다.



## spring-hateoas

- 스프링 프로젝트중 하나로,
- HATEOAS 원칙을 만족하는 rest api를 만들 때 유용한 라이브러리다.
- spring-hateoas 에는 이런 기능이 있다.
  1. 링크를 만드는 기능
  2. 리소스를 만드는 기능
  3. 링크를 찾아주는 기능
     

### 링크 만드는 방법: 두가지 방법이 있다.

1. `new Link()`

   참고: https://docs.spring.io/spring-hateoas/docs/1.0.0.RC1/reference/html/#fundamentals.uri-templates

2. `linkTo`

```java
@GetMapping("/employees/{id}")
Resource<Employee> one(@PathVariable Long id) {

  Employee employee = repository.findById(id)
    .orElseThrow(() -> new EmployeeNotFoundException(id));

  return new Resource<>(employee,
    linkTo(methodOn(EmployeeController.class).one(id)).withSelfRel(),
    linkTo(methodOn(EmployeeController.class).all()).withRel("employees"));
}
```

```json
{
  "id": 1,
  "name": "Bilbo Baggins",
  "role": "burglar",
  "_links": {
    "self": {
      "href": "http://localhost:8080/employees/1"
    },
    "employees": {
      "href": "http://localhost:8080/employees"
    }
  }
}
```

참고: https://spring.io/guides/tutorials/rest/



### 링크에는 크게 두가지 정보가 들어간다.

1. href
2. rel: relation. 현재 이 리소스와의 관계를 표현한다.
   * self: 자기 자신에 대한 url을 넣을 때.
   * profile: 이 응답 본문에 대한 문서를 링크걸 때



example. 만약 employee를 생성한 후 response의 _links에는 아래와 같은 rel이 있을 수 있다.

* self
* profile: 나자신 데이터모양에 대한 설명서
* update: 수정하고 싶을 수 있으니까 

이런 링크들을 포함해서 줄 수 있을거다.



참고: https://youtu.be/Uc8CzBpHOjQ
