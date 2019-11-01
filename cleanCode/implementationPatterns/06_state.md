# 06. 상태

## 상태
* 인간의 두뇌는 선천적, 후천적으로 상태를 다루는 데 익숙하다.
* 그러나 프로그래밍에서 상태를 다루는건 어렵다.
* 특히 병렬 프로그래밍과 상태는 더 어렵다.

함수형 언어, 객체지향 언어
* 함수형 언어에는 변화하는 상태의 개념이 없다. 
	* 그래서 상태를 다루는 것에 대한 어려움은 없는데
	* 우리 두뇌에서 사고하는거랑은 좀 달라서 대중적으로 큰 인기를 얻지는 못했다.
* 객체지향 언어는 상태를 다루는데 적합하다.
	* 객체지향 언어에서는 전체 시스템을 매우 작은단위로 쪼갠 후
	* 접근제어자로 접근권한을 제한해서
	* “알지못하는사이에” 상태가 변해버리는 문제를 막아준다.

효과적으로 상태를 관리하기 위한 키포인트
1. 유사한 상태를 묶어서 관리하기
2. 각 상태를 별도로 관리하기

---
## 접근
* 프로그래밍은 결국 저장된 값에 *접근*해서 *계산*하는거다.

* 객체간의 접근 용이성을 위해 객체간의 독립성을 포기하는것은 바람직하지 않다.
	* 꼭 필요한 곳에만 public을 사용해서 객체간의 독립성을 지키자.
	* 그래야 “알지못하는 사이에” 상태가 변해버리는 문제를 예방할 수 있다.

## 직접 접근

```java
x = 10;
```

* 장점
	* 표현이 명확하다.
* 단점
	* 유연성이 떨어진다.
	* 프로그래머가 사고하는 수준보다 떨어진다. (= 코드만으로 의미가 잘 안읽힌다.)

```java
// 변수값에 1을 저장하는 것이 문을 여는것을 의미하는 경우
door = 1;
door.open();
```

## 간접 접근
* 메소드(setter)를 사용하면 명확성, 직접성을 희생해서 유연성을 얻을 수 있다.
* 저자는 클래스 내부에서는 직접 접근을 허락하지만, 클래스 외부에서는 간접 접근을 사용한다.

* 간접 접근이 특히 유용한 경우:
	* 두개 이상의 데이터 간에 의존관계가 있는 경우는 간접접근이 특히 유용하다.
```java
void setWidth(int width) {
	this.width = width;
	area = width * height;
}
```

---
## 공용상태
* 여러 메소드에서 같은 데이터 요소를 사용하는 경우, 클래스에 필드로 선언해서 사용하는것이 좋다.

```java
public class Ractangle {
	
	public int getArea(int width, int height) {
		return width * height;
	}

	public int getLength(int width, int height) {
		return (width+height)*2;
	}
}
```

Better:

```java
public class Ractangle {
	private int width;
	private int height;

	public Ractangle(int width, int height) {
		this.width = width;
		this.height = height;
	}

	public int getArea() {
		return width * height;
	}

	public int getLength() {
		return (width+height)*2;
	}
}
```

* 장점
	* 공용 상태를 사용하면 제대로된 객체를 만들기 위해 필드나 생성자에 어떤 요소가 필요한지 쉽게 알 수 있다. 
* 주의
	* 각 객체의 공용 상태는 모두 범위와 생명기간이 같아야한다.

> class 내에서 공용인거만 공용 field로 만들어서 -> 공용상태를 나타내고,
> 특정 method에 제한적이라면 local 변수로 사용하라는 이야기인듯.

## 가변상태

* 같은 객체에서도 인스턴스에 따라 다른 데이터요소를 필요로 할 때가 있다.

```java
class FlexibleObject {
	Map<String, Object> properties = new HashMap<String, Object>();

	Object getProperty(String key) {
		return properties.get(key);
	}

	void setProperty(String key, Object value) {
		properties.set(key, value);
	}
}
```

* 장점
	* 공용 상태에 비해 유연하다.
* 단점
	* 커뮤니케이션이 어렵다.

## 가변상태, 공용상태 코드 비교

```java
// 1. 가변상태를 사용한 설계
public class Widget {
	Map<String, Object> properties;
	// ("borderd", true)
	// ("borderWidth", 5)
	// ("borderColor", Color.RED)
}

// 2. 공용상태를 사용한 설계
public class Widget {
	boolean bordered;
	Integer borderWidth;
	Color color;
}
// 이 경우의 문제점:
// 객체의 모든 변수는 생명기간이 동일해야한다는 원칙을 어기게 된다.
// (bordered=false 이면 borderWidth=null, color=null이니까)

// 3. 다형성을 사용하면 좀더 명확하게 할 수 있다.
public class Widget {
	BaseBorder border;	
}
class Border extends BaseBorder {
	Integer width;
	Color color;
}
class UnBordered extends BaseBorder {
}
```

> **가능하다면 공용상태를 사용하라.** 
> 경우에 따라 어떤필드가 필요한지 확실하지 않은 경우에만 가변상태를 사용하라.

## 외재(Extrinsic) 상태

