# Ch9. 흐름제어 추상화

TIL

```
- Higher-order function: 함수를 인자로 받을 수 있다. -> 자유도가 높다.
- Currying: 커링한 함수는 인자를 한개씩 받는다.
```

## 9.1 코드 중복 줄이기

### Higher-order function

- 스칼라에서는 함수를 인자로 받는다. (== 자유도가 높다.)
- function을 인자로 받는다는 것 == 로직이 넘어온다는것 == 자유도가 높다.
- 함수를 인자로 넘기면 코드 중복도 줄일 수 있다.

코드 중복이 많아지면

- 함수를 넘겨볼까? 고민해보자.

## 9.3 커링(Currying)

커링한 함수는 인자목록이 하나가 아니고 여럿이다.

- 인자가 하나만 들어간 인자목록을 2개 사용해 함수를 호출한다.

```scala
//전형적인 함수 정의, 호출
def plainSum(x: Int, y:Int) = x + y
plainSum(1, 2)

//커링한 함수 정의, 호출
def curriedSum(x:Int)(y:Int) = x + y
curriedSum(1)(2)
```

- curriedSum을 호출한 것은 실제로는 2개의 전통적인 함수를 연달아 호출한 것이다.

## 9.4 새로운 제어구조 작성

### resource를 다루는 함수를 currying, {}로 리팩토링 

resource를 다루는 함수를 만들어보자. 그리고 이를 추상화시켜보자.

```scala
//정의
def withPrintWriter(file: File, op: PrintWriter => Unit) {
	val writer = new PrintWirter(file)
	try {
		op(writer)
	} finally {
		writer.close()
	}
}

//호출
withPrintWriter(
	new File("date.txt"),
	writer => writer.println(new java.util.Date)
)
```
커링을 이용해 인자를 한개씩만 받도록 바꾸면 아래와 같다.

```scala
//정의
def withPrintWriter(file: File)(op: PrintWriter => Unit) {
	val writer = new PrintWriter(file)
	try {
		op(writer)
	} finally {
		writer.close()
	}
}

//호출
val file = new File("date.txt")
withPrintWriter(file) {
	writer => writer.println(new java.util.Date)
}
```

- 스칼라에서는 메소드에 넘겨기는 인자가 한개일 때 소괄호() 대신 중괄호{}를 사용할 수 있다.
- ()대신 {}를 사용해서 코드를 좀 더 내장제어 구조처럼 보이게 할 수 있다.
- 기존의 `withPrintWriter`에서는 인자가 두개라서 {}를 사용할 수 없으므로, 
	1. 커링을 이용해서 인자를 한개씩 받도록 바꾼 후 
	2. {}를 사용해서 코드를 정리했다. 

### resource를 관리하는 라이브러리(타입클래스)

- cats-effect
- scalaz-zio: Bracket

예제에서 resource에 대해 try/catch로 짠 부분을 좀 더 추상화한것이 위 라이브러리들의 Bracket부분(타입클래스) 이다.



