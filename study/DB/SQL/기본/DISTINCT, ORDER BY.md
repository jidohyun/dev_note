
### 1. DISTINICT
- **정의**: 조회 결과에서 **중복된 행**을 제거하여 고유한 값만 반환.
- **역할**: 동일한 데이터가 여러 번 나타나지 않도록 필터링.
- **문법**:
```sql
SELECT DISTINCT column1, column2, ... FROM table_name;
```
- `DISTINCT`는 지정된 열(들)의 조합에서 중복 제거.
- 단일 열 또는 여러 열에 적용 가능.

- **예시**:
```sql
SELECT DISTINCT city FROM customers;
```
- `customers` 테이블에서 `city`열의 고유한 값만 조회 (예: 'Seoul', 'Busan'만 반환).

```sql
SELECT DISTINCT name, age FROM customers;
```
- `name`과 `age`의 조합이 고유한 행만 반환 (동일한 이름과 나이 쌍은 하나로).

- **주의점**
	- `DISTINCT`는 모든 지정된 열의 조합에 적용 (단일 열이 아닌 경우)
	- 성능 저하 가능: 대규모 데이터에서 중복 제거라는 리소스 소모.
	- `NULL`은 하나의 고유 값으로 간주.

### 2. ORDER BY
- **정의**: 조회 결과를 특정 열 기준으로 **정렬**
- **역할**: 데이터를 오름차순(ACS) 또는 내림차순(DESC)으로 정렬해 가독성/분석 용이성 제공.
- **문법**:
```sql
SELECT column1, column2, ... FROM table_name ORDER BY column_name [ASC | DESC];
```
- `ASC`: 오름차순 (기본값, 생략 가능).
- `DESC`: 내림차순.
- 여러 열 정렬 가능: `ORDER BY column1 ASC, column2 DESC`.

- **예시**:
```sql
SELECT name, age FROM customers ORDER BY age ASC;
```
- `customers` 테이블에서 `name`, `age`를 나이순(오름차순)으로 정렬.

```sql
SELECT product_name, price FROM products ORDER BY price DESC, product_name ASC;
```
- `products` 테이블에서 `price`를 내림차순, 같은 `price`일 경우 `product_name`을 오름차순으로 정렬.

- **주의점**
	- 정렬 기준 열은 `SELECT`에 없어도 사용 가능(예: `SELECT name FROM customers ORDER BY age;`).
	- `NULL` 은 RDBMS마다 다르게 처리 (MySQL/PostgreSQL: 오름차순 시 `NULL`이 먼저).
	- 인덱스 활용 여부에 따라 성능 차이.

- **종합 예시**

```sql
SELECT DISTINCT city 
FROM customers 
WHERE age > 25 
ORDER BY city ASC;
```
- **설명**: `customers` 테이블에서 `age`가 25 초과인 고객의 고유한 `city`를 조회하고, 알파벳순(오름차순)으로 정렬.

### 주요 특징 및 주의점
- **DISTINCT**:
	- 중복 제거 대상이 많을수록 쿼리 실행 시간 증가.
	- `GROUP BY`로 대체 가능 경우 있음 (예: 집계 없이 고유 값 조회)
	- `SELECT DISTINCT *`는 모든 열의 중복 제거 -> 비효율적 사용 주의.
- **ORDER BY**:
	- 다중 열 정렬 시 우선순위는 왼쪽 열 기준
	- 열 번호로도 정렬 가능 (예: `ORDER BY 2` -> 두 번째 열 기준), 하지만 가독성 저하로 비권장.
	- 대규모 데이터 정렬 시 인덱스 활용으로 성능 최적화 가능.
