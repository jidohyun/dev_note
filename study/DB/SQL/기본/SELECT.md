
### 정의

데이터베이스 테이블에서 조회할 **열(Column)**을 지정.

### 역할

원하는 데이터의 특정 속성(열)을 선택해 출력

### 문법

```sql
SELECT column1, column2, ... | * FROM table_name;
```
- `column1, column2, ...`: 조회할 열 이름.
- `*`: 모든 열 조회.

- 예시:

```sql
SELECT name, email FROM customers;
```
- `customers` 테이블에서 `name`과 `email`열을 조회.

```sql
SELECT * FROM customers;
```
- `customers` 