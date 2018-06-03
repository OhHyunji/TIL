# Future Combinator

Collection에 Combinator가 있듯이 Future에도 Combinator가 있다. (예: map, flatmap)

## flatmap

> 비동기식 함수를 순차적으로 적용하려면 flatmap을 사용하자.

```scala
Future[A].flatMap[B](f: A=>Future[B]): Future[B]
```

- 하나의 Future[A]와 비동기식 함수(f)를 받아서 => 새로운 Future[B]를 반환한다.
- 입력 Future가 성공적으로 완료되면 f를 호출한다.
- 이 호출의 결과는 새로운 Future 이며,
입력 Future[A], 비동기함수(f)가 모두 성공적으로 완료된 경우에만 완료된다.

### Example.

_Future[User]가 있고, 이 계정이 사용중지 되었는지 확인하는 isBannedAPI를 호출할거다._

1. 유저 정보를 가져와서
2. 이 유저의 계정이 활성상태인지 체크한다.

_이럴 때 flatMap을 사용할 수 있다._

## for-comprehension

> flatMap을 문법적으로 간편하게 해주는 for-comprehension이 있다.

```scala
val f = for {
    u <- getUser
    b <- isBanned(u)
} yield (u, b)
```
1. 유저 정보를 가져와서
2. 이 유저의 계정이 활성상태인지 체크한다.

## map

> 동기식 함수를 Future에 적용하려면 map을 사용하자.

### Example.

_Future[RawC]가 있고 Future[C]가 필요할 때.
동기식 함수 normalize가 RawC => C로 바꿔준다면 map을 사용하면 된다._

```scala
def normalize(raw: RawCredentials) = {
    new Credentials(raw.username.toLowerCase(), raw.password)
}
```
