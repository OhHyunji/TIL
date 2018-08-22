# sealed trait

TIL

```
sealed trait == enum
```

## Example

```scala
sealed trait ErrorStatus

object ResponseStatus {
	case object Success
	case object Fail with ErrorStatus
	case object NoSuchUser with ErrorStatus
	case object NoSuchUserEmail
}

```
pattern matching에 활용할 수 있다.

```
status match {
	case Success => info(s"[Success] $status")
	case s:ErrorStatus => error(s"[Fail] $s")
	case _ => warn(s"[Fail] $status")
}
```

- Fail, NoSuchUser는 error레벨 로그로 찍히고,
- NoSuchUserEmail은 warning레벨 로그로 찍힌다.

