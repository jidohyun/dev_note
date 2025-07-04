
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
- **연산자**: 
	- 비교: `=`, `!-`, `>`, `<`, `>=`, `<=`
	- 논리: `AND`, `OR`, `NOT`
	- 기타: `LIKE`, `IN`, `BETWEEN`, `IS NULL`

- **예시**:
```sql
SELECT name, age FROM customers WHERE age > 30;
```
- `customers` 테이블에서 `age`가 30 초과인 고객의 `name`과 `age` 조회.

```sql
SELECT product_name FROM products WHERE price BETWEEN 10 AND 50
```
- `products` 테이블에서 `price`가 10~50 사이인 제품의 `product_name` 조회.

```sql
SELECT email FROM users WHERE status = 'active' AND city = 'Seoul';
```
- `users` 테이블에서 `status`가 'active'이고 `city`가 'Seoul'인 사용자의 `email` 조회.

- **종합 예시**

```sql
SELECT customer_id, name, email 
FROM customers 
WHERE city = 'Seoul' AND age >= 25;
```

### 주요 특징 및 주의점

- **SELECT**:
	- 열 이름은 명시적으로 작성하는 것이 가독성에 좋음. `*`는 간편하지만, 불필요한 데이터 조회 시 성능 저하 가능.
	- 별칭(Alias) 사용 가능: `SELECT name AS customer_name`
- **FROM**: 
	- 테이블 이름은 스키마 명시 가능(예: `database_name.table_name`).
	- 잘못된 테이블 이름은 오류 발생.
- WHERE: 
	- 조건 순서에 따라 성능 차이 가능(인덱스 활용 고려).
	- 문자열은 단일 따옴표(`'`)로 감싸야 함(예: `'Seoul'`).
	- `NULL` 값은 `IS NULL` 또는 `IS NOT NULL`로 확인.