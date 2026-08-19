# MCP 버전 메인 교재 승격 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Supabase·Vercel MCP 버전 따라하기 문서가 웹 버전을 열지 않고도 처음부터 끝까지 완주 가능하도록 계정·프로젝트·저장소 생성 절차를 이식한다.

**Architecture:** 웹 버전 HTML에 이미 존재하는 절차 블록(`<h2 id="stepN">` ~ 다음 `<hr>`)을 MCP 버전 HTML로 복사해 넣고, 문서 상단의 목차·준비물·흐름도와 하단의 비교표·강사 메모를 새 구성에 맞게 고친다. `data/`(강사용 원본, `../assets/` 경로)와 `상상인AI교육_3회차_자료/`(배포본, `assets/` 경로, 강사 메모 없음) 두 벌을 같은 내용으로 유지하고, `.md`는 HTML을 따라가게 맞춘다.

**Tech Stack:** 순수 HTML + 인라인 CSS(문서마다 자체 포함), Markdown. 빌드 도구·테스트 러너 없음. 검증은 grep과 파일 존재 확인 스크립트, 그리고 브라우저 육안 확인.

## Global Constraints

- 대상 독자는 **비개발자**다. 새로 쓰는 문장은 기존 문서의 어조(존댓말, 짧은 문장, 영어 용어에 한글 병기)를 따른다.
- **스크린샷은 새로 찍지 않는다.** `Lesson3/assets/` 에 이미 있는 파일만 재사용한다.
- 이미지 경로 접두사: `Lesson3/data/*.html` 은 `../assets/...`, `Lesson3/상상인AI교육_3회차_자료/*.html` 은 `assets/...`.
- 문서 간 링크 이름: `data/` 에서는 `Supabase-따라하기.html` / `Vercel-배포-따라하기.html`, 배포본에서는 `Supabase-따라하기-웹버전-참고용.html` / `Vercel-배포-따라하기-웹버전-참고용.html`.
- 배포본(`상상인AI교육_3회차_자료/`)에는 **`강사 메모` 섹션이 없다.** 이 차이를 유지한다.
- Supabase MCP의 본 실습 단계 번호 **0~5단계는 바꾸지 않는다.** 계정·프로젝트 생성은 번호 없는 "시작 전 준비" 섹션으로 앞에 둔다.
- Vercel MCP의 단계 번호 **0~8단계 체계를 유지한다.** 기존 `0~3단계` 위임 섹션을 실제 0·1·2·3단계로 펼친다.
- 미러링 원본 저장소 주소: `https://github.com/tutorials-test1/Sangsangin_3_vercel_sample.git`
- 미러링으로 만들 내 저장소 이름: `vercel-test`
- 커밋 메시지는 한국어, `docs:` 접두사를 쓴다(기존 이력과 동일).

---

### Task 1: Supabase MCP — "시작 전 준비" 섹션 이식 (data/ HTML)

**Files:**
- Modify: `Lesson3/data/Supabase-따라하기-MCP.html`
- Reference (읽기만): `Lesson3/data/Supabase-따라하기.html:235-290`

**Interfaces:**
- Consumes: 없음 (첫 작업)
- Produces: `id="prep"`, `id="prep-a"`, `id="prep-b"` 앵커. Task 3(배포본)과 Task 5(.md)가 이 섹션 문안을 그대로 복제한다.

- [ ] **Step 1: 현재 상태 확인**

```bash
cd /Users/peter/Repo/sangsangin_edu_3/Lesson3
grep -n 'nav class="toc"\|<h2>준비물</h2>\|class="flow"\|id="step0"' data/Supabase-따라하기-MCP.html | head
```

기대 출력: `nav class="toc"` (206행 부근), `<h2>준비물</h2>` (218행 부근), `class="flow"` (227행 부근), `id="step0"` (232행 부근). 행 번호는 참고용이며 정확히 일치하지 않아도 된다.

- [ ] **Step 2: 목차(`nav.toc`)에 준비 섹션 추가**

`<nav class="toc">` 의 `<h2>전체 흐름</h2>` 바로 아래, `<ol>` 앞에 준비 목록을 넣는다. 기존 `<ol>` 은 손대지 않는다.

```html
<nav class="toc">
  <h2>전체 흐름</h2>
  <p style="margin:0 0 6px;font-size:0.92em;color:#59636e">시작 전 준비 — 처음 한 번만 (다음 실습부터는 건너뜁니다)</p>
  <ul style="margin:0 0 12px 1.2em">
    <li><a href="#prep-a">Supabase 계정 만들기</a></li>
    <li><a href="#prep-b">프로젝트 만들기</a></li>
  </ul>
  <ol>
```

- [ ] **Step 3: 준비물 항목 교체**

기존 항목

```html
  <li><strong>Supabase 계정</strong> (GitHub으로 가입 가능)</li>
```

을 아래로 바꾼다.

```html
  <li><strong>GitHub 계정</strong> — Supabase 계정과 프로젝트는 아래 <a href="#prep">시작 전 준비</a>에서 직접 만듭니다</li>
```

- [ ] **Step 4: 흐름도 문장에 준비 단계 반영**

기존

```html
<p>전체 흐름은 이렇게 5단계입니다. 웹 버전 7단계가 5단계로 줄었습니다.</p>
<div class="flow">0. 통로 열기(MCP 연결) → 1. 표 만들기 → 2. 데이터 넣기<br>
&nbsp;&nbsp;&nbsp;→ 3. 읽기 공개 정책 → 4. 사이트에 연결 → 5. 화면 확인</div>
```

을 아래로 교체한다.

