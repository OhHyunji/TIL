# Fold

Option에 defaultValue를 setting할 때 `.getOrElse`를 썼었는데, 이번에 `fold`로 써봐서 정리해보았다.

## defaultValue를 setting 할 때

The fold method allows you to also specify an *initial value*.

Example. Convert: Option[Int] => Duration

```scala
//sessionTimeoutOpt: Option[Int]

//before: getOrElse
sessionTimeoutOpt.map(Duration.fromSeconds).getOrElse(Duration.Top)
//after: fold
sessionTimeoutOpt.fold(Duration.Top)(Duration.fromSeconds)
```

## fold, foldLeft, foldRight

```scala
def fold[A1 >: A](z: A1)(op: (A1, A1) ⇒ A1): A1
def foldLeft[B](z: B)(op: (B, A) ⇒ B): B
def foldRight[B](z: B)(op: (A, B) ⇒ B): B
```
* z: initial value
* op: operation

Example. fold, foldLeft, foldRight

```scala val list: List[Int] = List(1, 3, 5, 7, 9)

list.foldLeft(0)(_ + _)
// This is the only valid order
0 + 1 = 1
        1 + 3 = 4
                4 + 5 = 9
                        9 + 7 = 16
                                16 + 9 = 25 // done
                                
list.foldRight(0)(_ + _)
// This is the only valid order
0 + 9 = 9
        9 + 7 = 16
                16 + 5 = 21
                         21 + 3 = 24
                                  24 + 1 = 25 // done
                                  
list.fold(0)(_ + _) // 25
// One of the many valid orders
0 + 1 = 1    0 + 3 = 3             0 + 5 = 5
        1            3 + 7 = 10            5 + 9 = 14    
        1                    10          +         14 = 24
        1                        +                      24 = 25 // done
```

Example. With Collection *(in amm)*

```scala
@ numbers = List(2,3,4,5)

//foldLeft
@ numbers.foldLeft(0){(acc,i) => {println(s"acc=$acc, i=$i"); acc+i}}
acc=0, i=2	//op(0,2)
acc=2, i=3	//op(op(o,2),3)
acc=5, i=4	//op(op(op(o,2),3),4)
acc=9, i=5	//op(op(op(op(o,2),3),4),5)
res5: Int = 14
```

### another use case of  `fold`

공식

```scala
opt1.fold(alt)(foo)
opt1.map(foo).getOrElse(alt)
```

> 이게 이번에 사용한 부분이다.

Example. fold with Option

```scala
val opt1: Option[Int] = Some(5)
opt1.fold(0)(_+1)		//6
```


참고: [Scala fold, foldLeft and foldRight | Commit Logs](https://commitlogs.com/2016/09/10/scala-fold-foldleft-and-foldright/)
