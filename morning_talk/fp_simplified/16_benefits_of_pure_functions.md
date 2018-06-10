# 16. Benefits of Pure Functions

 benefits of pure functions are
 
 1. They’re easier to reason about
 2. They’re easier to combine
 3. They’re easier to test
 4. They’re easier to debug
 5. They’re easier to parallelize
 6. They are idempotent
 7. They offer referential transparency
 8. They are memoizable
 9. They can be lazy

## 1. They’re easier to reason about
 
- Pure Function Signatures Tell All.
- no side effects, no hidden input.

## 2. They’re easier to combine
 
- output depends only on input.
 
## 3. They’re easier to test

- can property-based testing.
- in impure function, you might get different results each time.

## 4. They’re easier to debug

- the output of a pure function depends only on the function’s input parameters and your algorithm.
- you don’t need to look outside the function’s scope to debug it.

## 5. They’re easier to parallelize

- there are no data dependencies.

## 6. They are idempotent

idempotent(멱등법칙)
 
- the result of executing it multiple times for a given input is the same as executing it only once for the same input.
- 같은 input 으로 한번 돌릴때나 여러번 돌릴때나 결과값이 같다.

## 7. They offer referential transparency

referential transparency(참조 투명성)

- it can be replaced by its resulting value without changing the behavior of the program.


## 8. They are memoizable

- Because a pure function always returns the same result when given the same inputs, a compiler (or your application) can also use caching optimizations.

## 9. They can be lazy

- 스칼라에서는 큰 데이터나 스트림을 다룰 때 주로 lazy를 사용하는데, 앨빈아저씨가 아직 이쪽은 마땅한 예제가 없다고 스킵하심;;