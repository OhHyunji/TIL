# TypeClass

TIL 

```
타입클래스의 목적: 상속없이 기능확장!

타입클래스 만들기
1. 타입클래스 선언: trait
2. 타입클래스 인스턴스: implicit
3. 인터페이스 작성

사용할 타입별에 대한 인스턴스가 있어야 
인터페이스(메서드)를 사용할 수 있다. 
```

객체지향(자바)에서는 기능을 확장할 때 상속을 사용한다.

타입클래스를 이용하면 코드의 수정/상속을 하지 않고 기능을 확장할 수 있다.

## Example

### 상속을 통한 확장 

Foo, Bar가 id, name을 반드시 가져야 할 때

```scala
trait Base {
	def id: Int
	def name: String
}

case class Foo(id: Int, name: String, createdAt: LocalDateTime) extends Base
case class Bar(id: Int, name: String, createdAt: LocalDateTime) extends Base
```

### TypeClass를 통한 확장 

타입클래스

```scala
object Sample {
  case class Foo(id: Int, name: String, age: Int)

  //1. 타입클래스(TC) 선언
  trait TC[A] {
    def id(a: A): Int
    def name(a: A): String
  }

  //2. 타입클래스의 인스턴스
  implicit val fooInstance = new TC[Foo] {
    override def id(a: Foo): Int = a.id
    override def name(a: Foo): String = a.name
  }

  //3. 인터페이스
  def info[A](foo:A)(implicit tc: TC[A]): (Int, String) = {
    (tc.id(foo), tc.name(foo))
  }
}
```

1. 타입클래스 선언
	- 특정 기능을 구현하기 위한 일반적인 특징
2. 타입클래스의 인스턴스
	- 우리가 쓰고자 하는 커스텀 타입에 대한 구현을 정의해놓은 object다.
	- `implicit`으로 선언해두면 컴파일러가 알아서 주입한다.
3. 인터페이스
	- 사용자들이 사용할 메소드를 정의

사용예

```scala
object Test extends App {
  import Sample._
  Sample.info(Foo(1, "azalea", 27))
}
```