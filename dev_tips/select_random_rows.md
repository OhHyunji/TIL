# Select random rows

api 성능테스트를 위해 스크립트를 짜야했는데,
DB에서 몇개의 Sample 데이터를 뽑아서 돌려봐야 하는 내용이었다.

## 방법1. DB에서 랜덤으로 Select 해오자.

```sql
SELECT * FROM table_name ORDER BY RAND() LIMIT 100;
``` 
개발용DB에서는 랜덤으로 데이터를 잘 가져오는 것을 확인했는데, 실DB에 바로 돌리다가는 DB팀에서 연락올것같아서  (무서워서ㅠㅠ)`ORDER BY RAND()`에 대해 찾아보았다.

### ORDER BY RAND()

위 쿼리를 실행하면 아래 순서로 처리된다.

1. The MySQL Database will query for all the rows you requested 
2. and filter out those that don’t match from WHERE, JOIN conditions etc.
3. **all rows** will be in memory or in a temporary table.
4. Then the RAND() will be replaced with a random number and attached to each row.
5. After this, the set can be sorted ascending or descending.
6. And then the LIMIT will be used.

참고: [mysql-order-by-rand-a-case-study-of-alternative](http://www.roberthartung.de/mysql-order-by-rand-a-case-study-of-alternatives/)

all rows에 대해 랜덤값을 주고 그것을 다시 sorting 한다..? 
내가 조회하려는 테이블에 데이터 엄청 많은데...;

## 방법2. 스크립트로 랜덤 ids를 만들자.

이 생각이 들자마자, 왜 이생각을 못했지? 했다!


일단 이렇게 해서 ids_set을 위한 맨 처음, 맨 마지막을 알아내고
_(200~3000이라 가정)_

```sql
SELECT * FROM table_name ORDER BY Id DESC limit 1;
SELECT * FROM table_name limit 1;
```

python 스크립트를 만들었다.

```python
import random

ids_set = range(200, 3000)
count = random.randrange(1, 100)
ids = random.sample(ids_set, count)

# call api 
url = "api?ids=%s"%ids

# 뽀너스. python에서 string 사이에 변수 넣기
print("ids_set=%s, count=%s"%(id_set, count))
```

참고: [select-random-item-list-tuple-data-structure-python](https://www.pythoncentral.io/select-random-item-list-tuple-data-structure-python/)

참고: [pythonspot::random](https://pythonspot.com/random/)