```html
<p><strong>처음 한 번만</strong> 계정과 프로젝트를 만들고, 그 뒤 본 실습은 <strong>5단계</strong>입니다. 웹 버전에서 7단계에 걸쳐 클릭하던 일이 5단계로 줄었습니다.</p>
<div class="flow">[처음 한 번] A. 계정 만들기 → B. 프로젝트 만들기<br>
0. 통로 열기(MCP 연결) → 1. 표 만들기 → 2. 데이터 넣기<br>
&nbsp;&nbsp;&nbsp;→ 3. 읽기 공개 정책 → 4. 사이트에 연결 → 5. 화면 확인</div>
```

- [ ] **Step 5: "시작 전 준비" 본문 삽입**

Step 4에서 고친 `<div class="flow">` 다음의 `<hr>` 과 `<h2 id="step0">` **사이**에 아래 블록 전체를 넣는다.

```html
<h2 id="prep">시작 전 준비 <small>(처음 한 번만 · 웹 화면에서)</small></h2>
<p>계정을 만들고 데이터가 담길 <strong>프로젝트</strong>를 하나 만듭니다. 이 두 가지는 사람이 직접 해야 하는 일이라 웹 화면에서 진행합니다. <strong>한 번 해두면 다음 실습부터는 건너뜁니다.</strong></p>

<h3 id="prep-a">A. Supabase 계정 만들기</h3>
<ol>
  <li>브라우저에서 <a href="https://supabase.com" target="_blank" rel="noopener">https://supabase.com</a> 에 접속합니다.</li>
  <li><strong>Start your project</strong> 또는 <strong>Sign In</strong> → <strong>Continue with GitHub</strong>(GitHub로 계속)를 누릅니다.</li>
  <li>GitHub 로그인 창이 뜨면 여러분의 GitHub 계정으로 로그인·허용합니다.</li>
</ol>
<figure>
  <img src="../assets/supabase/01-login.png" alt="Supabase 로그인 화면">
  <figcaption>Supabase 로그인 화면</figcaption>
</figure>
<figure>
  <img src="../assets/supabase/01b-github-login-google.png" alt="GitHub 로그인 창">
  <figcaption>GitHub 로그인 창 (Google 계정으로 로그인하는 경우)</figcaption>
</figure>
<figure>
  <img src="../assets/supabase/01c-authorize-supabase.png" alt="Supabase 접근 허용 화면">
  <figcaption>Supabase의 GitHub 접근 허용 화면</figcaption>
</figure>
<figure>
  <img src="../assets/supabase/01d-new-organization.png" alt="새 조직 만들기 화면">
  <figcaption>처음 로그인하면 뜨는 조직(Organization) 만들기 화면</figcaption>
</figure>
<div class="note">GitHub로 가입하면 Vercel 때 쓴 계정과 같아 헷갈리지 않습니다. 이메일 가입도 가능합니다. <strong>조직(Organization)은 꼭 하나 만들어 두세요</strong> — 0단계에서 통로를 열 때 이 조직을 고르게 됩니다.</div>

<h3 id="prep-b">B. 프로젝트 만들기 (New Project)</h3>
<p><strong>프로젝트</strong> = 우리 팀의 데이터가 담길 하나의 상자입니다. 팀마다 하나씩 만듭니다.</p>
<ol>
  <li>대시보드에서 <strong>New Project</strong>(새 프로젝트)를 누릅니다. (조직(Organization)을 먼저 고르라고 하면 있는 것 하나를 고르거나 새로 만듭니다.)</li>
  <li>입력칸을 채웁니다.
    <ul>
      <li><strong>Name</strong>(이름): 팀 이름 등 알아보기 쉬운 이름 (예: <code>1jo-site</code>)</li>
      <li><strong>Database Password</strong>(데이터베이스 비밀번호): 자동 생성 버튼을 눌러도 됩니다. <strong>반드시 메모해 둡니다.</strong></li>
      <li><strong>Region</strong>(지역): 가까운 곳 — <strong>Northeast Asia (Seoul)</strong> 권장</li>
    </ul>
  </li>
</ol>
<figure>
  <img src="../assets/supabase/02-new-project.png" alt="프로젝트 생성 화면">
  <figcaption>프로젝트 생성 화면</figcaption>
</figure>
<figure>
  <img src="../assets/supabase/02b-db-password.png" alt="Database Password 입력칸">
  <figcaption>Database Password 자동 생성 버튼</figcaption>
</figure>
<ol start="3">
  <li><strong>Create new project</strong>(프로젝트 생성)를 누릅니다. 준비되는 데 <strong>1~2분</strong> 걸립니다.</li>
</ol>
<figure>
  <img src="../assets/supabase/02d-project-ready.png" alt="프로젝트 준비 완료 화면">
  <figcaption>준비가 끝나 Healthy 상태가 된 화면</figcaption>
</figure>
<div class="warn">
  <strong>중요</strong> — Database Password는 나중에 찾기 어렵습니다. 팀 메모장에 적어 두세요. (이 데모에서 직접 쓰진 않지만 분실하면 곤란합니다.)
</div>
<div class="note">여기까지가 웹 화면에서 하는 전부입니다. <strong>0단계부터는 Claude Code 창에서 말로 진행합니다.</strong></div>

<hr>
```

- [ ] **Step 6: 비교표 행 정정**

"웹 버전과 나란히 보기" 표에서 아래 행을 찾아

```html
<td>1단계 프로젝트 만들기</td><td>(강사 사전 준비)</td><td>강사가 팀별로 미리 만들어 둡니다</td>
```

