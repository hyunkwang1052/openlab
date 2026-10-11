---
name: marketing-kit
description: 소상공인 사장님의 "내 가게·사업 홍보 컨텐츠"를 인터뷰→가게 이름·로고→디자인시트(브랜드 스킬)→홍보물 4종(웹·소개 PPT·카드뉴스·명함)까지 끝까지 만드는 오케스트레이터. "내 홍보 컨텐츠를 만들어줘", "우리 가게 홍보물 만들어줘", "가게 홍보 웹·명함·카드뉴스까지", "브랜드 키트 만들어줘"를 요청받으면 반드시 사용. 단계마다 확인받고 중간 산출물을 보여주며, 각 단계를 직접 할 때의 프롬프트도 알려준다(교육). 4종이 끝나면 인터넷에 올리기(Vercel 배포 · 오픈그래프 · 모바일 점검)까지 묻고 해 준다. 후속 "이어서/디자인만 다시/홍보물만 다시/다른 가게 이어서/올려 줘/오픈그래프 예쁘게"도 이 스킬.
---

# 소상공인 홍보 컨텐츠 자동 제작 (marketing-kit)

디지털이 낯선 **소상공인 사장님의 "내 가게·사업"** 을, 따뜻한 인터뷰로 끌어내 **디자인 시트(브랜드 스킬)** 를 만들고, 그 한 벌의 옷으로 **웹·소개 PPT·카드뉴스·명함 4종** 홍보물을 깔맞춤해 만든다. 핵심은 **디자인 시트를 스킬로 패키징해 재사용(단골집)** 하는 것 — 한 번 만들어 두면 다음 홍보물도 같은 색·폰트로 나온다.

## 두 가지 약속
1. **단계마다 멈춰 확인받고 중간 산출물을 보여준다.** (게이트)
2. **교육 목적** — 단계가 끝나면 "직접 하려면 이렇게 쳐보세요" 프롬프트를 보여준다.

## 큰 메시지 (마무리에서)
AI는 목적이 아니라 **내 가게를 알리는 수단**. 이 홍보 키트는 사장님이 오래 지켜 온 가게의 이야기를 손님에게 전하는 첫걸음이다. (감동 마무리)

---

## Phase 0: 컨텍스트 확인 (초기 / 후속 / 부분 재실행)
슬러그를 정한다(가게 이름 → 영문, 예: `hanmogeum`). 작업 폴더 **`_workspace/marketing-kit/<slug>/`**.
- 폴더 없음 → 초기 실행(1단계부터).
- 폴더 있음 + "디자인만 다시/산출물만 다시" → 해당 단계만(앞 산출물 재사용).
- 폴더 있음 + 새 인터뷰 → 기존을 `<slug>_prev/`로 옮기고 새로.

부분 재실행: |요청|1 인터뷰|2 디자인시트|3 산출물| / "인터뷰 보완"→1 / "디자인 다시"→2,3 / "명함만 다시"→3.

---

## 실행 모델
- **1단계는 메인 대화(실시간)** — 사장님과 주고받음. `marketing-interview` 스킬로 리더(=`marketing-interviewer`)가 직접.
- **2단계는 혼합:** 슬로건·레퍼런스 **선택은 메인 대화(실시간)**, 디자인시트 **추출·패키징만 서브에이전트**.
  - ⚠️ **선택을 서브에이전트에 넘기지 말 것.** 서브에이전트는 사장님과 실시간 대화를 못 해서 "레퍼런스 골라라"를 못 묻고 **자동 선택돼 버린다(실제 버그).** 선택은 반드시 메인에서 묻고, 고른 결과만 서브에이전트에 넘긴다.
- **3단계는 서브에이전트**(`Agent`, **model: "opus"**).
- 데이터는 **파일로 전달**: `_workspace/marketing-kit/<slug>/`. 최종 산출물은 `_workspace/marketing-kit/<slug>/`.

