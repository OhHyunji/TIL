# Explain

이 쿼리가 index 타는지 확인하기

- SELECT, DELETE, INSERT, REPLACE, UPDATE

```sql
EXPLAIN SELECT * FROM brands WHERE name LIKE '메트로시티%'
```

- type: ALL이면 index 타지 않는것.
- key: index 걸려있는 key

참고: [MySQL - explain output](https://dev.mysql.com/doc/refman/8.0/en/explain-output.html)