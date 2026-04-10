# 오픈소스기초 1분반 32244791 한예준 Git활용 과제 제출

## 기본 정보

| 항목 | 내용 |
|------|------|
| 과목 | 오픈소스SW기초 1분반|
| 교수님 | 송인식 교수님 |
| 학과 | 모바일시스템공학과 |
| 학번 | 32244791 |
| 이름 | 한예준 |

---

## 1. 프로젝트 개요

본 프로젝트는 간단한 Python 기반 계산기 프로그램을 구현하면서, Git의 주요 기능인 브랜치(branch), 커밋(commit), 병합(merge) 과정을 실습하는 것을 목표로 한다.

### 프로젝트 구조
git_basic/
├── src/
│ ├── calculator.py  # 사칙연산 함수 구현
│ └── main.py        # 프로그램 실행 파일
└── docs/
└── README.md        # 프로젝트 설명 문서

- 디렉토리 구성: `src/`, `docs/`
- 주요 파일: `calculator.py`, `main.py`, `README.md`

### 원격 저장소

- GitHub Repository  
  > https://github.com/gksdPwns1/git-report-opensource

---

## 2. 작업 내역

### 2-1. 초기 설정 및 커밋

프로젝트 초기 환경을 구성하고 기본 파일을 생성한 후 첫 커밋을 수행하였다.

```bash
git init
git branch -m main
git remote add origin https://github.com/gksdPwns1/git-report-opensource.git

git add .gitignore src/calculator.py src/main.py docs/README.md
git commit -m "init: 프로젝트 초기 구조 설정"

---

### 2-2. Fast-Forward Merge

Fast-Forward Merge는 기준 브랜치(main)에 새로운 커밋이 없는 경우, 브랜치 포인터만 이동하여 병합되는 방식이다.

git checkout -b feature/multiply
# 곱셈 함수 추가
git add src/calculator.py
git commit -m "feat: 곱셈 함수 추가"

git checkout main
git merge feature/multiply
 > 결과 : Fast-forward
         src/calculator.py | 3 +++
 > 별도의 merge commit이 생성되지 않았으며 단순히 브랜치 포인터가 앞으로 이동하였다.

---

### 2-3. 3-Way-Merge
 > 두 브랜치가 각각 변경된 이력이 있을 경우, 공통 조상을 기준으로 병합이 수행되며 새로운 merge commit이 생성된다.
```bash
git checkout -b feature/divide
# 나눗셈 함수 추가
git add src/calculator.py
git commit -m "feat: 나눗셈 함수 추가"

git checkout main
# main에서도 별도 변경 발생
git add src/main.py
git commit -m "feat: main.py에 곱셈 출력 추가"

git merge feature/divide -m "merge: feature/divide 병합"

**결과**
Merge made by the 'ort' strategy.
 src/calculator.py | 5 +++++

### 2-4. 충돌해결(Conflict Merge)
> 양쪽 브랜치가 같은 파일의 같은 부분을 수정하면 충돌이 발생한다.

```bash
git checkout -b feature/update-readme
# docs/README.md 기능 목록을 영문 함수명 포함으로 수정
git add docs/README.md
git commit -m "docs: README 기능 목록 업데이트"

git checkout main
# docs/README.md 기능 목록을 한글로 수정 (같은 부분을 다르게 수정)
git add docs/README.md
git commit -m "docs: README 기능 목록 한글로 변경"

git merge feature/update-readme
```

**충돌 발생:**

```
자동 병합: docs/README.md
충돌 (내용): docs/README.md에 병합 충돌
자동 병합이 실패했습니다. 충돌을 바로잡고 결과물을 커밋하십시오.
```

**충돌 내용:**

```
<<<<<<< HEAD
## 주요 기능
- 더하기
- 빼기
- 곱하기
- 나누기
=======
## 기능
- 덧셈 (add)
- 뺄셈 (subtract)
- 곱셈 (multiply)
- 나눗셈 (divide)
>>>>>>> feature/update-readme
```

**해결:** 양쪽의 장점을 합쳐 한글 제목 + 영문 함수명으로 통합했다.

```markdown
## 주요 기능
- 더하기 (add)
- 빼기 (subtract)
- 곱하기 (multiply)
- 나누기 (divide)
```

```bash
git add docs/README.md
git commit -m "fix: README 충돌 해결 (한글 제목 + 영문 함수명 병합)"
```

---

## 3. 원격 저장소 반영

```bash
git push -u origin main
```

모든 작업 내역을 GitHub 원격 저장소에 push했다.

---

## 4. 최종 커밋 히스토리

```
*   87efbc4 fix: README 충돌 해결 (한글 제목 + 영문 함수명 병합)
|
v   c0d995e docs: README 기능 목록 업데이트
*   c0c4fb4 docs: README 기능 목록 한글로 변경
v  
*   1f746f9 merge: feature/divide 3-way 병합
|  
v   77eb2a7 feat: 나눗셈 함수 추가
*   79d7ce0 feat: main.py에 곱셈 출력 추가
v
*   4610cd7 feat: 곱셈 함수 추가
*   bad82a1 init: 프로젝트 초기 구조 설정
```

| 시나리오 | 브랜치 | 결과 |
|----------|--------|------|
| Fast-Forward | `feature/multiply` | 포인터만 이동, merge commit 없음 |
| 3-Way Merge | `feature/divide` | 자동 병합, merge commit 생성 |
| Conflict Merge | `feature/update-readme` | 충돌 발생 → 수동 해결 후 커밋 |