(태그가 여러 줄로 나뉘어 있을 수 있다 — `grep -n "강사 사전 준비" data/Supabase-따라하기-MCP.html` 으로 위치를 찾는다) 다음 내용으로 바꾼다.

```html
<td>0단계 로그인 · 1단계 프로젝트 만들기</td><td>시작 전 준비 A·B</td><td>같은 절차 (처음 한 번만)</td>
```

- [ ] **Step 7: 강사 메모 수정**

아래 항목을 찾아

```html
<li><strong>1단계(프로젝트 생성)는 강사 사전 준비로 빼는 것을 권합니다.</strong>
```

로 시작하는 `<li>` 전체를 다음으로 교체한다.

```html
<li><strong>시간이 부족하면 "시작 전 준비"를 강사가 대신 처리할 수 있습니다.</strong> MCP로 팀별 프로젝트를 일괄 생성해두면 수강생은 0단계(통로 열기)부터 시작하면 됩니다. 다만 <strong>기본은 수강생이 직접 만드는 것</strong>입니다 — 자기 계정에 프로젝트가 있어야 다음 회차에 이어서 쓸 수 있습니다.</li>
```

이어서 아래 항목을 찾아

```html
<li><strong>웹 버전을 먼저 한 번 보여주고 MCP 버전으로 실습</strong>
```

로 시작하는 `<li>` 전체를 다음으로 교체한다.

```html
<li><strong>이 문서만으로 완주할 수 있습니다.</strong> 웹 버전은 "화면이 어디에 있는지" 감을 주기 위한 참고용입니다. 시간이 되면 4교시 도입에 웹 버전 화면을 5분쯤 훑어주면 나중에 대시보드에서 확인할 때 덜 막힙니다.</li>
```

- [ ] **Step 8: 검증 — 남은 위임 문구와 이미지 경로 확인**

```bash
cd /Users/peter/Repo/sangsangin_edu_3/Lesson3
grep -c "강사 사전 준비" data/Supabase-따라하기-MCP.html
for p in $(grep -o 'src="\.\./assets/[^"]*"' data/Supabase-따라하기-MCP.html | sed 's/src="\.\.\///;s/"//'); do [ -f "$p" ] || echo "MISSING: $p"; done
grep -o 'href="#[a-z0-9-]*"' data/Supabase-따라하기-MCP.html | sort -u
```

기대 출력:
- 첫 명령: `0`
- 두 번째 명령: 출력 없음 (누락 이미지 없음)
- 세 번째 명령: `#prep`, `#prep-a`, `#prep-b`, `#step0`~`#step5` 가 모두 보이고, 각 앵커에 대응하는 `id=` 가 문서에 존재해야 한다

- [ ] **Step 9: 브라우저 육안 확인**

```bash
open /Users/peter/Repo/sangsangin_edu_3/Lesson3/data/Supabase-따라하기-MCP.html
```

확인 항목: 목차의 "Supabase 계정 만들기"·"프로젝트 만들기" 링크가 준비 섹션으로 이동하는가, 이미지 7장이 모두 보이는가, 준비 섹션 다음에 0단계가 이어지는가.

- [ ] **Step 10: 커밋**

```bash
cd /Users/peter/Repo/sangsangin_edu_3
git add Lesson3/data/Supabase-따라하기-MCP.html
git commit -m "docs: Supabase MCP 문서에 계정·프로젝트 생성 준비 섹션 추가"
```

---

### Task 2: Vercel MCP — 0~3단계 실제 절차로 교체 (data/ HTML)

**Files:**
- Modify: `Lesson3/data/Vercel-배포-따라하기-MCP.html`
- Reference (읽기만): `Lesson3/data/Vercel-배포-따라하기.html:186-369`

**Interfaces:**
- Consumes: 없음 (Task 1과 독립)
- Produces: `id="step0"`~`id="step3"` 앵커. Task 4(배포본)와 Task 6(.md)가 이 문안을 그대로 복제한다.

웹 버전 HTML은 0~5단계에 걸쳐 있는 내용을 MCP 버전에서는 0~3단계로 접는다. 대응은 이렇다.

| 웹 HTML | MCP 문서 |
|---|---|
| 0단계 Vercel 로그인 | 0단계 (그대로) |
| 1단계 GitHub 연결 + Vercel 앱 설치 | 1단계 (그대로) |
| 2단계 내 저장소 만들기 (미러링) | 2단계 (그대로) |
| 3단계 Import + 4단계 Deploy + 5단계 사이트 확인 | 3단계 (셋을 합침) |

- [ ] **Step 1: 교체 대상 범위 확인**

```bash
cd /Users/peter/Repo/sangsangin_edu_3/Lesson3
grep -n '기존 문서 그대로\|id="step4"' data/Vercel-배포-따라하기-MCP.html | head
```

기대 출력: `0~3단계` 위임 섹션의 `<h2>` 행과, 그 뒤 `id="step4"`(통로 열기) 행. 이 둘 사이가 이번에 통째로 교체할 범위다.

- [ ] **Step 2: 목차에 0~3단계 항목 명시**

`<nav class="toc">` 의 `<ol>` 첫 항목이 `0~3단계 — 웹으로` 하나로 묶여 있다면, 아래 4개 항목으로 펼친다. 나머지 항목(4~8단계)은 그대로 둔다.

```html
    <li><a href="#step0">Vercel 로그인 (GitHub로 가입)</a></li>
    <li><a href="#step1">GitHub 연결 + Vercel 앱 설치</a></li>
    <li><a href="#step2">내 저장소 만들기 (미러링)</a></li>
    <li><a href="#step3">프로젝트 가져오기 + 배포</a></li>
```

