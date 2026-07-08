---
title: Supabase 따라하기 스크린샷 — Playwright 캡처 가이드
date: 2026-07
task: AXLab-154
status: draft
updatedAt: 2026-07-06
audience: 문서 작성자·강사 (수강생용 아님)
---

# Supabase 따라하기 스크린샷 — Playwright 캡처 가이드

[Supabase 따라하기](Supabase-따라하기.md) 문서가 참조하는 스크린샷 **8장**을 Playwright로 일관되게 캡처해 `assets/supabase/`에 채우는 방법입니다. 로그인이 필요한 화면(대시보드)이 대부분이라, **로그인 상태를 한 번 저장해 재사용**하고, **API 키 같은 민감정보는 마스킹**하는 것이 핵심입니다.

> 이 문서는 **수강생용이 아니라 문서 작성자/강사용**입니다. 스크린샷을 만드는 사람만 봅니다.

---

## 채워야 할 이미지 (파일명 = 문서가 참조하는 그대로)

파일명이 한 글자라도 다르면 문서에서 깨진 이미지로 나옵니다. 아래 이름을 **그대로** 씁니다.

| # | 파일명 | 화면 | 로그인 | 상호작용 필요 | 비고 |
|---|--------|------|:------:|:------------:|------|
| 1 | `01-login.png` | Supabase 로그인 화면 | ✗ | ✗ | 로그인 전 화면 — 인증 불필요 |
| 2 | `02-new-project.png` | New Project 생성 폼 | ✓ | ✓ (폼 입력) | 실제 프로젝트가 생성됨 — 주의 |
| 3 | `03-table-new.png` | Table Editor 새 표 만들기 | ✓ | ✓ (모달) | 표 이름·열 입력 상태 |
| 4 | `04-insert-row.png` | 행 추가(Insert row) 모달 | ✓ | ✓ (모달) | title/content 채운 상태 |
| 5 | `05-table-filled.png` | 데이터가 채워진 표 | ✓ | ✗ | URL 직접 이동으로 자동 캡처 |
| 6 | `06-api-settings.png` | Project URL + API Key | ✓ | ✗ | **키 마스킹 필수** |
| 7 | `07-vercel-env.png` | Vercel 환경변수 입력 | ✓(Vercel) | ✗ | **Supabase 아님 — Vercel 계정** |
| 8 | `08-live-data.png` | 배포 사이트에 데이터 표시 | ✗ | ✗ | 공개 URL — 인증 불필요 |

저장 위치: `Docs/Lesson3/assets/supabase/`

> **7번은 Vercel, 8번은 공개 사이트**라 Supabase 로그인 상태와 무관합니다. 7번은 Vercel용 로그인 상태를 따로 저장하거나 수동 캡처가 편합니다.

---

## 사전 준비

작업용 폴더는 저장소 밖(또는 `.gitignore` 처리)에 두는 걸 권장합니다 — `auth.json`(로그인 세션)이 커밋되면 안 됩니다.

```bash
mkdir -p ~/supabase-shots && cd ~/supabase-shots
npm init -y
npm i -D playwright
npx playwright install chromium
```

> **멀티-OS**: 위 명령은 macOS/Windows(git-bash)/Linux 공통입니다. Windows에서 `npx`가 안 먹으면 `npm exec playwright install chromium`로 대체.

---

## 핵심 개념 3가지

1. **로그인 상태 저장 (`storageState`)** — 브라우저에서 한 번만 수동 로그인하고 세션을 `auth.json`으로 저장하면, 이후 스크립트가 그 상태로 대시보드에 바로 진입합니다. GitHub OAuth를 스크립트로 흉내 낼 필요가 없습니다.
2. **민감정보 마스킹 (`mask`)** — `page.screenshot({ mask: [...] })`로 특정 요소를 색 블록으로 가립니다. **anon 키·service_role 키·프로젝트 참조가 그대로 찍히면 안 됩니다.**
3. **일관된 크기 (`viewport` + `deviceScaleFactor`)** — 모든 캡처를 같은 뷰포트로 찍어 문서 안에서 크기가 들쭉날쭉하지 않게 합니다. 레티나 선명도를 위해 `deviceScaleFactor: 2`.

---

## 1단계 — 로그인 상태 저장 (한 번만)

`save-auth.mjs`:

```js
import { chromium } from 'playwright'

const browser = await chromium.launch({ headless: false }) // 눈으로 보며 로그인
const context = await browser.newContext({
  viewport: { width: 1440, height: 900 },
  deviceScaleFactor: 2,
})
const page = await context.newPage()

await page.goto('https://supabase.com/dashboard/sign-in')

// 열린 브라우저에서 직접 GitHub 로그인 → 대시보드가 보일 때까지 진행.
// 로그인이 끝나면 Playwright Inspector의 ▶ Resume 를 누른다.
await page.pause()

await context.storageState({ path: 'auth.json' })
console.log('로그인 상태를 auth.json 에 저장했습니다.')
await browser.close()
```

실행:

```bash
node save-auth.mjs
```

> `auth.json`에는 세션 토큰이 들어 있습니다. **절대 커밋하지 마세요.** 작업 폴더에 `.gitignore`로 `auth.json`을 넣거나, 저장소 밖에서 작업합니다.

---

## 2단계 — 자동 캡처 (인증 필요 화면)

프로젝트 참조(ref)는 대시보드 URL에서 확인합니다: `https://supabase.com/dashboard/project/`**`<여기가 ref>`**. 아래 `PROJECT_REF`만 바꿉니다.

`capture.mjs`:

```js
import { chromium } from 'playwright'

const PROJECT_REF = 'xxxxxxxxxxxxxxxx'          // ← 본인 프로젝트 ref로 교체
const OUT = '/절대경로/Docs/Lesson3/assets/supabase' // ← 저장소 assets 경로로 교체
const BASE = `https://supabase.com/dashboard/project/${PROJECT_REF}`

const browser = await chromium.launch()
const context = await browser.newContext({
  storageState: 'auth.json',                    // 1단계에서 저장한 로그인 상태
  viewport: { width: 1440, height: 900 },
  deviceScaleFactor: 2,
})
const page = await context.newPage()

// 민감정보로 보일 만한 요소(JWT 형태 키 등)를 가리는 마스크 목록
const keyMasks = () => [
  page.getByText(/eyJ[A-Za-z0-9._-]{20,}/),     // anon/service_role JWT
]

// (05) 데이터가 채워진 표 — Table Editor로 직접 이동
await page.goto(`${BASE}/editor`)
await page.waitForLoadState('networkidle')
await page.screenshot({ path: `${OUT}/05-table-filled.png` })

// (06) API 설정 — 키는 반드시 마스킹
await page.goto(`${BASE}/settings/api`)
await page.waitForLoadState('networkidle')
await page.screenshot({
  path: `${OUT}/06-api-settings.png`,
  mask: keyMasks(),
  maskColor: '#94a3b8',                         // 가림 블록 색(기본 분홍 대신 회색)
})

await browser.close()
console.log('05, 06 캡처 완료.')
```

실행:

```bash
node capture.mjs
```

> **URL은 현재(2026-07) 기준**입니다. Supabase가 경로를 바꾸면(`/editor`, `/settings/api` 등) 대시보드에서 실제 주소를 확인해 교체하세요. `page.waitForLoadState('networkidle')`로 데이터가 다 그려진 뒤 찍습니다.

---

## 3단계 — 반자동 캡처 (모달·폼이 있는 화면)

`02`(New Project 폼), `03`(새 표 모달), `04`(행 추가 모달)는 **사람이 폼을 채운 상태**를 찍어야 자연스럽습니다. `page.pause()`로 멈춘 뒤 손으로 그 화면까지 만들고, Resume 하면 스크립트가 찍습니다.

`capture-manual.mjs`:

```js
import { chromium } from 'playwright'

const OUT = '/절대경로/Docs/Lesson3/assets/supabase'

const browser = await chromium.launch({ headless: false })
const context = await browser.newContext({
  storageState: 'auth.json',
  viewport: { width: 1440, height: 900 },
  deviceScaleFactor: 2,
})
const page = await context.newPage()

async function manualShot(name, startUrl) {
  await page.goto(startUrl)
  console.log(`\n▶ ${name}: 화면을 원하는 상태로 만든 뒤 Inspector의 Resume를 누르세요.`)
  await page.pause()                             // 여기서 손으로 폼/모달 채우기
  await page.screenshot({ path: `${OUT}/${name}` })
  console.log(`  저장: ${name}`)
}

await manualShot('02-new-project.png', 'https://supabase.com/dashboard/new/_')
await manualShot('03-table-new.png',   'https://supabase.com/dashboard/project/PROJECT_REF/editor')
await manualShot('04-insert-row.png',  'https://supabase.com/dashboard/project/PROJECT_REF/editor')

