## 🖇Connect to Existing Remote
### 🚀 전체 실행 프로세스
#### Method 1
```bash
# 1. Git 초기화 및 원격 저장소 연결
git init
git remote add origin https://github.com/yourusername/docs.git
git remote -v
git pull origin main --allow-unrelated-histories

# 2. 파일 추가 및 로컬 커밋
git add .
git commit -m "docs: add rebase practice folder"

# 3. 브랜치 설정 및 원격 전송 (Push)
git branch -M main
git push -u origin main
```

#### Method 2
```bash
# 1. 원격 저장소를 다시 내 컴퓨터로 가져오기
git clone https://github.com/yourusername/docs.git
cd docs

# 2. 03 폴더 만들기
mkdir 03-rebase-practice

# 3. 추가 및 푸시
git add .
git commit -m "docs: add 03 practice"
git push origin main
```
---
### ⚠️ **중요**: 만약 Push가 실패한다면? (Rejected 오류)
원격 저장소에 이미 `README.md`나 다른 파일이 존재할 경우, 로컬의 내역과 충돌하여 push가 거부될 수 있음. 
- **원격 내용 먼저 가져오기**: `git pull origin main`
- **이력이 달라도 합치기 허용**: `git pull origin main --allow-unrelated-histories`
- **충돌 해결 후 다시 Push**: 파일 내 `<<<< HEAD` 같은 표시가 있다면 수정 후 `add` -> `commit` -> `push`
---
### 💡 **꿀팁**: 안 꼬이고 협업하는 습관
- 작업 시작 전 `pull`: 작업 시작하기 전 항상 `git pull origin main`을 실행
- 브랜치 활용 : 다른 작업 시 `git checkout -b [new_branch_name]`으로 작업 후 `push`