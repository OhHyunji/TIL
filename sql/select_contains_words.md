# LIKE

특정 단어가 포함된 레코드 검색하기 

흔히 쓰는 WHERE절

```sql
SELECT * FROM brands
WHERE name='메트로시티'

# 메트로시티
```

LIKE를 사용하면

```sql
SELECT * FROM brands 
WHERE name LIKE '%메트로시티%'

# 메트로시티, 메트로시티(시계), 메트로시티(주얼리)
```

좀 더 응용해보면

```sql
# '메트로'로 시작하는
SELECT * FROM brands WHERE name LIKE '메트로%'

# '시티'로 끝나는
SELECT * FROM brands WHERE name LIKE '%시티'

# '메트로시티'가 들어가는
SELECT * FROM brands WHERE name LIKE '%메트로시티%'

# name, category 필드에 적용
SELECT * FROM brands WHERE name LIKE '%메트로시티%' AND category LIKE '%패션%'
```

그러나, LIKE는 인덱스를 안타서 쿼리 성능이 좋지 않다.

- '메트로%' 이런건 인덱스 타는데,
- '%시티' 이거는 인덱스안타고 full scan 한다.

> 집에 꽂혀있는.. SQL책좀 봐야겠다..!