* 어떤 객체와 관련된 특수 목적 정보는 객체가 아니라 **그 객체를 필요로 하는 부분에** 저장하는게 낫다.

```java
public class CouponDto {
	int version;	// 호출하는 곳에서 저장해뒀다가 판단하는게 낫다.

	String itemName;
	String itemImageUrl;
	long price;
}
```

* 외재상태를 사용하는 경우는 많지 않다. 
* 하지만 적절하게 사용할 경우 외재상태는 매우 유용하게 사용될 수 있다.
	* 외재상태를 유용하게 사용해보신분!!??

---

## 변수

* 변수의 범위
	* 지역변수
	* 필드변수
		* public, package, protected, private
	* static 변수

> 대부분의 경우에는 지역변수를 사용하고, 간단히 static 변수와 필드변수를 사용해서 객체간의 의존성을 줄이는 것이 좋다.

* 변수의 생명기간
	* 같은 범위에서 정의되는 변수들은 모두 같은 생명기간을 갖게 하라.

* 변수의 타입
	* 선언하는 타입을 가급적 명확히 하라.
		* Example. Collection 보다는 ArrayList 로 명확하게 선언(103p)

* 변수이름
	* 변수이름에 많은 정보를 담으려고 하는것보다 **단순한 변수이름**을 사용해서 코드를 단순화하는게 더 낫다.
		* 변수의 범위, 생명기간, 타입 => 문맥으로 나타내고
		* 변수의 **역할** => 변수 이름

## 지역변수

* 지역변수는 변수가 선언된 지점이 속한 범위에서만 접근할 수 있다.
* 지역변수는 사용되기 직전에 가급적 최소범위 내에서 선언하라.

* 지역변수의 역할
	* 컬렉터: 이후에 사용되기 위한 정보를 모은다. 
		* Example: result, results
	* 카운터 
		* Example. count, rawCount, totalCount
	* 설명: 복잡한 표현로직에서 표현내용을 지역변수에 저장하면 코드가 좀 더 읽기 쉬워진다.
	* 재사용, 원소

```java
// 설명
int top=...;
int left=...;
int height=...;
int bottom=...;
return new Ractangle(top, left, height, width);

long now = Systme.currentTimeMillis();	// 재사용
for(Clock each: getClocks()) {			// 원소
	each.setTime(now);
}
```

## 필드
* 필드의 범위와 생명기간은 필드를 갖고있는 객체의 범위, 생명기간과 같다.
* 모든 필드는 클래스의 가장 앞이나 뒤에 한꺼번에 선언하는것이 좋다.
	* 보통 가장 앞에 선언하는듯! 

* 필드변수의 역할
	* 도우미: 여러 메소드에서 객체를 파라미터로 전달받는다면, 파라미터를 도우미 필드로 바꾸고 생성자에서 필드를 설정하는 방법을 고려해보자.
		* Example. 위에 공용상태 Ractangle 예제 참고
	* 플래그
		* Example. Coupon 클래스 -> used
	* 전략
		* 객체의 연산을 하는 다른 방법이 있음을 나타내는 경우, 그 부분을 수행하는 객체를 필드에 저장하라.
		* Example. Money 클래스 -> currency
		* 객체의 생명기간동안 연산방법이 바뀌지 않으면 -> 생성자에서 설정
		* 객체의 생명기간동안 연산방법이 바뀔 수 있으면 -> setter로 제공
	* 상태
		* 객체의 행위를 결정한다는 점에서 상태필드는 전략필드랑 비슷하다.
		* 하지만 상태필드는 스스로 다음 상태를 결정한다. 반면 전략필드는 다른 객체에 의해 설정된다.
		* Example. Coupon 클래스 -> status
			* 스스로 다음 상태를 결정한다는게.. Coupon.status는 Coupon의 다른 필드들(=여기서 말하는 스스로)조건에 따라 결정된다는 말인듯?
	* 부속
		* 객체가 소유하는 객체나 데이터를 저장한다.
		* Example. Coupon 클래스 -> item, releasedAt, expiredAt

---

## 파라미터

필드를 사용하면 클래스간에 강한 의존성이 생겨난다.

* 필드와 파라미터 둘다 가능한 경우라면, 파라미터로 사용하는 편이 낫다.

```java
// User를 필드로 사용한 경우
public class Member {
	private User user;
	private DateTime registeredAt;
	private Role role;

 	// Example. email이 있으면 유효하다고 판단하는 경우
	public boolean isValid() {
		return user.getEmail() != null;
		// User를 이 method 에서만 사용한다. 
		// 다른 method 들에서는 User를 한번도 안쓴다.
		// Member 클래스를 사용하는 쪽에서도 
		// role, registeredAt 만 필요하다.
  }
}

// 이럴때는 User를 파라미터로 사용하는게 낫다.

// User를 파라미트로 사용한 경우
public class Member {
	private DateTime registeredAt;
	private Role role;

	public boolean isValid(User user){ 
		return user.getEmail() != null;
	}
}
```

반복해서 같은 파라미터를 사용하면 의존성이 커진다.
* 여러 메소드에서 파라미터로 받고있다면, 필드로 사용하는게 낫다.

