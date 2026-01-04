## 💥 Merge & Conflict
> `Merge`는 Branch를 합치는 작업, `Conflict`는 자동으로 합치지 못하는 상황에서 Git이 사용자에게 선택을 요구하는 신호.

### 1. What is Merge? (3-way Merge의 원리)
> Git은 단순히 두 파일을 비교하지 않고 공통 부모(Base) Commit을 포함한 세 지점을 비교한다.
- **동작 원리** : Base와 비교해 한 쪽만 변경되었다면 자동병합, 양쪽 모두 변경 되었다면 충돌을 발생
- Merge의 종류:
  - ① **Fast-Forward Merge**: 기준 브랜치에 변화가 없을 때, 대상 브랜치 위치로 포인터만 '빨리감기' 하듯 이동시키며, 별도의 커밋이 남지 않음.
  - ② **3-Way Merge (Recursive)**: 브랜치가 분기된 경우, 두 줄기를 합친 새로운 Merge Commit을 생성

### 2. What is Conflict?
> Git의 자동 병합 알고리즘이 판단을 내릴 수 없을 때 발생하며, 사용자가 최종 스냅샷을 결정
- Conflict의 유형:
  - ① **Content Conflict** : 같은 파일의 같은 줄을 서로 다르게 수정했을 때.
  - ② **Add/Add Conflict** : 공통 부모에는 없던 파일을 각 브랜치에서 동일한 경로로 새로 추가했을 때
  - ③ **Delete/Modify Conflict** : 한쪽에서 파일을 삭제하는 동안 다른 쪽에서 수정했을 때

### 3. Conflict Solution
1️⃣ Merge
```bash
git switch main
git merge test
# CONFLICT 발생 확인
```
2️⃣ Solution
파일 내의 마커(`<<<<<<<`, `=======`, `>>>>>>>`)를 직접 수정하거나, 아래 명령어로 브랜치 단위 결정
- **내 브랜치 중심**: `git checkout --ours [file_name]`
- **상대 브랜치 중심**: `git checkout --theirs [file_name]`
  
3️⃣ Finish
충돌 해결 후에는 반드시 수동으로 상태를 확정
```bash
git add [file_name]
git commit -m "merge: resolve conflict using theirs version"
git push origin main
```

### 4. Conflict 해결 전략 요약
|옵션|설명|
|--|--|
|`--ours`|현재 체크아웃된 branch(main) 버전 선택|
|`theirs`|병합 대상 브랜치(test) 버전 선택|
|직접 수정|충돌 표시 마커를 제거하고 수동으로 내용 합치기|
> Tip
> 충돌 파일을 편집 후 `git diff`로 변경 사항 확인 가능, 충돌 해결이 끝나면 반드시 `git add` 후 커밋해야 merge 완료

### 5. Advanced: Maintenance & Sync
#### 5-1. 잘못 병합 시 되돌리기
```bash
# merge commit 되돌리기
# -m 1 : main 브랜치를 기준으로 revert
git revert -m 1 <merge_commit_hash>

# 단순 파일 되돌리기
git checkout HEAD~1 -- [file_name]
```
#### 5-2. 원격(Remote) 상태 확인 및 동기화
| 명령 | 동작|
| -- | -- |
|`git fetch`|원격 브랜치 정보를 가져옴 (내 파일에 영향 없음, 비교용)|
| `git pull` | 원격 브랜치의 최신 커밋을 가져와 로컬 브랜치에 합침 (merge or rebase) |
| `git push` | 로컬 브랜치의 커밋을 원격으로 보냄                             |

```bash
git fetch origin
git log --oneline --graph --decorate --all
git pull origin main
```
> 🌟 **중요** : `pull` 한다고 비교 대상인 파일만 받아지고 로컬에만 있는 파일은 없어지지 않음