---

## Phase 1 — 브랜드 인터뷰 (메인 대화)
**스킬:** `marketing-kit:marketing-interview` · **담당:** 리더 = `marketing-interviewer`
1. 따뜻하게 한 턴씩 → `01_brand.md`(재료 + 가게 이름 + 슬로건 후보 5 + 손님 + 가게 정보 + 무드).
2. 보여주고 "고칠 데 있으세요?" (게이트 ①).
3. 교육: "'내 가게 인터뷰해줘'라고 치면 돼요."

## Phase 2 — 디자인 시트 = 브랜드 스킬
**스킬:** `marketing-kit:marketing-sheet` · **에이전트:** `marketing-sheet-maker`

**2a. 선택 (메인 대화 — ⚠️ 서브에이전트로 넘기지 말 것):**
1. `01_brand.md`의 **슬로건 후보 5개를 보여주고 사장님이 하나 고르게** 한다. 가게 이름은 인터뷰에서 확정된 것을 쓴다(이름이 없던 경우만 이름 후보 5개에서 함께 고른다).
2. 브랜드 결(카테고리·무드·청중)에 맞는 **레퍼런스 사이트 2~3개를 이유 한 줄과 함께 추천 → 사장님이 하나 고른다.** **반드시 사장님 답을 기다린다**(실시간 문답). 사장님이 정말 없는 무인 배치 실행일 때만 무드에 맞는 1개 기본 선택 + 표기.

**2b. 빌드 (서브에이전트):**
3. **2a에서 확정한 가게 이름·슬로건·고른 레퍼런스를 입력으로** `Agent(subagent_type:"marketing-kit:marketing-sheet-maker", model:"opus")`: 디자인 시트 추출 → **워드마크 로고**(이미지 생성 X) → **브랜드 스킬 패키징** `_workspace/marketing-kit/<slug>/brand-skill/`(SKILL.md+specs+usage-examples+mark.svg).
4. 색·폰트·로고 카탈로그를 보여준다 (게이트 ②: "이 느낌 맞아요?").
5. 교육: "'이 사이트 보고 디자인 시트 만들어줘' 한 줄이면 돼요."

## Phase 3 — 산출물 4종 (서브에이전트)
**서브에이전트를 부르기 전에 한 줄 말한다**: "넷을 만드는 데 3~5분 걸려요. 화면이 조용해도 멈춘 게 아닙니다." (실측: 이 사이 5~10분 침묵 → 사장님이 Ctrl+C 를 누른다.) Phase 2b 도 같다.
**에이전트:** `marketing-kit-builder` (별도 빌더 스킬 없이 **브랜드 스킬** + 본체로 생성 = brand-as-skill)
1. `Agent(subagent_type:"marketing-kit:marketing-kit-builder", model:"opus")`로: `brand-skill/`의 토큰을 **그대로 써서** 4종을 `_workspace/marketing-kit/<slug>/outputs/`에 만든다 — `web.html`·`ppt.html`(로고 좌하단)·`cardnews.html`(정사각 5장)·`namecard.html`(앞뒤+메일서명).
   - **기술 셸은 `references/output-specs.md`를 따른다.** ppt(hash·auto-fit)·cardnews(필름스트립) 같은 셸은 brand-skill에 없으니 **플러그인 안 셸(`marketing-kit` 스킬 폴더(스킬을 불러올 때 표시되는 Base directory)의 `references/shells/`{web,ppt,cardnews,namecard,index,og}.html — 빌더 호출 때 이 **절대 경로**를 프롬프트에 적어 넘긴다)을 복제 후 토큰·문구만 교체**(매번 재발명 금지).
   - ⚠️ **사장님 컴퓨터에는 `_workspace/marketing-kit/hanmogeum/` 같은 골드 폴더가 없다. 찾지 마라.** `find / …`·`find ~ …` 같은 넓은 검색 금지(실측: 5분 낭비 + 권한 경고). 셸은 위 `references/shells/` 뿐이다.