- [ ] **Step 3: 준비물에 원본 저장소 주소 추가**

준비물 `<ul>` 의 GitHub 계정 항목 아래에 다음 `<li>` 를 넣는다.

```html
  <li><strong>강사 저장소 주소</strong> — <code>https://github.com/tutorials-test1/Sangsangin_3_vercel_sample.git</code> (2단계에서 내 계정으로 복사할 원본)</li>
```

- [ ] **Step 4: 위임 섹션을 실제 절차로 교체**

`<h2 ...>0~3단계 — 웹으로 (기존 문서 그대로)</h2>` 부터 그 다음 `<hr>` 까지(= `id="step4"` 직전까지)를 아래 블록 전체로 교체한다.

이 교체로 기존의 "왜 자동화가 안 되나" 표는 사라진다. 그 내용은 아래 블록 안에 흡수되어 있다 — 0단계 도입 문단("계정 만들기와 저장소 연결은 사람이 직접 허락해줘야 하는 일이라 자동화되지 않습니다")과 1단계 경고 박스 끝의 "이 경고는 MCP로도 해결되지 않습니다". 표를 따로 남기지 않는다.

```html
<h2 id="step0"><span class="step-badge">0단계</span>Vercel 로그인 (GitHub로 가입)</h2>
<p>0~3단계는 <strong>웹 화면에서</strong> 합니다. 계정 만들기와 저장소 연결은 사람이 직접 허락해줘야 하는 일이라 자동화되지 않습니다. 4단계부터 Claude Code로 넘어옵니다.</p>
<ol>
  <li>브라우저에서 <a href="https://vercel.com" target="_blank" rel="noopener">https://vercel.com</a> 에 접속합니다.</li>
  <li><strong>Sign Up</strong>(가입) 또는 <strong>Log In</strong>(로그인) → <strong>Continue with GitHub</strong>를 누릅니다.</li>
  <li>GitHub 로그인 창이 뜨면 여러분의 GitHub 계정으로 로그인합니다.</li>
</ol>
<div class="note">처음이면 자동으로 회원가입이 됩니다. GitHub로 가입하면 다음 단계의 연결도 수월합니다.</div>

<hr>

<h2 id="step1"><span class="step-badge">1단계</span>GitHub 연결 + Vercel 앱 설치 <small>(가장 많이 막히는 곳)</small></h2>
<p>Vercel이 우리 GitHub 저장소를 가져오려면, <strong>"Vercel이 내 GitHub를 봐도 된다"고 한 번 허락</strong>해줘야 합니다. 이 허락이 안 돼 있으면 다음 단계에서 저장소가 안 보이거나 빨간 경고가 뜹니다.</p>
<ol>
  <li>새 프로젝트를 만들기 위해 <strong>Add New… → Project</strong>로 들어갑니다.</li>
</ol>
<figure>
  <img src="../assets/vercel/00-add-new-project.png" alt="Add New 버튼과 Project 메뉴">
  <figcaption>① Add New… 버튼 → ② Project 선택</figcaption>
</figure>
<ol start="2">
  <li><strong>Import Git Repository</strong>(Git 저장소 가져오기) 영역에서 <strong>Continue with GitHub</strong>를 누릅니다.</li>
</ol>
<figure>
  <img src="../assets/vercel/01-git-connect.png" alt="Git 제공자 연결 화면">
  <figcaption>Git 제공자 연결 화면</figcaption>
</figure>
<ol start="3">
  <li>GitHub 화면으로 넘어가면 <strong>Install / Authorize Vercel</strong>(설치·허용)을 누릅니다.
    <ul>
      <li>저장소 접근 범위는 <strong>All repositories</strong>(전체) 또는 <strong>Only select repositories</strong>(특정 저장소만) 중 하나를 고릅니다.</li>
    </ul>
  </li>
</ol>
<div class="warn">
  <strong>자주 나는 함정</strong> — 저장소를 가져오려 할 때 <strong>"To link a GitHub repository, you need to install the GitHub integration first"</strong> 라는 빨간 경고가 보이면, 그 저장소가 있는 계정(개인 계정 또는 조직)에 <strong>Vercel 앱이 아직 설치 안 된 것</strong>입니다. 위 1~3번처럼 그 계정에 Vercel을 한 번 설치하면 해결됩니다. <strong>이 경고는 MCP로도 해결되지 않습니다.</strong>
</div>
<figure>
  <img src="../assets/vercel/02-install-warning.png" alt="설치가 필요하다는 경고">
  <figcaption>설치가 필요하다는 경고</figcaption>
</figure>

<hr>

<h2 id="step2"><span class="step-badge">2단계</span>내 저장소 만들기 (미러링) <small>(강사 저장소 → 내 저장소)</small></h2>
<p>강사가 만들어 둔 튜토리얼 저장소를 <strong>통째로 복사해서 내 저장소로 만드는</strong> 과정입니다. 이걸 <strong>미러링(mirroring)</strong>이라고 합니다. Vercel은 <strong>내 저장소</strong>만 가져올 수 있고, 앞으로 코드를 고치려면 내 것이어야 하기 때문입니다.</p>
<ol>
  <li>GitHub 홈에서 왼쪽 위 초록색 <strong>New</strong>(새로 만들기) 버튼을 누릅니다.</li>
</ol>
<figure>
  <img src="../assets/vercel/mirror-1-github-new-repo.png" alt="GitHub 홈의 New 버튼">
  <figcaption>① 초록색 New 버튼</figcaption>
</figure>
<ol start="2">
  <li><strong>Repository name</strong>(저장소 이름)에 <code>vercel-test</code> 를 입력합니다. <strong>이름은 <code>vercel-test</code> 로 맞춰주세요.</strong></li>
  <li>나머지 설정은 그대로 두고, 맨 아래 <strong>Create repository</strong>(저장소 만들기)를 누릅니다.</li>
</ol>
<div class="note"><strong>Add README·.gitignore·license는 켜지 마세요.</strong> 완전히 비어 있는 저장소여야 다음 단계의 복사가 깔끔하게 들어갑니다.</div>
<figure>
  <img src="../assets/vercel/mirror-2-create-repo-form.png" alt="새 저장소 만들기 폼">
  <figcaption>① 저장소 이름은 vercel-test ② Create repository 버튼</figcaption>
</figure>
<ol start="4">
  <li>저장소가 만들어지면 안내 화면이 나옵니다. 여기 있는 <strong>HTTPS 주소</strong>를 복사합니다. 이게 <strong>내 저장소 주소</strong>입니다.</li>
</ol>
<figure>
  <img src="../assets/vercel/mirror-3-repo-url.png" alt="새로 만든 저장소의 HTTPS 주소">
  <figcaption>① 복사할 내 저장소 주소</figcaption>
</figure>
<ol start="5">
  <li>Claude Code에 <strong>강사 저장소 주소</strong>와 <strong>방금 복사한 내 저장소 주소</strong>를 함께 주고, 복사(미러링)해 달라고 요청합니다.
    <div class="flow">아래 저장소를 내 저장소로 그대로 복사해줘.<br>
가져올 곳: https://github.com/tutorials-test1/Sangsangin_3_vercel_sample.git<br>
넣을 곳: (방금 복사한 내 저장소 주소)</div>
  </li>
</ol>
<figure>
  <img src="../assets/vercel/mirror-4-claude-request.png" alt="Claude Code에 미러링을 요청하는 화면">
  <figcaption>① 강사가 만든 원본 저장소 (가져올 곳) ② 내가 방금 만든 저장소 (넣을 곳)</figcaption>
</figure>
<div class="warn">
  <strong>두 주소를 헷갈리지 마세요.</strong> <strong>위</strong>가 강사가 만든 <strong>원본</strong>(가져올 곳), <strong>아래</strong>가 방금 만든 <strong>내 저장소</strong>(넣을 곳)입니다. 순서가 바뀌면 엉뚱한 저장소를 덮어쓰게 됩니다.
</div>
<ol start="6">
  <li>끝나면 내 저장소 페이지를 새로고침합니다. 파일 목록과 <strong>브랜치·커밋</strong>이 원본 그대로 들어와 있으면 성공입니다.</li>
</ol>
<figure>
  <img src="../assets/vercel/mirror-5-after-mirroring.png" alt="미러링이 끝난 내 저장소">
  <figcaption>① 브랜치와 ② 커밋이 원본 그대로 복사됨</figcaption>
</figure>

<hr>

<h2 id="step3"><span class="step-badge">3단계</span>프로젝트 가져오기 (Import) + 배포</h2>
<ol>
  <li>Vercel에서 <strong>Add New… → Project</strong>로 들어가면 <strong>Import Git Repository</strong> 목록에 방금 만든 <code>vercel-test</code>가 보입니다.</li>
</ol>
<figure>
  <img src="../assets/vercel/mirror-6-vercel-import.png" alt="Import Git Repository 목록에 보이는 vercel-test">
  <figcaption>① 미러링한 vercel-test 저장소 ② Import 버튼</figcaption>
</figure>
<div class="note">목록에 <code>vercel-test</code>가 안 보이면 1단계의 <strong>Vercel 앱 설치·접근 권한</strong>이 그 계정에 안 돼 있는 것입니다. 1단계로 돌아가 설치하거나, 접근 범위에 <code>vercel-test</code>를 추가해 주세요.</div>
<ol start="2">
  <li><code>vercel-test</code> 오른쪽의 <strong>Import</strong>(가져오기)를 누릅니다.</li>
  <li>설정 화면은 <strong>대부분 그대로 두면 됩니다.</strong> (Project Name·Application Preset·Root Directory 모두 기본값)</li>
</ol>
<figure>
  <img src="../assets/vercel/04-project-config.png" alt="프로젝트 설정 화면">
  <figcaption>프로젝트 설정 화면</figcaption>
</figure>
<ol start="4">
  <li>맨 아래 <strong>Deploy</strong>(배포)를 누릅니다. 빌드에 <strong>1~2분</strong> 걸립니다.</li>
</ol>
<figure>
  <img src="../assets/vercel/04-deploying.png" alt="배포 진행 중">
  <figcaption>배포 진행 중</figcaption>
</figure>
<ol start="5">
  <li>끝나면 <strong>Congratulations!</strong> 화면이 나옵니다. <strong>Continue to Dashboard</strong>를 눌러 우리 사이트 주소 <code>...vercel.app</code> 을 확인합니다.</li>
</ol>
<figure>
  <img src="../assets/vercel/05-deploy-success.png" alt="배포 성공 화면">
  <figcaption>배포 성공 화면</figcaption>
</figure>
<figure>
  <img src="../assets/vercel/06-project-dashboard.png" alt="프로젝트 대시보드">
  <figcaption>프로젝트 대시보드 — 여기서 사이트 주소를 확인합니다</figcaption>
</figure>
<div class="note">지금 사이트를 열면 팀 이름이 <strong>"우리 팀 사이트"</strong>라는 기본값으로 보입니다. 5단계에서 환경변수를 넣어 바꿉니다. <strong>여기까지가 웹 화면에서 하는 전부입니다.</strong></div>

<hr>
```

