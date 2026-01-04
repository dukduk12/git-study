## 📖 PR Guide
### 1. What is PR?
> Pull Request(PR)는 작업한 브랜치의 변경 사항을 다른 브랜치  
> (주로 `main` 또는 `develop`)에 병합하기 전에 **검토(review)를 요청하는 공식 절차**

PR은 단순한 “병합 요청”이 아니라,
**Issue에서 정의한 요구사항을 코드로 어떻게 해결했는지 설명하는 문서**이다.

- 코드 품질 검증
- 변경 의도 공유
- 협업 중 사고(실수, 충돌) 방지
- 기록 기반 의사결정

---
### 2. PR 작성 가이드
#### 2-1. PR 제목 (Title)
- **Commit Message Convention을 그대로 사용**
- 형식: `type: subject`
- PR 하나 = **하나의 Issue / 하나의 목적**

**예시**
- `feat: integrate login API`
- `docs: add PR guide documentation`
- `fix: resolve merge conflict on README`
---
#### 2-2. PR 본문 (Description)
> 리뷰어가 **모든 코드를 열지 않아도 맥락을 이해할 수 있도록** 작성한다.  
> PR 본문은 “이 Issue를 어떻게 해결했는가”에 집중한다.

##### 📌 PR Description Template
| 항목 | 설명 |
|---|---|
| Summary | 어떤 작업을 했는지 1~2줄 요약 |
| Key Changes | 주요 변경 사항 (파일 / 로직 단위) |
| Context / Why | 왜 이 작업이 필요한지 (이슈, 배경) |
| Test | 테스트 여부, 결과, 스크린샷(UI 변경 시 필수) |
| To Reviewers | 리뷰 시 중점적으로 봐줬으면 하는 부분 |
---
### 3. 실무 권장 협업 워크플로우 (Git-Flow 기반)
> 사고를 방지하고 일관된 이력을 남기기 위한 표준 순서 
1. **Issue 생성**: 해야 할 일을 Issue에 등록하고 번호(`ex: #12`)를 부여받음.
2. **Branch 생성**: `feature/issue-번호` 형식으로 브랜치를 만듦.
   - `git checkout -b feature/login-api`
3. **작업 및 Push**: 작업을 커밋 단위로 쪼개어 기록하고 원격에 올림.
    - `git push origin feature/login-api`
4. **PR 생성**: GitHub에서 PR을 생성함. 본문에 `Closes #12`를 적으면 병합 시 이슈가 자동 종료됨.
5. **Review & Feedback**: 리뷰어의 피드백을 반영해 추가 커밋을 하고 다시 `push` 함. (PR은 자동 갱신됩니다.)
6. **Merge**: 모든 리뷰가 승인되면 `main`에 병합(Merge)하고, 사용한 브랜치는 삭제.
---
### 4. PR 전 체크리스트
- [ ] 불필요한 주석이나 테스트용 코드(`print`, `console.log`)를 지웠는가?
- [ ] 이번 PR의 목적과 상관없는 파일이 포함되지는 않았는가?
- [ ] 새로 추가된 함수나 복잡한 로직에 설명이 충분한가?
- [ ] 변경 사항으로 인해 기존 기능이 깨지지 않는지 확인했는가?
- [ ] Issue 요구사항을 모두 충족했는가?
---
### [보충설명] : Issue ↔ PR 관계
> PR은 Issue에 적힌 내용을 “구현 결과로 증명”하는 단계

- **Issue**: 무엇을 / 왜 해야 하는가? (기획/문제 정의)
- **PR**: 그 Issue를 어떻게 해결했는가? (구현/해결책)

#### 💡 핵심 원칙
- **1 PR = 1 Issue**
- **Issue 없는 PR 허용 범위**:
  - 단순 문서 수정 (`docs`)
  - 기능 변화 없는 리팩터링 (`refactor`)
  - 설정 변경 및 의존성 관리 (`chore`)
- **자동 연동**: PR 본문에 `Closes #이슈번호` 포함 시 병합과 동시에 이슈가 자동 종료(Closed)
---