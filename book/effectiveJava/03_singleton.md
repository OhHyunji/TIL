# 03. private 생성자나 열거타입으로 싱글턴임을 보증하라

## 싱글턴

싱글턴 

* 인스턴스를 오직 하나만 생성할 수 있는 클래스
* 싱글턴의 전형적인 예: 설계상 유일해야하는 시스템 컴포넌트

그런데 클래스를 싱글턴으로 만들면 이를 사용하는 클라이언트를 테스트하기가 어려워진다.

* 싱글턴 인스턴스를 mock 구현으로 대체할 수 없기 때문이다.

## 싱글턴을 만드는 방법

### 1. public 필드

```java
public class Elvis {
	public static final Elvis INSTANCE = new Elvis();
	
	private Elvis() { ... }
}
```

* private 생성자는 public static final 필드인 `Elvis.INSTANCE`를 초기화할 때 딱 한번만 호출된다.
* public이나 protected 생성자가 없으므로 Elvis 클래스가 초기화될 때 만들어진 인스턴스가 전체 시스템에서 하나뿐임이 보장된다.

장점

* 해당 클래스가 싱글턴임이 API에 명확히 드러난다. 
* 간결하다.

### 2. public 정적 팩터리 메서드

```java
public class Elvis {
	// 필드는 private 으로 바꾸고 
	private static final Elvis INSTANCE = new Elvis();
	
	private Elvis() { ... } 
	
	// 정적 팩터리 메서드를 public 으로 제공
	public static Elvis getInstatnce() { return INSTANCE; }
}
```

* `Elvis.getInstance()`는 항상 같은 객체의 참조를 반환한다.

장점

* API를 바꾸지 않고도 싱글턴이 아니게 변경할 수 있다.
* 정적 팩터리를 제네릭 싱글턴 팩터리로 만들 수 있다.
* 정적 팩터리의 메서드 참조를 Supplier로 만들 수 있다.

```java
public static Supplier<Elvis> getInstance = () -> INSTANCE;
```

이러한 장점들이 굳이 필요없다면 더 간결한 public 필드 방식이 더 좋다.

두 방법 모두 싱글턴 클래스를 직렬화하려면 Serializable을 구현한다고 선언하는것 말고도 추가 작업들이 필요하다. (p.24)

### 3. 원소가 하나인 열거타입

```java
public enum Elvis {
	INSTANCE;
}
```

장점

* public 필드 방식과 비슷하지만 더 간결하고, 추가 노력없이 직렬화할 수 있고, 아주 복잡한 직렬화 상황이나 리플렉션 공격에도 제 2의 인스턴스가 생기는 일을 완벽히 막아준다.
* 조금 부자연스러워 보일 수 는 있으나 대부분 상황에서는 원소가 하나뿐인 열거타입이 싱글턴을 만드는 가장 좋은 방법이다.

단점

* 단, 만들려는 싱글턴이 Enum 외에 클래스를 상속해야한다면 이 방법은 사용할 수 없다.

## 핵심정리

싱글턴을 만드는 방법

1. public static final 필드
2. public 정적 팩토리 메서드
3. 원소가 하나인 열거타입

"원소가 하나인 열거타입" 방법이 제일 간결하고 좋다.

* 그다음은 "public satatic final 필드" 방법, 
* 그다음은 "publid 정적 팩토리 메서드" 방법.