- [ ] **Step 5: 비교표 정정**

"웹 버전과 나란히 보기" 표에서 0~3단계에 해당하는 행들의 "달라지는 점"을 `동일 (웹 화면에서 진행)` 로 바꾼다. 위치는 다음으로 찾는다.

```bash
grep -n "나란히 보기" -A 20 data/Vercel-배포-따라하기-MCP.html
```

- [ ] **Step 6: 검증**

```bash
cd /Users/peter/Repo/sangsangin_edu_3/Lesson3
grep -c "기존 문서 그대로" data/Vercel-배포-따라하기-MCP.html
for p in $(grep -o 'src="\.\./assets/[^"]*"' data/Vercel-배포-따라하기-MCP.html | sed 's/src="\.\.\///;s/"//'); do [ -f "$p" ] || echo "MISSING: $p"; done
grep -o 'id="step[0-9]"' data/Vercel-배포-따라하기-MCP.html | sort -u
```

기대 출력:
- 첫 명령: `0`
- 두 번째 명령: 출력 없음
- 세 번째 명령: `id="step0"` 부터 `id="step8"` 까지 9개가 중복 없이 나온다

- [ ] **Step 7: 브라우저 육안 확인**

```bash
open /Users/peter/Repo/sangsangin_edu_3/Lesson3/data/Vercel-배포-따라하기-MCP.html
```

