
**1. os.path.join** 

- 여러 문자열(폴더/파일명)을 OS에 맞는 경로 구분자로 합쳐줌
- 윈도우: ‎`\` / 리눅스, 맥: ‎`/` 자동 처리
- 경로를 직접 문자열로 합치지 말고 항상 ‎`os.path.join` 사용

**2. os.makedirs** 

- 여러 단계의 하위 폴더까지 한 번에 생성
- 이미 폴더가 있으면 에러 없이 넘어감 (‎`exist_ok=True` 옵션)
- ‎`os.mkdir`는 한 단계 폴더만 생성, 이미 있으면 에러 발생

**3. os.path.basename / os.path.dirname** 

- ‎`basename`: 경로에서 파일명만 추출
- ‎`dirname`: 경로에서 폴더 경로만 추출
- 둘 다 필요하면 ‎`os.path.split` 사용 (튜플로 반환: (폴더, 파일명))

**4. os.path.exists / os.path.isfile / os.path.isdir** 

- ‎`exists`: 경로(파일/폴더)가 존재하는지 확인
- ‎`isfile`: 경로가 파일인지 확인
- ‎`isdir`: 경로가 폴더인지 확인

**예시 코드**

```python
import os

# 경로 합치기
path = os.path.join("folder1", "folder2", "file.txt")

# 폴더 생성 (여러 단계, 이미 있으면 에러 없음)
os.makedirs("folder1/folder2", exist_ok=True)

# 파일명/폴더명 추출
filename = os.path.basename(path)
dirname = os.path.dirname(path)
folder, file = os.path.split(path)

# 존재 여부 및 타입 확인
os.path.exists(path)
os.path.isfile(path)
os.path.isdir(dirname)
```

