## 📖 Commit Guide
### 1. Commit Message 작성 원칙 
> Commit은 단순 기록이 아니라 **변경 이력을 탐색·복구·리뷰하기 위한 인터페이스**
> 따라서 일관된 형식과 의미 전달이 핵심
---
### 2. Commit Message Format
`<type>(<scope>): <subject>`
  - **type**: 변경의 성격
  - **scope**: 변경 범위 (optional)
  - **subject** : 변경 내용 요약 (50자 내외)
  - **Body**(optional): 변경 이유, 배경, 설계 판단
  - **Footer**(optional): 이슈 번호, breaking change 등
---
### 3. Commit Type
| Type | 의미 | 사용 예시 |
|---|---|---|
| feat | 새로운 기능 추가 | `feat: add Google login` |
| fix | 버그 / 오류 수정 | `fix: resolve image rendering issue` |
| docs | 문서 수정 | `docs: add PR guideline` |
| refactor | 기능 변경 없는 코드 개선 | `refactor: remove duplicated logic` |
| chore | 설정, 빌드, 의존성 관리 | `chore: update gitignore` |
| test | 테스트 코드 추가/수정 | `test: add merge conflict test` |
---
### 4. Commit Message 작성 규칙 (Convention)
- **명령문 / 현재형 사용**
  - ❌ `Fixed login bug`
  - ✅ `Fix login bug`

- **마침표 사용 금지**
  - ❌ `Add README.`
  - ✅ `Add README`

- **영문 기준 첫 글자 대문자 권장**
  - `Add`, `Fix`, `Update`

- **의미 없는 커밋 금지**
  - ❌ `update`
  - ❌ `fix bug`
  - ✅ `fix: handle null response in auth flow`
  
---
### 5. 좋은 Commit의 기준

- 하나의 커밋 = 하나의 논리적 변경
- 되돌리기(`revert`) 가능한 단위
- 커밋 메시지만 보고 **무엇이 왜 바뀌었는지 추론 가능**
---