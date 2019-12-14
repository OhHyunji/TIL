# 01. 생성자 < 정적 팩터리 메서드

클래스의 인스턴스를 생성하도록 열어주는 방법

1. public 생성자
2. 정적 팩터리 메서드(static factory method)
3. builder 패턴

## 생성자보다 정적 팩터리 메서드: 장점

1. 이름을 가질 수 있다.
2. 호출될때마다 인스턴스를 새로 생성하지 않아도 된다.
3. 반환타입의 하위타입 객체를 반환할 수 있다.
4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
5. 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.
   * 대표적인 예가 JDBC (p.11)

example.

```java
// 이름을 가질 수 있다.
Date.now();
Date.from(instatnt);

// 호출될때마다 새로운 인스턴스를 생성하지 않아도 된다.
public static Boolean valueOf(boolean b) {
	return b ? Boolean.TRUE : Boolean.FALSE;
}
```

## 생성자보다 정적 팩터리 메서드: 단점

1. 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.
2. 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.
   * 그래서 정적 팩터리 메서드를 선언할때는 잘 알려진 네이밍 규약을 따르는게 좋다.

example. 정적 팩터리 메서드 네이밍 규약 예

```java
Date.from(instant);
// 매개변수 하나 받아서 인스턴스를 반환

EnumSet.of(APPLE, BANANA, ORANGE);
// 매개변수 여러개를 받아서 인스턴스를 반환

BigInteger.valueOf(Integer.MAX_VALUE);
// from, of의 더 자세한 버전

StackWalker.getInstance(options);
Singleton.getInstance();
// 같은 인스턴스임을 보장하지는 않는다.

Array.newInstance(classObject, length)
// 매번 새로운 인스턴스를 생성해 반환함을 보장한다.

FileStore fs = Files.getFileStore(path);
// 다른 클래스에 팩터리메서드를 정의할때 getType() 네이밍을 활용한다.

BufferedReader br = Files.newBufferedReader(path);
// (newInstance 와 유사) 다른 클래스에 팩터리 메서드를 사용할때 newType() 네이밍을 활용한다.

List<User> users = Collections.list(user);
// getType(), newType()의 간결한 버전
```

## 핵심정리

* public 생성자, 정적 팩토리 메서드는 각자의 쓰임이 있으니 → 각각의 장단점을 이해하고 사용하는것이 좋다.
* 보통은 정적 팩터리 메소드를 사용하는것이 유리한경유가 더 많으므로, 무작정 public 생성자를 제공하던 습관이 있었다면 고치다.




 