확인 항목: 목차 9개 링크가 모두 대응 섹션으로 이동하는가, 0~3단계 이미지 11장이 모두 보이는가, 3단계 다음에 4단계(통로 열기)가 이어지는가.

- [ ] **Step 8: 커밋**

```bash
cd /Users/peter/Repo/sangsangin_edu_3
git add Lesson3/data/Vercel-배포-따라하기-MCP.html
git commit -m "docs: Vercel MCP 문서의 0~3단계를 실제 절차로 교체"
```

---

### Task 3: Supabase MCP 배포본 동기화

**Files:**
- Modify: `Lesson3/상상인AI교육_3회차_자료/Supabase-따라하기-MCP.html`
- Reference (읽기만): `Lesson3/data/Supabase-따라하기-MCP.html` (Task 1 결과)

**Interfaces:**
- Consumes: Task 1이 만든 `#prep`/`#prep-a`/`#prep-b` 섹션 문안
- Produces: 없음 (배포본은 최종 산출물)

배포본은 `data/` 버전과 **두 가지만 다르다**: 이미지 경로에 `../` 가 없고, `강사 메모` 섹션이 없다.

- [ ] **Step 1: 현재 차이가 그 두 가지뿐인지 확인**

```bash
cd /Users/peter/Repo/sangsangin_edu_3/Lesson3
diff <(sed 's|\.\./assets/|assets/|g' data/Supabase-따라하기-MCP.html) 상상인AI교육_3회차_자료/Supabase-따라하기-MCP.html
```

기대 출력: Task 1에서 추가한 준비 섹션 블록과, 배포본에 없는 `강사 메모` 섹션만 차이로 나온다. 그 외의 차이가 보이면 **작업을 멈추고 보고**한다.

- [ ] **Step 2: Task 1의 변경 6곳을 배포본에 동일하게 적용**

Task 1의 Step 2·3·4·5·6을 그대로 적용하되, 삽입하는 `<img src=...>` 경로에서 `../` 를 뺀다 (`assets/supabase/01-login.png` 형태). Task 1 Step 7(강사 메모)은 배포본에 해당 섹션이 없으므로 **적용하지 않는다.**

- [ ] **Step 3: 검증**

```bash
cd /Users/peter/Repo/sangsangin_edu_3/Lesson3
for p in $(grep -o 'src="assets/[^"]*"' 상상인AI교육_3회차_자료/Supabase-따라하기-MCP.html | sed 's/src="//;s/"//'); do [ -f "상상인AI교육_3회차_자료/$p" ] || echo "MISSING: $p"; done
grep -c '\.\./assets' 상상인AI교육_3회차_자료/Supabase-따라하기-MCP.html
diff <(grep -o '<h2[^>]*>.*</h2>' data/Supabase-따라하기-MCP.html | sed 's/<[^>]*>//g' | grep -v '강사 메모') <(grep -o '<h2[^>]*>.*</h2>' 상상인AI교육_3회차_자료/Supabase-따라하기-MCP.html | sed 's/<[^>]*>//g')
```

기대 출력: 첫 명령 출력 없음, 두 번째 `0`, 세 번째 차이 없음.

- [ ] **Step 4: 커밋**

```bash
cd /Users/peter/Repo/sangsangin_edu_3
git add "Lesson3/상상인AI교육_3회차_자료/Supabase-따라하기-MCP.html"
git commit -m "docs: Supabase MCP 배포본에 준비 섹션 동기화"
```

---

### Task 4: Vercel MCP 배포본 동기화

**Files:**
- Modify: `Lesson3/상상인AI교육_3회차_자료/Vercel-배포-따라하기-MCP.html`
- Reference (읽기만): `Lesson3/data/Vercel-배포-따라하기-MCP.html` (Task 2 결과)

**Interfaces:**
- Consumes: Task 2가 만든 `#step0`~`#step3` 섹션 문안
- Produces: 없음

- [ ] **Step 1: 현재 차이 확인**

```bash
cd /Users/peter/Repo/sangsangin_edu_3/Lesson3
diff <(sed 's|\.\./assets/|assets/|g' data/Vercel-배포-따라하기-MCP.html) 상상인AI교육_3회차_자료/Vercel-배포-따라하기-MCP.html
```

