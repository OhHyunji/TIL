# Reactive Systems are

1. Responsive
2. Resilient
3. Elastic
4. Message Driven

## Responsive(응답성)

> 시스템이 가능한 한 즉각적으로 응답해야한다.

- Responsive는 사용자의 편의성, 유용성의 기초가 된다.
- Responsive는 문제가 발생했을 때 신속하게 탐지하고 효과적으로 대처하는것을 의미하기도 한다
- Responsive System은 신속하고 일관성있는 응답시간을 제공하고, 신뢰할 수 있는 상한선을 설정하여 일관된 서비스품질을 제공한다.

## Resilient(탄력성)

> 시스템이 장애가 나도 응답성을 유지한다.

- Resilient은 highly-available, mission-critical systems 시스템에만 적용되지 않는다.
- 탄력성이 없는 시스템은 장애 발생시 응답성을 잃게된다.
- Resilience은 replication, containment(봉쇄), isolation and delegation(위임)에 의해 실현된다. 
- 시스템은 여러 컴포넌트(구성요소)로 이루어져있고, 장애는 어떤 부분에서든 발생할 수 있다. 각각의 컴포넌트들은 서로 분리되어있기 때문에 시스템이 부분적으로 고장나더라도 전체시스템을 위험하지 않게 복구할 수 있도록 보장한다.

## Elastic(유연성)

> 시스템 작업량이 변화해도 응답성을 유지한다.

- Reactive System은 input rate의 변화에 따라 할당된 자원을 늘리거나 감소시키면서 변화에 대응한다.
- 시스템에 병목현상이 없도록 설계해서, 구성요소를 샤딩하거나 복제해서 input을 분산시키는 것을 의미한다.

## Message Driven(메시지 구동)

> Reactive System은  asynchronous message-passing에 의존한다.

읽어도 잘 이해가 안간다.. 이것을 내일 TIL 주제로 삼겠다.