```java
member.isValid(user);
member.isStudent(user);
member.getDisplayNickname(user);

// 이런경우는 필드로 사용하는게 낫다.

// 클래스를 아래와같이 수정하면 
public class Member {
	private User user;		// 추가
	// 생략

	public Member(User user) {
		this.user = user;
 	}
}

// 위 코드는 이렇게 바뀔거다.
Member member = new Member(signedUser);
member.isValid();
member.isStudent();
member.getDisplayNickname();
```

## 수집 파라미터

* 결과들을 모아서 return 하는 경우 

```java
int size() {
	int result = 1;
	for (Node each: getChildren()) {
		result += each.size();
	}
	return result;
}
```

* 결과를 모으는 로직이 복잡하다면 수집 파라미터를 전달해서 결과를 수집하는게 더 직관적이다.
	* 수집파라미터 (= “여기다가 수집해서 return한다” 의미인듯;)

```java
asList() {
	List results = new ArrayList();	// 수집 파라미터
	addTo(results);
	return results;
}
```

## 옵션 파라미터

* 반드시 필요한 파라미터를 앞에서 전달하고, 옵션 파라미터는 뒤에 전달한다.
* 생성자의 경우, 옵션 파라미터별로 여러개 선언하면 된다.

```java
public ServerSocket()
public ServerSocket(int port)
public ServerSocket(int port, int backlog)
```

## 가변인자

```java
List<DisplayCoupon> map(Coupon ... coupons){ ... }
```

* 가변인자는 항상 마지막 파라미터여야한다.

## 파라미터 객체

* 여러개의 파라미터가 함께 여러 메소드로 전달된다면 이들을 묶어서 하나의 객체로 만드는 것을 고려할 수 있다.
```java
public class Pagination {
	int pageNum;
	int pageSize;
	boolean isLast;
}
```

* 장점
	* 코드가 짧아지고, 의도를 좀 더 명확히 전달할 수 있다. -> 가독성
	* 로직에 있어서도 그 데이터들이 밀접한 연관성이 있음을 보여준다.
* 단점
	* 성능
		* 객체를 생성할 때 시간이 걸리기 때문인데, 크리티컬하지 않다.

> 파라미터 객체를 사용하면 성능이 떨어지지만 크게 걱정할 수준은 아니므로,
> 파라미터 객체를 잘 활용해서 읽기 쉽고, 잘 나뉘어있고, 테스트하기쉬운 코드를 만들자!

---

## 상수

상수를 사용해야하는 경우 `static final`로 선언하고, 이 변수를 참조하자.

```java
public static final String COLOR_WHITE = "0xFFFFFF";
```

* 상수 변수를 사용하면 실수를 줄일 수 있다.
* 상수값 변경에도 용이하다.

---

## 변수이름(역할 제시형 작명)

* 변수의 **역할**을 전달할 수 있는 짧고 명료한 이름을 짓자.
	* 적합한 변수 이름을 생각하지 못하는 경우는, 그 연산을 잘 이해하고있지 못하는 경우가 많다.
* 변수 역할 외 다른 정보들(생명기간, 범위, 타입)은 문맥을 통해 전달할 수 있다.

## 선언타입

* 타입에는 구현보다는 역할을 반영해야한다.

```java
Collection<Person> members;		// 구현보다는
List<Person> members;			// 역할을 반영하자.
```

* 일반적인 선언타입(Collection)을 사용하면 클래스 구현을 변경하기가 쉽다.
	* 하지만 일반성을 위해 Collection을 선언했다 치자. 어디서는 List 어디서는 Set 으로 members를 다루고있다면 일관성이 없어서 코드를 읽는 사람이 어려움을 겪을것이다.
* 일관성을 위해서라면 어느정도 일반성을 포기하는것도 괜찮다.
	* 추가로 “members는 ArrayList이다” 라고 하는것은 “members는 Collection”이다 라고 하는것 보다 많은 정보를 함축하고있다.
		* = 명확하고 정확하다.

---

## 초기화

초기화 시점, 성능 차이를 가진 두 패턴이 있다.

* 열성적 초기화
* 게으른 초기화

## 열성적 초기화

* 변수가 선언되거나 생성되자마자 초기화하는 방법. (선언문, 생성자)
* 장점
	* 모든 변수가 초기화 이후에 사용되는 것을 보장할 수 있다.
	* 초기화와 선언을 함께하면 한곳에서 선언타입과 실제 타입을 쉽게 확인할 수 있다.
* 단점
	* (비교적) 성능이 떨어진다.

## 게으른 초기화

* getter를 만들고 이 메소드가 호출될 때(이 변수를 필요로 할때) 초기화한다.

```java
Collection<Person> getMembers() {
	if(member == null)
		member = new ArrayList<Person>();
	return members;
}
```

* 장점
	* 성능
* 단점
	* 코드가 복잡하다.

> 이 방식은 과거 컴퓨터의 처리능력이 떨어지던 시절에 많이 사용되던 기법이다.
> 이 방식을 사용했다면 “아, 이부분에서는 성능이 매우매우 중요하구나” 생각하면 된다.