기대 출력: Task 2에서 교체한 0~3단계 블록과 `강사 메모` 섹션만 차이로 나온다.

- [ ] **Step 2: Task 2의 변경 4곳을 배포본에 동일하게 적용**

Task 2의 Step 2·3·4·5를 그대로 적용하되 이미지 경로에서 `../` 를 뺀다. 문서 간 링크가 있으면 배포본 파일명(`Vercel-배포-따라하기-웹버전-참고용.html`)을 쓴다.

- [ ] **Step 3: 검증**

```bash
cd /Users/peter/Repo/sangsangin_edu_3/Lesson3
for p in $(grep -o 'src="assets/[^"]*"' 상상인AI교육_3회차_자료/Vercel-배포-따라하기-MCP.html | sed 's/src="//;s/"//'); do [ -f "상상인AI교육_3회차_자료/$p" ] || echo "MISSING: $p"; done
grep -c '\.\./assets' 상상인AI교육_3회차_자료/Vercel-배포-따라하기-MCP.html
grep -o 'href="[^"#]*\.html"' 상상인AI교육_3회차_자료/Vercel-배포-따라하기-MCP.html | sort -u
```

기대 출력: 첫 명령 출력 없음, 두 번째 `0`, 세 번째는 `Supabase-따라하기-MCP.html` 과 `Vercel-배포-따라하기-웹버전-참고용.html` 만 (파일명에 `-웹버전-참고용` 없는 웹 문서 링크가 있으면 깨진 링크다).

- [ ] **Step 4: 커밋**

```bash
cd /Users/peter/Repo/sangsangin_edu_3
git add "Lesson3/상상인AI교육_3회차_자료/Vercel-배포-따라하기-MCP.html"
git commit -m "docs: Vercel MCP 배포본에 0~3단계 절차 동기화"
```

---

### Task 5: Markdown 원본 3종 동기화

**Files:**
- Modify: `Lesson3/data/Supabase-따라하기-MCP.md`
- Modify: `Lesson3/data/Vercel-배포-따라하기-MCP.md`
- Modify: `Lesson3/data/Vercel-배포-따라하기.md` (누락된 미러링 단계 역이식)

**Interfaces:**
- Consumes: Task 1·2의 HTML 문안
- Produces: 없음

`.md` 는 HTML보다 뒤처져 있다. HTML을 기준으로 맞춘다. HTML의 `<figure><img src="X" alt="A"><figcaption>C</figcaption></figure>` 는 마크다운에서 `![A](X)` + 다음 줄 `*C*` 형태로 쓴다(기존 문서 관례와 동일).

- [ ] **Step 1: Supabase MCP md에 준비 섹션 추가**

`## 준비물` 의 `- **Supabase 계정** (GitHub으로 가입 가능)` 항목을
`- **GitHub 계정** — Supabase 계정과 프로젝트는 아래 "시작 전 준비"에서 직접 만듭니다` 로 바꾸고,
흐름 코드블록을 아래로 교체한다.

```
[처음 한 번] A. 계정 만들기 → B. 프로젝트 만들기
0. 통로 열기(MCP 연결) → 1. 표 만들기 → 2. 데이터 넣기
   → 3. 읽기 공개 정책 → 4. 사이트에 연결 → 5. 화면 확인
```

그 다음 `## 0단계 — 통로 열기 (MCP 연결)` 앞에 `## 시작 전 준비 (처음 한 번만 · 웹 화면에서)` 섹션을 넣는다. 본문은 Task 1 Step 5의 HTML을 마크다운으로 옮긴 것으로, 이미지 경로는 `../assets/supabase/...` 를 그대로 쓴다.

- [ ] **Step 2: Supabase MCP md의 비교표·강사 메모 수정**

Task 1의 Step 6·7과 동일한 내용으로 고친다.

- [ ] **Step 3: Vercel MCP md의 0~3단계 교체**

`## 0~3단계 — 웹으로 (기존 문서 그대로)` 섹션 전체(다음 `---` 까지)를 Task 2 Step 4의 내용을 마크다운으로 옮긴 4개 섹션(`## 0단계` ~ `## 3단계`)으로 교체한다. 준비물에 강사 저장소 주소 항목도 추가한다.

- [ ] **Step 4: Vercel 웹 md에 미러링 단계 역이식**

`Lesson3/data/Vercel-배포-따라하기.md` 는 `.html` 에만 있는 "2단계 내 저장소 만들기(미러링)"이 빠져 있어 이후 단계 번호가 전부 하나씩 어긋나 있다. HTML(`data/Vercel-배포-따라하기.html:186-369`)을 기준으로 다음을 맞춘다.

- 준비물에 저장소 관련 두 항목 추가
- 흐름 설명을 8단계로 수정
- `## 2단계 — 내 저장소 만들기 (미러링)` 섹션 삽입 (HTML 258~299행 내용)
- 기존 2~7단계를 3~8단계로 번호 이동

- [ ] **Step 5: 검증 — md와 html의 단계 구성 대조**

```bash
cd /Users/peter/Repo/sangsangin_edu_3/Lesson3
echo "--- Supabase MCP ---"
diff <(grep -o '<h2[^>]*>.*</h2>' data/Supabase-따라하기-MCP.html | sed 's/<[^>]*>//g;s/단계/단계 — /') <(grep '^## ' data/Supabase-따라하기-MCP.md | sed 's/^## //') || true
echo "--- Vercel MCP 단계 수 ---"
grep -c '^## [0-8]단계' data/Vercel-배포-따라하기-MCP.md
echo "--- Vercel 웹 단계 수 ---"
grep -c '^## [0-8]단계' data/Vercel-배포-따라하기.md
```

