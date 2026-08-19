# MCP 버전 따라하기 문서를 자체 완결형 메인 교재로 승격

작성일: 2026-08-19
대상: `Lesson3/` — Supabase·Vercel 따라하기 MCP 버전

## 배경

`상상인AI교육_3회차_자료/index.html`은 이미 MCP 버전을 "실습 따라하기 — 본 교재",
웹 버전을 "참고용"으로 배치하고 있다. 그러나 두 MCP 문서 모두 앞부분이 웹 문서에 의존해
혼자서는 처음부터 끝까지 진행할 수 없다.

- **Supabase MCP**: 계정 가입과 프로젝트 생성(DB 비밀번호·리전) 단계가 없다.
  비교표에서 "1단계 프로젝트 만들기 → (강사 사전 준비)"로 빼 두었고, 강사 메모도 같은 방침을 적고 있다.
  수강생 계정에 조직·프로젝트가 없으면 0단계 MCP OAuth의 조직 선택 화면에서 막힌다.
- **Vercel MCP**: `## 0~3단계 — 웹으로 (기존 문서 그대로)` 섹션이 웹 문서로 넘긴다.
  본문은 "무엇을 왜 자동화 못 하는가" 표만 있고 실제 절차가 없다.

추가로 `.md`와 `.html`이 갈라져 있다. Vercel 웹 문서의 "2단계 내 저장소 만들기(미러링)"은
`.html`에만 존재하고 `.md`에는 없다(커밋 26ec776이 html만 수정). **실제 배포물인 HTML이 최신본이다.**

## 목표

MCP 버전 문서 두 개가 웹 문서를 열지 않고도 처음부터 끝까지 완주 가능하도록 만든다.
웹 버전은 "화면이 어디 있는지 눈으로 확인하고 싶을 때" 보는 참고용 지위를 유지한다.

## 결정 사항

| 항목 | 결정 |
|---|---|
| 범위 | Supabase MCP + Vercel MCP 두 문서 모두 |
| 이식 방식 | 웹 문서의 기존 절차와 스크린샷을 그대로 복사 (MCP `create_project` 자동화는 쓰지 않음) |
| 단계 번호 | 계정·프로젝트 생성은 **번호 없는 "시작 전 준비" 섹션**으로 앞에 배치, 본 실습 0~5단계 번호는 유지 |
| 기준 파일 | **HTML이 기준.** HTML 4개를 고치고 `.md` 2개도 동일 내용으로 맞춘다 (누락된 미러링 단계 역이식 포함) |

단계 번호를 통번호로 재배정하지 않는 이유: "웹 7단계 → MCP 5단계로 줄었다"는 문서 전반의
서술과 비교표, 강사 진행 기준이 깨진다. 준비 섹션은 1회성이고 다음 실습부터는 건너뛴다.

## 변경 대상 파일

수정 (8개):

- `Lesson3/data/Supabase-따라하기-MCP.html` — 기준
- `Lesson3/data/Supabase-따라하기-MCP.md`
- `Lesson3/data/Vercel-배포-따라하기-MCP.html` — 기준
- `Lesson3/data/Vercel-배포-따라하기-MCP.md`
- `Lesson3/상상인AI교육_3회차_자료/Supabase-따라하기-MCP.html` — 배포본 (강사 메모 없음)
- `Lesson3/상상인AI교육_3회차_자료/Vercel-배포-따라하기-MCP.html` — 배포본 (강사 메모 없음)
- `Lesson3/상상인AI교육_3회차_자료/index.html` — 카드 설명 문구
- `Lesson3/data/Vercel-배포-따라하기.md` — 미러링 단계 역이식 (html과 동기화)

재생성 (1개):

- `Lesson3/상상인AI교육_3회차_자료_20260818.zip` — 배포 묶음. 날짜 부분을 새로 갱신한다.

건드리지 않음: 웹 버전 HTML(본문 절차의 원본), `assets/` (기존 스크린샷 재사용), `Curriculum.md`.

## 1. Supabase 따라하기 MCP

기존 0~5단계 앞에 "시작 전 준비" 섹션을 추가한다. 본문은 웹 문서 0·1단계에서 복사하되
"1회만 하면 되고 다음 실습부터는 건너뛴다"는 안내를 붙인다.

**A. Supabase 계정 만들기** — 웹 0단계에서 이식
- supabase.com → Start your project → Continue with GitHub
- 스크린샷: `assets/supabase/01-login.png`, `01b-github-login-google.png`,
  `01c-authorize-supabase.png`, `01d-new-organization.png`

**B. 프로젝트 만들기** — 웹 1단계에서 이식
- New Project → Name / Database Password / Region
- **Database Password는 자동 생성 버튼을 눌러도 되지만 반드시 메모**한다는 경고 유지
- Region은 Northeast Asia (Seoul) 권장
- 생성에 1~2분 소요
- 스크린샷: `assets/supabase/02-new-project.png`, `02b-db-password.png`, `02d-project-ready.png`

부수 수정:

