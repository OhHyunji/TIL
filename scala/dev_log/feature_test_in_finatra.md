# Feature Test in Finatra

참고: [Feature Test](https://twitter.github.io/finatra/user-guide/testing/feature_tests.html)

- We highly recommend writing feature tests for your services
- as they provide a very good signal of whether you have correctly implemented the features of your service.

## Example1. HTTP Server 

1. extend the FeatureTest trait.
2. override the server definition with instance of your EmbeddedHttpServer.

```scala
import com.twitter.finatra.http.EmbeddedHttpServer
import com.twitter.finagle.http.Status
import com.twitter.inject.server.FeatureTest

class ExampleServerFeatureTest extends FeatureTest {
  override val server = new EmbeddedHttpServer(new ExampleServer)

  test("ExampleServer#perform feature") {
    server.httpGet(
      path = "/",
      andExpect = Status.Ok)
    ???
  }
}
```


### 주의! server를 선언할 때 `val`로 선언해야한다. 

```scala
override val server = new EmbeddedHttpServer(new ExampleServer)
```

server를 `def`로 선언하면

```scala

class ExampleServerFeatureTest extends FeatureTest {

  //1) 선언할때 한번 뜨고
  override def server = new EmbeddedHttpServer(new ExampleServer)

  test("ExampleServer#perform feature") {
  
  	 //2) 사용할때 한번 또 뜬다.
    server.httpGet(
      path = "/",
      andExpect = Status.Ok)
    ???
  }
}
```

`def`니까 사용할때 한번 또 뜬다.

그런데 EmbeddedHttpServer [코드](https://github.com/nagyistoce/twitter-finatra/blob/master/http/src/test/scala/com/twitter/finatra/http/test/EmbeddedHttpServer.scala#L69)를 열어보면 close() 한번만 한다.

그래서 (Server가 두개 떴는데 한개만 Close 되었으니..) 아래와 같은 Exception이 발생하게 된다.

```
java.lang.IllegalStateException: ExampleServer is not closed
    at com.twitter.inject.server.EmbeddedTwitterServer.assertCleanShutdown(EmbeddedTwitterServer.scala:296)
    at com.twitter.inject.server.FeatureTestMixin.afterAll(FeatureTestMixin.scala:64)
    ...(이하생략)...
```