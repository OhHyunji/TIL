# kafka, zipkin, ES

## kafka

- 비동기 처리를 위한 메시징 큐의 한 종류다. 
- 프로듀서, 컨슈머가 있다.
- apach에서 스칼라로 만들었다.
- 대표적인 비동기 메시징 시스템인 메일과 비교하면 이해가 쉬울 것 같다.

```
메일: Sender - Mail Server - Receiver
카프카: Producer - Kafka Server - Consumer
```

참고: [kafka 운영자가 말하는 처음 접하는 kafka](https://www.popit.kr/kafka-%EC%9A%B4%EC%98%81%EC%9E%90%EA%B0%80-%EB%A7%90%ED%95%98%EB%8A%94-%EC%B2%98%EC%9D%8C-%EC%A0%91%ED%95%98%EB%8A%94-kafka/)

## zipkin

- zipkin은 tracing system이다.
- microservice architectures에서 api 호출 -> 호출 -> ... timing data를 ui로 볼 수 있다. 

아래는 원문.

> - Zipkin is a distributed tracing system.
> - It helps gather timing data needed to troubleshoot latency problems in microservice architectures.
> - Applications are instrumented to report timing data to Zipkin.

참고: [Zipkin](https://zipkin.io/)

## Elasticsearch

- Elasticsearch는 확장성이 뛰어난 오픈소스 풀텍스트 검색 및 분석엔진이다.
- 방대한 양의 데이터를 신속하게, 거의 실시간으로 **저장, 검색, 분석**할 수 있도록 지원한다.

사용예

> - 온라인 웹스토어: ES를 사용해서 전체 제품 카탈로그 및 재고 정보를 저장하고, 그에 대한 검색 및 자동완성 제안 기능을 제공할 수 있다.
> - 로그, 트랜잭션 데이터를 수집, 분석, 마이닝해서 -> 추이, 통계, 요약정보를 얻을 수 있다.
> - 분석이 필요한 방대한 데이터를 신속하게 조사, 분석, 시각화, 임시Query를 수행하고 싶은 경우: Elasticsearch를 사용하여 데이터를 저장한 다음 -> Kibana를 사용하여 대시보드를 만들 수 있다.

참고: [Elasticsearch 설명서](https://www.elastic.co/guide/kr/elasticsearch/reference/current/getting-started.html)