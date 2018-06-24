# SQL Cheat Sheet

## 기본

```
SELECT *
FROM
WHERE
LIMIT
ORDER clum_name DESC/ACE
GROUP_BY
```

## 두 테이블 연결: JOIN

```
FROM items JOIN brands ON items.brand_id = brands.id
```

- Inner Join: 교집합
- Outer Join: 합집합 
- Left: FROM 자리에 있는 테이블
- Right: ON 자리에 있는 테이블

## 특정 단어가 포함된 레코드 검색하기: LIKE

```
SELECT * FROM brands 
WHERE name LIKE '%메트로시티%'

# '메트로'로 시작하는
SELECT * FROM brands WHERE name LIKE '메트로%'

# '시티'로 끝나는
SELECT * FROM brands WHERE name LIKE '%시티'
```

## 이 쿼리가 인덱스 타는지 확인하기: EXPLAIN

```
EXPLAIN SELECT * FROM brands WHERE name LIKE '메트로시티%'
```
- SELECT, DELETE, INSERT, REPLACE, UPDATE