- 준비물: "Supabase 계정 (GitHub으로 가입 가능)" → 준비 섹션에서 만든다는 안내로 교체
- 전체 흐름 다이어그램: 앞에 `[준비] 계정 → 프로젝트` 한 줄 추가, 뒤 `0~5단계` 유지
- "웹 버전과 나란히 보기" 표: `1단계 프로젝트 만들기 | (강사 사전 준비)` 행을
  `시작 전 준비 B | 동일 (웹 절차 그대로)`로 교체
- 강사 메모(`data/`만): "1단계(프로젝트 생성)는 강사 사전 준비로 빼는 것을 권합니다" 항목을
  "시간이 부족하면 준비 섹션을 강사가 사전에 대신 처리할 수 있다"는 선택지로 완화.
  "웹 버전을 먼저 보여주고 MCP 버전으로 실습" 항목도 MCP 단독 진행이 기본임을 반영해 수정

## 2. Vercel 배포 따라하기 MCP

`## 0~3단계 — 웹으로 (기존 문서 그대로)` 섹션을 실제 절차로 교체한다.
번호는 유지하되(4~8단계가 뒤에 그대로 있으므로) 각 항목을 웹 HTML에서 복사한다.

Supabase는 "번호 없는 준비 섹션", Vercel은 "번호 있는 0~3단계"로 서로 다르게 가는 것이 맞다.
Vercel MCP는 처음부터 0~3단계를 자기 번호 체계에 포함해 두고 내용만 위임한 형태였으므로
번호를 그대로 두면 되고, Supabase MCP는 0단계가 이미 MCP 연결이라 앞에 번호를 끼우면
전체가 밀린다. 두 문서 모두 "이 부분은 1회만, 웹 화면에서" 라는 성격은 동일하게 표시한다.

- **0단계 Vercel 로그인** — Continue with GitHub
- **1단계 GitHub 연결 + Vercel 앱 설치** — 가장 많이 막히는 곳.
  "To link a GitHub repository, you need to install the GitHub integration first" 빨간 경고 함정 포함.
  스크린샷: `assets/vercel/01-git-connect.png`, `02-install-warning.png`
- **2단계 내 저장소 만들기 (미러링)** — 강사 원본 저장소를 내 계정으로 복사.
  원본은 `https://github.com/tutorials-test1/Sangsangin_3_vercel_sample.git`,
  새 저장소 이름은 `vercel-test`, README·.gitignore·license 체크 금지.
  스크린샷: `assets/vercel/mirror-1-github-new-repo.png` ~ `mirror-6-vercel-import.png`
- **3단계 프로젝트 가져오기(Import) + 배포** — Import → 설정 그대로 → Deploy →
  Congratulations 화면 → `주소.vercel.app` 확인.
  스크린샷: `assets/vercel/03-import-config.png`, `04-deploying.png`,
  `05-deploy-success.png`, `06-project-dashboard.png`

"왜 자동화가 안 되나" 표는 삭제하지 않고, 각 단계 안의 짧은 강사 포인트 노트로 흡수한다
(웹으로 해야 하는 이유는 여전히 알려줄 가치가 있으나, 절차를 대신할 수는 없다).

부수 수정:

- 준비물: 원본 저장소 주소가 필요하다는 항목 추가
- 전체 흐름 다이어그램의 `[웹으로]` 줄은 유지 (실제로 웹에서 하는 게 맞음)
- "웹 버전과 나란히 보기" 표에서 0~3단계 행을 "동일"로 정정

## 3. index.html 및 배포 묶음

- 본 교재 카드 설명에 준비 단계가 포함됐음을 반영
  - Vercel: "로그인·저장소 미러링 → 배포 → 통로 열기 → 환경변수 → 재배포 → 막혔을 때"
  - Supabase: "계정·프로젝트 만들기 → 표 → 데이터 → 읽기 공개 정책 → 연결 → 확인"
- 웹 버전 카드 설명에 "MCP 버전만으로 완주할 수 있으며, 화면 위치를 확인하고 싶을 때 참고"라는
  취지를 반영
- 배포 zip을 갱신된 폴더로 재생성

## 검증

문서 작업이므로 자동 테스트는 없다. 다음을 눈으로 확인한다.

1. `grep`으로 MCP 문서 4개에 남은 웹 문서 의존 문구(`기존 문서 그대로`, `강사 사전 준비`)가
   없는지 확인
2. MCP HTML이 참조하는 모든 `assets/` 경로가 실제 파일로 존재하는지 스크립트로 검사
   (`data/`는 `../assets/`, `상상인AI교육_3회차_자료/`는 `assets/` 기준 — 경로 접두사가 다름에 주의)
3. 브라우저로 MCP HTML 2개를 열어 목차 링크(`#step*` 앵커)와 이미지가 깨지지 않는지 확인
4. `data/` 버전과 `상상인AI교육_3회차_자료/` 버전의 차이가 "강사 메모 유무와 asset 경로"뿐인지 확인
5. `.md`와 `.html`의 단계 구성이 일치하는지 헤딩 목록으로 대조

## 범위 밖

- MCP `create_project`를 이용한 프로젝트 자동 생성 (선택지에서 탈락)
- 웹 버전 문서의 절차 자체 개선
- `Supabase-로그인-글쓰기-MCP` (6교시 자료) — 이번 변경과 독립
