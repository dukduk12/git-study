## 𖦥 Branch Basics
> **"`Branch` 는 🌟[commit 객체]를 가리키는 이동식 pointer 이다. "**
> 
Git의 Branch는 40글자의 Hash값(commit ID)이 담긴 텍스트 파일이다. 그래서 Branch 생성과 삭제가 빠르다.

---
### 1. Why we use Branch?
프로그래밍은 끊임 없는 실험으로 아래와 같은 이득을 얻을 수 있다.
- **격리성** : 새로운 기능을 만들다가 망쳐도 `main`(원본)은 안전
- **병렬 작업** : 한 명은 버그를 고치고, 한 명은 새 기능을 만들며 동시 작업이 가능
- **기록 보존** : 특정 시점의 실험 기록을 남길 수 있음

---
### 2. Branch Internal
브랜치를 이해하기 위한 핵심 개념은 2가지 이다.
- 1️⃣ `Commit Object`
  - 커밋할 때마다 생성되는 스냅샷
  - 부모 커밋이 누구인지 알고 있음
- 2️⃣ `HEAD` 
  - "현재 checkout 된 브랜치 또는 커밋"을 가리키는 포인터
  - 일반적으로는 브랜치를 가리키며, 이 브랜치가 작업 기준이 됨
  - `switch`을 하면 `HEAD`가 옮겨짐 
---
### 3. Experiment Log
#### 3-1. 과정 요약
1. `main`에서 기준 파일(`state.txt`) 생성 후 커밋
2. `test` 브랜치 생성 (초기에는 `main`과 동일 지점)
3. `test` 브랜치로 이동 후 파일 수정 및 커밋
4. 커밋 그래프 분기 확인 및 Remote Push 

#### 3-2. 상세 명령어
|단계|명령어|설명|
|--|--|--|
|기초|`git branch test`|`test`라는 이름의 포인터를 현재 커밋에 생성|
|이동|`git switch test`|`HEAD`를 `test`로 옮겨 작업 환경 변경|
|확인|`git branch -a`|로컬과 원격의 모든 브랜치 상태 조회|
|연결|`git push -u origin test`|원격에 브랜치 생성 및 Upstream(추적) 설정|

#### 3-3. 시각적 변화 확인 (`git log --graph --all`)
```Plaintext
* c29ff37 (HEAD -> test, origin/test) 01: update state.txt on test branch
* 96e29e3 (origin/main, main) init: base state for git experiments
```
- **HEAD -> test**: 현재 내가 `test` 브랜치에 있고, 이 브랜치가 최신 커밋을 가리킴.
- **main**: 아직 96e29e3 커밋에 멈춰 있음. (분기 발생!)

---
### 4. Insight
- **Upstream의 중요성**: `-u` 옵션은 로컬 브랜치에 기본 동기화 대상(remote branch)을 설정함. 이후 `git pull`, `git push`는 이 upstream 브랜치를 기준으로 자동 동작함.

- **좌우 비교 시각화** :
    |구분|Local Branch|Remote (Origin)|
    |--|--|--|
    |Main|96e29e3 (Base)|96e29e3 (Synced)|
    |Test|c29ff37 (New!)|c29ff37 (Synced)|

- **용어 정리** :
    ```
                    (tracking)
    test ───────────────────▶ origin/test
    ↑
    HEAD
    ```
    |요소|역할|
    |--|--|
    |HEAD|내가 지금 작업 중인 위치|
    |test|로컬 브랜치 포인터|
    |origin/test|remote 브랜치 포인터|
    |upstream|test가 추적하는 origin/test|

---
### 5. `git push`와 브랜치 전송의 오해
#### 5-1. `HEAD`와 `push`는 별개로 움직인다.
- Git에서 `push`는 **현재 내가 서 있는 브랜치(HEAD)**를 기준으로 동작하지 않는다.
- 대신, **명시한 브랜치 이름**을 기준으로 동작한다.
- 즉, 하단의 코드는 
    ```
    git push origin main
    ```
    “로컬 `main` 브랜치를 remote의 `main` 브랜치로 전송하라”는 의미이다.
- 이 명령은 현재 HEAD에 상관없이 항상 로컬 `main`만을 기준으로 처리된다.

#### 5-2. `test` 브랜치에 있는 상태에서 `git push origin main`을 실행하면?
현재 상태가 다음과 같다고 가정하자.
```
HEAD
 ↓
test  ─────────▶ c29ff37
|
main ─────────▶ 96e29e3
```
이 상태에서:
```
git push origin main
```
Git의 실제 동작:
- 로컬 `main` → 커밋 `96e29e3`
- remote `main` → 커밋 `96e29e3`
- 변경 사항 없음 → 아무 일도 일어나지 않음
👉 `test` 브랜치의 커밋(`c29ff37`)은
remote `main`으로 전송되지 않는다.

#### 5-4. `test`의 작업 내용을 `main`으로 반영하려면?
`push`가 아니라 병합(merge) 이 필요하다.

#### 5-5. `-u (upstream)` 옵션과의 관계
```
git push -u origin test
````
- 로컬 `test` 브랜치를 `origin/test`와 연결
- 이후 `git pull`, `git push` 시 기본 대상이 됨

> ⚠️ `-u` 옵션은
> ** “어떤 브랜치를 보낼지”를 결정하지 않는다.**
> 
> 단지 “앞으로 기본 동기화 기준을 무엇으로 할지”를 설정할 뿐이다.