# scala 주요 개념들

week1에서 다루는 내용 중, 스칼라의 주요 개념들에 대해 정리.

## Evalutaion Rules

1. call by value
2. call by name

```scala
def example = 2      // evaluated when called
val example = 2      // evaluated immediately
lazy val example = 2 // evaluated once when needed

def square(x: Double)    // call by value
def square(x: => Double) // call by name
```

## Higher order functions

1. a function as a parameter 
2. return functions

```scala
def sum(f: Int => Int): (Int, Int) => Int = { ... }
```

## Currying

함수 이어붙이기.

- multiple arguments into a function with a single argument that returns another function.

```scala
def f(a: Int)(b: Int): Int //type is Int => Int => Int
```

## Class Organization
![class_organization](../../../images/20180606_scala_class_organization.png)

- scala.Any: base type of all types.
- scala.AnyVal: base type of all primitive types.
- scala.AnyRef: base type of all reference types.

#### Null, Nothing
- scala.Null: a subtype of any scala.AnyRef.
- scala.Nothing: a subtype of any other type.

## Type Parameters

Similar to Java generics.

```scala
class MyClass[T](arg1: T) { ... }  
new MyClass[Int](1)  
```

## Pattern Matching

```scala
userOpt match {
	case Some(u) => ...
	case _ => ...
}
```

## For-Comprehensions

syntactic sugar for 

1. map
2. flatMap 
3. filter 

operations on collections


