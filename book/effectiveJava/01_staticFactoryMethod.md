# 01. 생성자 < 정적 팩터리 메서드

클래스의 인스턴스를 생성하도록 열어주는 방법

1. public 생성자
2. 정적 팩터리 메서드(static factory method)
3. builder 패턴

## public 생성자 보다 정적 팩터리 메서드: 장점

1. 이름을 가질 수 있다.

```java
Date.now();
Date.from(instatnt);
```

2. 호출될때마다 인스턴스를 새로 생성하지 않아도 된다.

```java
public static Boolean valueOf(boolean b) {
	return b ? Boolean.TRUE : Boolean.FALSE;
}
```

3. 반환타입의 하위타입 객체를 반환할 수 있다.
4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
5. 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.
   * 대표적인 예가 JDBC (p.11)

 