2. 게이트 ③ 체크: □ 토큰이 specs와 1:1 □ 포인트색 주색과 다른 계열·화면당 1곳 □ **어두운 배경 강조어 안 묻힘** □ ppt hash#1~6 점프. **이 네 줄로 끝낸다** — 대비비 계산·`node --check`·4종 전부 캡처 같은 추가 검사는 하지 않는다(실측: 11분 중 절반이 여기서 샜다). 캡처는 web.html 한 장만. → 여는 법 보여주고 부분 수정 받음.
3. 교육: "'내 브랜드로 카드뉴스 만들어줘'처럼 브랜드 스킬이 자동 적용돼요."
> 골드 기준: `references/shells/` 셸의 품질·일관성을 목표로.

---

## Phase 4 — 올리기 (인터넷에 배포 · 게이트 ④ · 선택) — 메인 대화
게이트 ③이 끝나면 **묻는다**: "인터넷에 올려서 링크로 공유할까요? (Vercel)". "네"면 아래를 **한 번에 이어서** 한다(중간에 되묻지 않는다). "아니요"면 여는 법만 안내하고 마무리.

1. **모음 페이지** `outputs/index.html` — 워드마크·슬로건·한 줄 소개 + 4종으로 가는 카드 4개(web·ppt·cardnews·namecard). 브랜드 스킬 토큰 그대로, 자체완결 1파일.
2. **모바일** — 5개 파일(index + 4종) `<head>`에 `<meta name="viewport" content="width=device-width, initial-scale=1">`. `web.html`·`index.html`은 폭 390px에서 가로 스크롤이 없게. **판정은 캡처가 아니라 숫자로**: 크롬 헤드리스 `--headless=new --window-size=390,900 --dump-dom` 이 아니라, 간단히 CSS 를 본다 — 고정 폭(`width:1120px` 같은 px 고정)이 있으면 `max-width:100%` 로 바꾼다. **캡처를 보고 "깨졌다"고 판단하지 않는다**(구형 `--headless` 는 잘린 캡처를 만든다 — 실측 5분 낭비). **`npm install`·puppeteer 설치 금지.**
3. **오픈그래프(카톡·문자 미리보기 카드)** — 5개 파일 `<head>`에 5줄: `og:title`(가게 이름 — 슬로건) · `og:description`(01_brand의 한 줄 소개, 60자 안) · `og:type` website · `og:url`(배포 뒤 **그 파일의** 절대 주소 — web.html 은 `…/web.html`) · `og:image`(아래 `og.png`의 **절대 주소**). 파일마다 title은 "가게 이름 — 웹/가게소개/카드뉴스/명함"으로.
4. **미리보기 그림** `og.html`(1200×630 · 브랜드 배경색 · 워드마크 SVG 크게 · 슬로건 · 폰트 CDN 없이 시스템 폰트) → 크롬 헤드리스로 `og.png`:
   - 맥 `"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --disable-gpu --hide-scrollbars --window-size=1200,630 --screenshot=og.png og.html`
   - 윈도우 `"C:\Program Files\Google\Chrome\Application\chrome.exe" --headless=new --disable-gpu --hide-scrollbars --window-size=1200,630 --screenshot=og.png og.html` (없으면 `%LOCALAPPDATA%\Google\Chrome\Application\chrome.exe`)
   - 크롬이 없으면 **`og:image` 줄만 빼고 계속 간다**(멈추지 않는다). 카드에 그림만 안 뜬다.
