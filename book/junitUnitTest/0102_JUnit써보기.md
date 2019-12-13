# 1장. 첫번째 JUnit 테스트 만들기

```java
import static org.junit.Assert.*;
import org.junit.*;

public class SampleTest {

	@Test
	public void test() {
		// 1. arrange: 테스트 상태 설정 
		// 2. act: 검증하려는 코드 실행
		// 3. assert
	}
}
```

# 2장. JUnit 진짜로 써보기

어떤 테스트를 작성할지? → 테스트 케이스

* Criteria 인스턴스가 Criterion 객를 포함하지 않을 때
* answers.get()에서 반환된 Answer 객체가 null일 때 등등 (p.46)

> 코드 p.43 참고

JUnit에서 각 테스트는 고유 맥락을 갖는다.

* 즉 JUnit은 결정된 순서로 테스트를 실행하지 않으며며, 모든 테스트는 다른 테스트 결과에 영향을 받지않는다.
* JUnit은 테스트 두개를 위해 각각 별도의 Test 인스턴스를 생성한다.
 
## @Before

여러 테스트 메소드에 공통으로 필요힌 초기화로직은 여기에 담자.

JUnit 작동순서는 다음과같다.

1. JUnit은 새로운 ProfileTest 인스턴스를 만들고 profile, question, criteria 필드는 초기화되지 않았다.
2. JUnit은 @Before 메서드를 호출해서 profile, question, criteria를 적절한 인스턴스로 초기화한다.
3. JUnit은 matchAnsersTrueTest() 를 실행하고 테스트가 성공/실패했는지 표기한다.
4. 다른 테스트가 있기 떄문에 JUnit은 ProfileTest 인스턴스를 새롭게 생성한다.
5. JUnit은 새로운 인스턴스에 대해 @Before메서드를 호출해서 필드를 초기화하고
6. JUnit은 다른 테스트 메서드 matchAnsersFalseTest() 를 호출한다.

JUnit은 테스트마다 새로운 인스턴스를 생성한다.

* JUnit은 이런 방식으로 모든 테스트를 독립적으로 만든다.
* 만약 여러 테스트메서드가 동일한 인스턴스에서 실행된다면 공유된 Profile 객체 상태를 clean up 하는것도 신경써야했을거다.

테스트클래스에는 static필드를 피해야한다.

*  테스트마다 새로운 인스턴스를 생성해도 상태가 공유되기 때문이다.


## 마무리
 
테스트코드를 작성하면 메서드가 기대한대로 동작한다는 자신감을 가질 수 있다.

9장에서는 좋은 설계가 어떻게 테스트를 쉽게 만드는지 보여줄거다.
12장에서는 작은 양의 코드를 만들면서 테스트하면 테스트가 자연스럽고 즐거워진다는 것을 알아볼것이다.