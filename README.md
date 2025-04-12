# 📦 Git 자동 커밋/푸시 스크립트

## 📌 소개

이 스크립트는 Git 커밋 및 푸시 과정을 자동화하며, 사용자가 겪을 수 있는 다양한 오류를 사전에 점검하거나 발생 시 친절하게 해결 방법을 안내해주는 도구입니다.

처음 GitHub를 사용할 때 영어 에러 메시지로 인해 당황했던 경험에서 출발하여, **누구나 쉽게 Git을 사용할 수 있도록** 개발된 스크립트입니다.

---

## ✨ 주요 기능

- 📍 현재 브랜치, OS 정보, KST 기준 시간 표시
- 📋 변경된 파일 목록 확인
- 🔍 사전 점검: 병합 충돌, 사용자 정보 누락, lock 파일 유무, Git 레포지토리 여부, 원격 저장소 접근 가능 여부 등
- 🚫 커밋/푸시 단계 오류 감지 및 안내
- 🎨 친절하고 직관적인 컬러 UI 및 이모지 출력
- 💡 각 오류별 해결 가이드 제공

---

## 🛠️ 사용법

```bash
# 방법 1: bash 명령어로 실행
bash auto_git.sh

# 방법 2: 실행 권한 부여 후 실행
chmod +x auto_git.sh
./auto_git.sh
```

스크립트 실행 시 화면을 정리(clear)한 뒤, 깔끔하게 결과를 안내합니다.

---

## 🔍 주요 체크 항목 (사전 점검)

- ✅ Git 레포지토리인지 여부
- ✅ 병합 충돌 상태 확인
- ✅ Git 사용자 정보 설정 여부
- ✅ .git/index.lock 파일 존재 여부
- ✅ 변경 사항 존재 여부
- ✅ 원격 저장소(remote.origin.url) 설정 및 연결 가능 여부

---

## 📦 커밋 메시지 포맷

```bash
🚀 [브랜치명] automatic commit | yyyy-mm-dd hh:mm (KST) | OS 정보
```

예: `🚀 [main] automatic commit | 2025-04-12 22:55 (KST) | macOS`

---

## ❗ 오류 발생 시 예외 처리 방식

오류 발생 시 화면을 컬러로 나누고 이모지와 함께 문제 원인을 친절하게 출력합니다.각 오류에 대해 가능한 해결 방법을 단계별로 안내하여, 초보자도 빠르게 문제를 해결할 수 있습니다.

---

## 🧭 내부 동작 순서 (로직 요약)

1. **터미널 정리 (clear)**
2. 현재 시간, 브랜치, 운영체제 확인
3. 변경 사항 있는지 확인
4. 사전 점검 (충돌, 설정 누락 등)
5. 커밋 메시지 생성 및 커밋 시도
6. 에러 발생 시 메시지 출력 및 해결 가이드 안내
7. 원격 저장소 연결 확인
8. 푸시 시도 및 에러 대응
9. 성공 시 축하 메시지 출력

---

## 🐛 오류 목록 정리 (자동화로 방지 가능한지 포함)

📦 1️⃣ git add 단계

- 에러 메시지: fatal: pathspec 'xxx' did not match any files  
  ↳ 원인: 파일명 오타 또는 해당 경로에 파일 없음  
  ↳ 자동화 방지 가능: ✅ git add . 로 커버 가능

- 에러 메시지: fatal: Not a git repository  
  ↳ 원인: .git 폴더 없음 (Git 초기화되지 않은 디렉토리)  
  ↳ 자동화 방지 가능: ✅ 스크립트 시작 시 체크 가능

📦 2️⃣ git commit 단계

- 에러 메시지: nothing to commit, working tree clean  
  ↳ 원인: 변경된 파일이 없음  
  ↳ 자동화 방지 가능: ✅ 커밋 전에 상태 확인

- 에러 메시지: Aborting commit due to empty commit message.  
  ↳ 원인: 커밋 메시지가 비어 있음  
  ↳ 자동화 방지 가능: ✅ 자동 메시지 삽입

- 에러 메시지: Committing is not possible because you have unmerged files.  
  ↳ 원인: 병합 충돌 해결되지 않음  
  ↳ 자동화 방지 가능: ✅ 충돌 여부 사전 점검 가능

- 에러 메시지: Unable to create '.git/index.lock'  
  ↳ 원인: 다른 Git 프로세스가 작업 중이거나 종료 중 충돌 발생  
  ↳ 자동화 방지 가능: ✅ lock 파일 존재 여부 사전 체크

- 에러 메시지: Please tell me who you are.  
  ↳ 원인: Git 사용자 정보가 설정되지 않음  
  ↳ 자동화 방지 가능: ✅ 사용자 설정 체크 및 안내 가능

📦 3️⃣ git push 단계

- 에러 메시지: error: failed to push some refs to ...  
  ↳ 원인: 원격 브랜치에 변경 사항이 있음  
  ↳ 자동화 방지 가능: ❌ 수동으로 pull 필요

- 에러 메시지: Updates were rejected because the remote contains work that you do not have locally  
  ↳ 원인: 원격과 로컬 내용 불일치  
  ↳ 자동화 방지 가능: ❌

- 에러 메시지: non-fast-forward  
  ↳ 원인: 강제 푸시 필요 (히스토리 불일치)  
  ↳ 자동화 방지 가능: ❌

- 에러 메시지: fatal: repository 'origin' does not exist  
  ↳ 원인: 원격 저장소 주소 설정 안 됨  
  ↳ 자동화 방지 가능: ✅ remote 설정 여부 사전 체크

- 에러 메시지: fatal: Not a git repository  
  ↳ 원인: .git 디렉토리 없음  
  ↳ 자동화 방지 가능: ✅


## 🙋 만든 이유

처음 Git을 사용할 때 자주 마주하던 커밋/푸시 오류들, 특히 영어로 된 에러 메시지에 대한 거부감을 줄이고, Git을 더 쉽게 배우고 쓸 수 있도록 도와주는 도구가 있었으면 좋겠다는 생각에서 출발했습니다.

이 스크립트를 통해 누구나 안정적으로 Git을 사용할 수 있고, 작업 흐름을 끊김 없이 이어갈 수 있기를 바랍니다.

---

## ✨ 추천 사용 대상

- Git이 익숙하지 않은 입문자
- 매번 같은 커밋 루틴을 반복하는 개발자
- 깃 커밋/푸시 에러로 멘붕 온 적 있는 모든 분

---