await browser.close()
```

> 한 번에 하나씩 하고 싶으면 필요한 `manualShot(...)` 줄만 남기고 실행하세요. `PROJECT_REF`는 실제 값으로 교체.

---

## 4단계 — 인증 불필요 화면 (01, 08)

로그인·공개 페이지라 `auth.json` 없이 찍습니다.

```js
import { chromium } from 'playwright'
const OUT = '/절대경로/Docs/Lesson3/assets/supabase'

const browser = await chromium.launch()
const ctx = await browser.newContext({ viewport: { width: 1440, height: 900 }, deviceScaleFactor: 2 })
const page = await ctx.newPage()

// (01) 로그인 화면
await page.goto('https://supabase.com/dashboard/sign-in')
await page.waitForLoadState('networkidle')
await page.screenshot({ path: `${OUT}/01-login.png` })

// (08) 배포된 우리 사이트에 데이터가 뜬 화면
await page.goto('https://<우리팀>.vercel.app')   // ← 팀 배포 주소로 교체
await page.waitForLoadState('networkidle')
await page.screenshot({ path: `${OUT}/08-live-data.png` })

await browser.close()
```

---

## 5단계 — 07 (Vercel 환경변수) 처리

`07-vercel-env.png`는 **Supabase가 아니라 Vercel 대시보드**입니다. 두 가지 방법:

- **간단**: 이미 만든 [Vercel 배포 따라하기]의 `09-env-form.png`류 캡처 방식과 동일하게 **수동 캡처** 후 파일명만 `07-vercel-env.png`로 저장.
- **Playwright로**: 위 `save-auth.mjs`를 `vercel.com`용으로 한 번 더 돌려 `auth-vercel.json`을 만들고, `storageState: 'auth-vercel.json'`로 `https://vercel.com/<팀>/<프로젝트>/settings/environment-variables`를 캡처. 이때도 값 칸은 마스킹.

---

## 민감정보 체크 (커밋 전 필수)

- **service_role 키**는 화면에 절대 드러나면 안 됩니다. 기본은 가려져 있지만, "Reveal"을 누른 상태로 찍지 마세요. 실수로 찍혔으면 그 이미지는 폐기하고 **키를 회전(rotate)** 하세요.
- **anon public 키**도 마스킹 권장(문서 목적상 위치만 보이면 됨).
- 캡처 후 각 PNG를 눈으로 열어 키·비밀번호·개인 이메일이 남지 않았는지 확인합니다.
- `auth.json` / `auth-vercel.json`은 커밋 금지. 작업 폴더를 저장소 밖에 두거나 `.gitignore` 처리.

---

## 팁 & 트러블슈팅

- **셀렉터를 모를 때**: `npx playwright codegen https://supabase.com/dashboard` 로 클릭을 녹화하면 정확한 셀렉터·URL을 얻을 수 있습니다. 그걸 위 스크립트의 `page.pause()` 대신 넣으면 완전 자동화도 가능.
- **특정 패널만 찍기**: 전체 뷰포트 대신 요소만 —
  `await page.locator('main').screenshot({ path: ... })`.
- **이미지가 너무 큼**: `deviceScaleFactor: 2`는 픽셀이 2배(1440×900 → 2880×1800)입니다. 용량이 부담되면 `1`로 낮추거나 캡처 후 압축(`pngquant`, `oxipng`).
- **한글 폰트 깨짐**: Linux CI 환경이면 `fonts-noto-cjk` 설치. 로컬 macOS/Windows는 문제없음.
- **로딩 타이밍**: `networkidle`로도 늦게 뜨는 위젯은 `await page.getByText('Project API Keys').waitFor()`처럼 **특정 요소 등장**을 기다린 뒤 찍으면 안정적입니다.
- **파일명 검증**: 캡처 후 `ls Docs/Lesson3/assets/supabase/` 로 8개 이름이 문서 참조와 1:1인지 확인. 문서를 브라우저로 열어 깨진 이미지가 없는지 최종 확인.

---

## 요약 흐름

```
save-auth.mjs (1회 수동 로그인 → auth.json)
   → capture.mjs         : 05, 06 (자동, 06은 키 마스킹)
   → capture-manual.mjs  : 02, 03, 04 (page.pause로 손으로 상태 만들고 찍기)
   → 무인증 스크립트      : 01, 08
   → 07 (Vercel)         : 수동 또는 Vercel용 auth 별도
   → 커밋 전 민감정보 점검 → assets/supabase/ 에 8장 완성
```