기대 출력: Vercel MCP `.md` 의 단계 수 `9`(0~8단계), Vercel 웹 `.md` 의 단계 수 `9`(0~8단계). 첫 diff는 서식 차이로 완전히 일치하지 않을 수 있으므로 **단계 제목의 순서와 개수만** 눈으로 대조한다.

- [ ] **Step 6: 이미지 경로 존재 확인**

```bash
cd /Users/peter/Repo/sangsangin_edu_3/Lesson3
for f in data/Supabase-따라하기-MCP.md data/Vercel-배포-따라하기-MCP.md data/Vercel-배포-따라하기.md; do
  for p in $(grep -o '](\.\./assets/[^)]*)' "$f" | sed 's/](\.\.\///;s/)//'); do [ -f "$p" ] || echo "MISSING in $f: $p"; done
done
```

기대 출력: 없음.

- [ ] **Step 7: 커밋**

```bash
cd /Users/peter/Repo/sangsangin_edu_3
git add Lesson3/data/Supabase-따라하기-MCP.md Lesson3/data/Vercel-배포-따라하기-MCP.md Lesson3/data/Vercel-배포-따라하기.md
git commit -m "docs: 마크다운 원본을 HTML 기준으로 동기화 (미러링 단계 역이식 포함)"
```

---

### Task 6: index.html 문구 갱신 및 배포 묶음 재생성

**Files:**
- Modify: `Lesson3/상상인AI교육_3회차_자료/index.html`
- Create: `Lesson3/상상인AI교육_3회차_자료_20260819.zip`
- Delete: `Lesson3/상상인AI교육_3회차_자료_20260818.zip`

**Interfaces:**
- Consumes: Task 3·4의 배포본 HTML
- Produces: 없음 (최종 산출물)

- [ ] **Step 1: 본 교재 카드 설명 갱신**

Vercel 카드의 `<span>` 을 다음으로 바꾼다.

```html
  <span>로그인 → 내 저장소 만들기(미러링) → 배포 → 통로 열기 → 환경변수 → 재배포 → 막혔을 때</span>
```

Supabase 카드의 `<span>` 을 다음으로 바꾼다.

```html
  <span>계정·프로젝트 만들기 → 표 만들기 → 데이터 입력 → 읽기 공개 정책(RLS) → 사이트에 연결 → 화면에서 확인</span>
```

- [ ] **Step 2: 웹 버전 섹션 안내문 갱신**

웹 버전 섹션의 안내 `<p>` 를 다음으로 바꾼다.

```html
<p style="font-size:0.9em;color:#59636e;margin:-2px 0 4px">본 교재만으로 처음부터 끝까지 진행할 수 있습니다. 이 문서들은 <strong>화면이 어디에 있는지 눈으로 확인하고 싶을 때</strong> 참고용으로만 보세요.</p>
```

- [ ] **Step 3: 배포 묶음 재생성**

```bash
cd /Users/peter/Repo/sangsangin_edu_3/Lesson3
rm -f 상상인AI교육_3회차_자료_20260818.zip
zip -rq 상상인AI교육_3회차_자료_20260819.zip 상상인AI교육_3회차_자료 -x '*.DS_Store'
unzip -l 상상인AI교육_3회차_자료_20260819.zip | tail -3
```

기대 출력: 파일 개수가 60개 이상, 마지막 줄에 총 파일 수가 표시된다.

- [ ] **Step 4: 배포본 자체 완결성 최종 확인**

```bash
cd /Users/peter/Repo/sangsangin_edu_3/Lesson3/상상인AI교육_3회차_자료
grep -c "기존 문서 그대로\|강사 사전 준비" *.html
for f in *.html; do
  for p in $(grep -o 'src="assets/[^"]*"' "$f" | sed 's/src="//;s/"//'); do [ -f "$p" ] || echo "MISSING $f -> $p"; done
  for h in $(grep -o 'href="[^"#:]*\.html"' "$f" | sed 's/href="//;s/"//'); do [ -f "$h" ] || echo "BROKEN LINK $f -> $h"; done
done
```

기대 출력: 첫 명령의 모든 파일이 `0`, 이후 명령들은 출력 없음.

- [ ] **Step 5: 브라우저로 배포본 전체 확인**

```bash
open /Users/peter/Repo/sangsangin_edu_3/Lesson3/상상인AI교육_3회차_자료/index.html
```

확인 항목: index에서 MCP 문서 2개로 들어가 준비 섹션·0~3단계가 보이고, 이미지가 깨지지 않으며, 웹 버전 참고용 링크가 정상 동작한다.

- [ ] **Step 6: 커밋**

```bash
cd /Users/peter/Repo/sangsangin_edu_3
git add -A Lesson3/
git commit -m "docs: index 문구 갱신 및 배포 묶음 재생성"
```

---

## 완료 기준

1. `상상인AI교육_3회차_자료/` 의 MCP HTML 2개에 `기존 문서 그대로`·`강사 사전 준비` 문구가 없다.
2. MCP 문서만 읽고 계정 생성부터 사이트 확인까지 빠진 단계 없이 진행할 수 있다.
3. 모든 `img src` 와 문서 간 `href` 가 실제 파일을 가리킨다 (Task 6 Step 4).
4. `data/` 와 배포본의 차이가 asset 경로 접두사와 `강사 메모` 섹션 유무뿐이다 (Task 3·4 Step 1).
5. `.md` 와 `.html` 의 단계 구성이 일치한다 (Task 5 Step 5).
