# prisma

참고: [prisma quick start](https://www.prisma.io/docs/quickstart/)

팀원분이 신문물을 전파해주셨다.

## 실행 

1. deploy
```
$ cd hello-world
$ prisma deploy
```
2. `prisma.yml` endpoint를 브라우저에서 연다.
3. 쿼리를 날려본다.

```
query getAllUsers {
  users(where : {
    # name_starts_with: "az"
    # name_contains: "o"
  }, orderBy: name_DESC) {
    id
    name
    nickname
  }
}

# mutation: 상태를 바꿔준다.
mutation newUser {
  createUser(data: {
    name: "olaf" 
    nickname: "oo"
  }) {
    id
    name
  }
}
```

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