5. **배포** — `outputs/` 안에서 `vercel deploy --prod --yes --name <slug>` (로그인이 안 되어 있으면 먼저 `vercel login`을 안내한다. 로그인 창이 뜨면 사장님이 직접 누른다). 처음 배포는 주소를 알기 위한 것 — 나온 `https://….vercel.app` 을 `og:url`·`og:image`(`https://…/og.png`)에 넣고 **같은 명령으로 한 번 더** 배포한다(오픈그래프 그림은 절대 주소여야 카톡이 읽는다). preview 가 아니라 **production**(`--prod`) 이어야 주소가 고정된다.
6. 사장님께 주소를 보여주며: "이 주소를 카톡에 붙이면 미리보기 카드가 뜹니다. 카드가 옛것으로 보이면 **그 메시지를 지우고 다시 보내면** 카톡이 새로 읽어 옵니다."
7. 게이트 ④ 체크: □ 주소가 열린다 □ 폰 폭에서 안 깨진다 □ 카톡에 붙였을 때 제목·설명·그림이 뜬다(그림은 크롬 없으면 생략).
8. 교육: "'지금 만든 브랜드 키트를 Vercel 로 올려 줘. 오픈그래프랑 모바일도 챙겨서.' 한 줄이면 돼요."

---

## 데이터 흐름
```
[메인]  Phase1 인터뷰 ─────────────→ 01_brand.md
[marketing-sheet-maker·서브] 01_brand → brand-skill/ (디자인시트=브랜드스킬 + 로고)
[marketing-kit-builder·서브] brand-skill → outputs/{web,ppt,cardnews,namecard}.html
[메인]  Phase4 올리기 ──────────────→ outputs/index.html + og.png → https://<slug>.vercel.app (선택)
```
중간물 `_workspace/marketing-kit/<slug>/` 보존. 최종 `_workspace/marketing-kit/<slug>/`.

## 에러 핸들링
- 서브에이전트 실패 시 1회 재시도, 그래도 실패면 그 산출물 없이 멈추고 보고(임의 추정 금지).
- 앞 단계 부실(예: `01_brand.md` 빈약) → 다음으로 안 넘기고 보완 권유.
- 색은 사이트 픽셀이 아니라 **무드(인터뷰 7번)**에서 정한다(marketing-sheet 정직화). 아는 브랜드면 대표색 근사. **둘 다 불가 → 무드 키워드로 팔레트 직접 생성**(베이스·포인트1·중립3) + ⚠️ 표기.
- 사실 보존: `01_brand.md`에 없는 경력·수치·고유명사 금지.

## 모델·윤문 규칙
- 모든 `Agent` 호출 **model: "opus"** (문자 그대로 `model` 인자를 넣는다 — 실측에서 빠진 적 있음). Phase 1·2·3·4 끝마다 「직접 하려면 이렇게」 교육 프롬프트를 **빠뜨리지 않는다**(실측: 2·3 누락). 사장님께 보이는 한국어 산문은 `humanize-korean`(별도 플러그인 — 설치돼 있으면 사용, 없으면 같은 원칙으로 직접 다듬기)(명령어·URL·구조·hex 보존).

## 테스트 시나리오
- **정상:** "내 홍보 컨텐츠를 만들어줘" → 슬러그 `hanmogeum` → Phase1 인터뷰 → 게이트① OK → Phase2 `marketing-sheet-maker`가 레퍼런스(차 브랜드) 추천→추출→브랜드 스킬 → 게이트② OK → Phase3 `marketing-kit-builder`가 4종 → 게이트③ → **Phase4 "올릴까요?" → 네 → index·viewport·오픈그래프·og.png·`vercel deploy --prod` 두 번 → 주소 안내.** 단계마다 교육 프롬프트.
- **에러:** Phase2에서 레퍼런스 색 접근 실패 → 근사 색 + ⚠️ 표기로 진행.
- **부분 재실행:** "명함만 다른 색으로 다시" → Phase0이 폴더 감지 → Phase3만(브랜드 스킬 색 조정 후) 재실행.

## 깎기 포인트 (v1)
- 한모금 정답지와 비교해 산출물 품질·일관성 갭이 보이면 해당 스킬을 일반화해 보강.
