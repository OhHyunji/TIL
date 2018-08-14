# Entity modified history

TIL

```
Entity에 대한 modified history 이력을 db에 남기고싶다.
=> Entity 클래스에 @Audited 붙이고
=> 설정 조금 하면 끝!
```

테이블 설계할 때 항상 필요한게 createdAt, modifiedAt, creator 등등 컬럼이다.
거의 모든 테이블에 default로 들어있고 중요한 정보다.

- 보일러 플레이트!
- spring-data-jpa에 이걸 자동으로 해주는 기능이 있다.

Entity 클래스에 `@Audited` 어노테이션을 붙이고 이 이력이 쌓일 테이블을 만들어두면 알아서 Entity에 대한 변경이력이 쌓인다.

참고: [Spring Boot + Envers로 엔티티 이력관리하기](http://haviyj.tistory.com/40)

참고: [spring-data-jpa Auditing을 추가해보자](http://blusky10.tistory.com/316)

참고: [권남 Spring Data Jpa Auditing](http://kwonnam.pe.kr/wiki/java/jpa/springdatajpa/audit)