# Reactive Programming & ReactiveX 지식공유회

TIL

```

```

## Reactive Programming

리액티브 프로그래밍은 여러 패러다임 중 하나다. (oop, fp 등등)

Reactive Programming은 특정 이벤트 발생에 대해 반응, 동작하도록 구현하는 패러다임이다.

- 평소에 알고있던 callback, listener가 바로 리액티브 프로그래밍의 시작이다.


## ReactiveX

리액티브 프로그래밍을 언어 무관하게 사용할 수 있도록 정의한 프로토콜이다.

- ReactiveX는 개념이고, 
- 이걸 여러 랭귀지 진영에서 구현한게 RxJava, RxJS, RxAndroid 등등 이다.

ReactiveX 에는 Observer, Schedular,.. 등등 개념이 있다.

- 참고: [ReactiveX docs](http://reactivex.io/intro.html)

기존의 리액티브 프로그래밍은 한계가 있었는데, 이를 개선한 신 패러다임이다.


기존 리액티브 프로그래밍의 한계

1. 콜백헬
2. 변수할당, thread 등등 -> 직접 관리해야함.

=> 코드가 복잡해져서 잘 안읽히고 유지보수가 어렵다.

그래서 기존 리액티브 프로그래밍 개념에 + 함수형 프로그래밍 개념을 좀 넣어서 => ReactiveX가 탄생했다.

- Rx잘 알려면 FP잘 알아야한다.

잠깐 FP 특징을 설명하면

1. 순수함수다. (동일input -> 동일output, side effect 없음)
2. 순수함수들을 조합 -> 큰 함수들을 만들어나간다.

