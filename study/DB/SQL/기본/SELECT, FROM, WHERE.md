
### 1. SELECT

- **정의**: 데이터베이스 테이블에서 조회할 **열(Column)** 을 지정.
- **역할**: 원하는 데이터의 특정 속성(열)을 선택해 출력.
- **문법**:

```sql
SELECT column1, column2, ... | * FROM table_name;
```
- `column1, column2, ...`: 조회할 열 이름.
- `*`: 모든 열 조회.

```sql
SELECT name, email FROM customers;
```
- `customers` 테이블에서 `name`과 `email` 열을 조회.

```sql
SELECT * FROM customers;
```
- `customers` 테이블의 모든 열을 조회.

### 2. FROM

- **정의**: 데이터를 조회할 **테이블**을 지정.
- **역할**: 데이터의 출처(테이블)를 명시.
- **문법**:

```sql
SELECT column_name FROM table_name;
```
- `table_name`: 조회 대상 테이블.

```sql
SELECT order_id, order_date FROM orders;
```
- `orders` 테이블에서 `order_id`와 `order_date` 열을 조회.

- **참고**: 여러 테이블을 조인(JOIN)할 때 FROM 절에 다수 테이블 명시 가능.
```sql
SELECT customers.name, orders.order_date 
FROM customers 
JOIN orders ON customers.customer_id = orders.customer_id;
```

### 3. WHERE

- **정의**: 조회할 데이터의 **조건**을 지정.
- **역할**: 특정 조건을 만족하는 행(Row)만 필터링해 출력.
- **문법**:

```sql
SELECT column_name FROM table_name WHERE condition;
```
- `condition`: 필터링 조건(예: `age > 30`, `status = 'active'`)