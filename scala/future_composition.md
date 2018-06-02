# Future Composition

## 1. for-comprehension

```scala
for {
      a <- Future.value(1)
      b <- Future.value(1)
} yield a
```
- Future를 순차적으로 조합할 때 faltMap을 사용한다.
- faltMap호출을 문법적으로 간편하도록 만든것이 for-comprehension
- 그런데 이것은 Future가 순차적으로 처리되므로, a가 complete 되어야 b가 실행된다.

## 2. [비동기] Future.join (twitter)
```scala
  Future.join(Future.value(1), Future.value(1)).map(a => a)
```
- `join[A, B, C](a: Future[A], b: Future[B], c: Future[C]) => Future[(A, B, C)]`
- 여러 타입 Future들을 받아서 Future[tuple]로 담아준다.
- 참고: finagle문서 [ko](https://twitter.github.io/scala_school/ko/finagle.html), [en](https://twitter.github.io/scala_school/finagle.html)
 
## 3. [비동기] mapN (cats)
```scala
(Future.value(1), Future.value(1)).map2((a, b) => a)
```
- `map2[F[_], A0, A1, Z](f0:F[A0], f1:F[A1])(f: (A0, A1) => Z) => : F[Z]`
- Future.join 이랑 비슷한 시그니처 cats 버전인듯 하다.
- 참고: [Parallel futures and exceptions](http://immutables.pl/2016/10/08/parallel-futures-and-exceptions/)

```
(Future.value(1), Future.value(1)).tupled
```
- tupled를 사용하면 Future들의 결과가 tuple로 한방에 return 된다.

### 추가로 알게된것
|@|

- The symbolic alias for product is |@|
- "데카르트의 곱(Cartesian product)"
- 참고: http://eed3si9n.com/herding-cats/Cartesian.html