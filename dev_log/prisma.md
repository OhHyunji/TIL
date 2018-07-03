# prisma

참고: [prisma quick start](https://www.prisma.io/docs/quickstart/)

팀원분이 신문물을 전파해주셨다.

## schema

hello-world 들어가서 `.graphql` 열어보면 스키마가 있다.

```
type User {
  id: ID! @unique
  name: String!
  nickname: String
}
```
- ! 붙으면 required, 안붙으면 optional

## graphql

- sql은 모든 쿼리 다 날릴 수 있는데, graphql은 서버에서 허용한 쿼리만 날릴 수 있다.
-  graphql 공부하고싶으면 [how to graphql](https://www.howtographql.com/)을 보시라!