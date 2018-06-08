# Custom Exception 

참고: [finatra user-guide](https://twitter.github.io/finatra/user-guide/)

## Exception 정의: extends Exception

```scala
class NotFoundUserInfoException extends RuntimeException
```

## Exception 발생: Future.exception

api로 userInfo를 가져오고, 없으면 new NotFoundUserInfoException

```scala
class SampleService @Inject()(
	userInfoService: UserInfoService
) {
	def getUserInfo(userId: Long): Future[User] = {
		val userF: Future[Option[User]] = userInfoService.getUserInfo(userId)
		userF.flatMap {
			case Some(f) => Future.value(f)
			case _ => Future.exception(new NotFoundUserInfoException)
		}
	}
}

```

## Exception 처리: Future.handle

```scala
class SampleController @Inject()(
	sampleService: SampleService) extends Controller {
	get("/users") {request: Request => 
		val userId = request.getParam("userId").toLong
		userService.getUserInfo(userId).handle {
			case _: NotFoundUserInfoException => response.internalServerError
		}
	}	
}
```

- NotFoundUserInfoException이 발생하면 InternalServcerError(500)으로 처리한다.
- 이외의 경우는 stackTrace가 찍힌다.

이걸 좀 더 응용하면

```scala
def toHttpStatus(value: CustomResponseStatus) = value match {
	case Success => response.ok
	case NotFoundUser | NotFoundEmail => response.notFound
	case _ => response.internalServerError
}
```

이렇게 처리하는 방법도 있다.
