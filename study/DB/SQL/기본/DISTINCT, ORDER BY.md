
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
- `cust`