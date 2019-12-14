# 02. 생성자에 매개변수가 많다면 빌더를

생성자, 정적팩터리에는 똑같은 제약이 하나 있다. **선택적 매개변수가 많을 때 적절히 대응하기 어렵다.**

## 대안1. Telescoping Constructor Pattern (점층적 생성자 패턴)

example. 점층적 생성자 패턴 구현 예

```java
public NutritionFact(int calories) { ... }
public NutritionFact(int calories, int fat) { ... }
public NutritionFact(int calories, int fat, int carbohydrate) { ... }
```

단점

* 보통 이런 생성자는 사용자가 설정하기 원치 않는 매개변수까지 포함하기 쉬운데, 이런 경우 어쩔 수 없이 그런 매개변수에도 값을 지정해야만한다. (예: calories, carbohydrate만 사용할 경우)
* 매개변수 갯수가 많아지면 클라이언트 코드를 작성하기도, 읽기도 어렵다.
* 특히 타입이 같은 매개변수가 연달아 있으면 더 어렵다. 헷갈리고 버그가능성도 높다.

## 대안2. JavaBeans Pattern

1. NoArgs 생성자로 객체를 만든 후
2. setter 메서드를 호출해 원하는 매개변수의 값을 설정하는 방식

example. 자바빈즈 패턴으로 인스턴스를 생성하는 클라이언트 코드

```java
NutritionFact cocaCola = new NutritionFact();
cocaCola.setCalories(100);
cocaCola.setFat(50);
cocaCola.setCarbohydrate(27);
```

자바빈즈 패턴에도 단점이 있다.

1. 객체 하나를 만들려면 메서드를 여러개 호출해야하고,
2. 객체가 완전히 생성되기 전까지는 일관성(contistency)이 무너진 상태에 놓이게 된다.
   * 생성자 패턴에서는 매개변수들이 유혀한지 생성자에서만 확인하면 일관성을 유지할 수 있었는데, 그 장치가 완전히 사라진것이다. 
   * 일관성이 깨진 객체는 런타임에 문제가 생길 가능성이 높고 디버깅도 만만치 않다.
   * 일관성이 무너지는 문제 때문에 자바빈즈 패턴에서는 클래스를 불변으로 만들 수 없으며, 스레드 안전성을 얻으려면 프로그래머가 추가 작업을 해야만한다. 

## 대안3. Builder Pattern

example. 빌더 패턴 구현 예

```java
public class NutritionFact {
	private final int calories; 
	private final int fat; 
	private final int carbohydrate;
	
	public static class Builder {
		// required
		private final int calories;
		
		// optional
		private final int fat; 
		private final int carbohydrate;
		
		public Builder(int calories) {
			this.calories = calories;
		}
		
		public Builder fat(int fat) {
			this.fat = fat;
			return this;
		}
		
		public Builder carbohydrate(int carbohydrate) {
			this.carbohydrate = carbohydrate;
			return this;
		}
		
		public NutritionFact build() {
			return new NutritionFact(this);
		}
	}
	
	public NutritionFact(Builder builder) {
		calories = builder.calories;
		fat = builder.fat;
		carbohydrate = builder.carbohydrate;
	}
}
```

example. 빌더패턴으로 인스턴스를 생성하는 클라이언트 코드

```java
NutritionFact cocaCola = new NutritionFact.Builder(100).calories(200).build();
```

장점

* 빌더패턴은 점층적 생성자 패턴과 자바빈즈 패턴의 장점만 취했다.
* 이 클라이언트 코드는 쓰기 쉽고, 읽기도 쉽다.

빌더패턴에도 단점이 있긴 있다.

* 객체를 만들려면 빌더부터 만들어야한다. 빌더 생성비용이 크지는 않지만 성능에 민감한 상황에서는 문제가 될 수 있다.
* 점층적 생성자 패턴보다는 코드가 장황해서 매개변수가 4개 이상은 되어야 값어치를 한다.

## 핵심정리

* 생성자, 팩터리메서드에 매개변수가 많을때는 Builder 패턴을 고려해보자.