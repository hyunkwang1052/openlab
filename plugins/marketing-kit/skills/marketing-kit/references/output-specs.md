# 산출물 4종 기술 셸 명세 (output-specs)

> 깎기 1순위(두 실험 합의): 4종의 **기술 셸**(슬라이드 크기·hash 네비·필름스트립 등)이 어느 스킬에도 없어 매번 재발명·재현 실패. 여기에 박제한다.
> **원칙: 골드 셸을 복제 후 토큰만 교체한다.** 색·문구가 아니라 **구조·스크립트**를 그대로 가져온다.
>
> **셸은 플러그인 안에 있다 — `marketing-kit` 스킬 폴더(스킬을 불러올 때 표시되는 Base directory)의 `references/shells/`{web,ppt,cardnews,namecard,index,og}.html** (라이트, 2026-09-10 실측 배포본 「새벽결」). 복제 후 토큰(:root)·문구·워드마크 SVG 만 바꾼다.
> 사장님 컴퓨터에 `_workspace/marketing-kit/hanmogeum/` 같은 골드 폴더는 **없다. 찾지 마라**(`find /` 금지).
> 다크 브랜드면 같은 셸에서 **클래스의 의미를 재매핑**한다:
> 1. 페이지/슬라이드 배경 토큰을 `--night`/`--surface`로(라이트 셸의 cream/paper 자리).
> 2. `.slide.light`(본문)·`.slide.dark`(표지) 등 **클래스 의미 재정의** — 다크 브랜드는 본문도 어두움(`.light`=night 배경).
> 3. 본문 텍스트는 밝게(`--text`), **강조어는 항상 네온(밝은 포인트 변형)** → 어두운 배경에 묻힘 방지.
> 4. CTA 푸터·명함 앞면·카드 1·5 등 "주색 배경" 자리는 `--brand`(딥 톤).

## 공통 (4종 전부)
- `<head>`에 `color-typography-specs.md`의 **link(폰트) + :root 토큰 + 공통 클래스를 그대로 인라인**. hex·폰트 변경 금지.
- 외부 이미지 0개(무드·심볼은 인라인 SVG/CSS). 폰트 CDN만 의존 — **4종과 index 가 같은 폰트**(브랜드 스킬의 것 하나).
- **포인트색은 화면당 1곳.** 어두운 배경 위 강조어는 '밝은 포인트 변형' 사용(묻힘 방지).
- 단일 자체완결 HTML 1파일.
- `<meta name="viewport" content="width=device-width, initial-scale=1">` 필수(폰에서 열린다). `web.html`은 390px 폭에서 가로 스크롤 0.
- **처음으로(모음 페이지) 돌아가기 — 4종(`web`·`ppt`·`cardnews`·`namecard`) 모두 필수.** 슬라이드·카드뉴스는 화면을 꽉 채워서 들어가면 index 로 돌아갈 길이 없다(사장님 실사용 피드백, 2026-10-11). 셸에 이미 들어 있으니 지우지 말 것:
  - ppt·cardnews: 하단 `.controls` 맨 앞에 `<a class="home-link" href="index.html">⌂ 처음으로</a>`.
  - namecard: `<body>` 바로 뒤 좌상단 고정 `<a class="home-link fixed" href="index.html">← 처음으로</a>`.
  - web: 헤더 `<nav>` **맨 앞**에 `<a class="home-link" href="index.html">⌂ 처음으로</a>`. 폰 폭(≤560px)에서 다른 메뉴는 숨어도 이 버튼은 남는다(`header nav{gap:10px}`). 랜딩이라 Esc 키는 넣지 않는다(실수로 페이지가 바뀌는 것 방지). (사장님 피드백, 2026-10-11)
  - 공통: `.home-link` CSS(토큰만 사용, 포인트색 X). ppt·cardnews·namecard 는 **Esc 키 → `index.html`** 도.
- 오픈그래프 자리 5줄(`og:title`·`og:description`·`og:type`·`og:url`·`og:image`)을 `<head>`에 미리 둔다 — 값은 Phase 4(올리기)에서 채운다. 배포 전엔 `og:url`·`og:image` 를 비워 둬도 된다.

## ① web.html — 랜딩 한 페이지
- 중앙 ~1120px, 반응형 2 브레이크포인트. 섹션: 헤더(워드마크 좌상단+메뉴+CTA) → 히어로(슬로건 큰 디스플레이 폰트+보조카피+주 CTA 1개+무드 SVG) → 이야기(01_brand 사실만) → 제품 3카드 → 후기 1~2 → CTA 푸터(보통 다크).

## ② ppt.html — 회사소개 슬라이드 (가장 까다로움)
- **1280×720 슬라이드 6장.** `#stage`(1280×720)를 `transform:scale()`로 화면 중앙 자동 맞춤.
- **키보드 ←/→ 넘김 + `location.hash`(#1~#6)로 슬라이드 점프**(스크린샷 필수). 우하단 페이지표시(N/6). **좌하단 워드마크 작게**(전 슬라이드 공통, JS로 주입).
- 표지=주색(어두운) 배경+밝은 글자(로고도 밝게), 본문=밝은 배경.
- 핵심 JS(골드에서 복제):
```js
const stage=document.getElementById('stage'), els=[...document.querySelectorAll('.slide')];
let cur=0;
function show(n){cur=Math.max(0,Math.min(els.length-1,n));els.forEach((e,i)=>e.classList.toggle('on',i===cur));location.hash=cur+1;}
function fit(){stage.style.transform='scale('+Math.min(innerWidth/1280,innerHeight/720)+')';}
addEventListener('resize',fit);
addEventListener('keydown',e=>{if(['ArrowRight',' '].includes(e.key))show(cur+1);else if(e.key==='ArrowLeft')show(cur-1);});
addEventListener('hashchange',()=>{const n=parseInt(location.hash.slice(1))||1;if(n-1!==cur)show(n-1);});
fit();show((parseInt(location.hash.slice(1))||1)-1);
```
- 6장: 표지 · 문제 · 약속 · 제품 · 숫자/약속 · 맺음+연락처.

## ③ cardnews.html — 정사각 5장
- 카드 1장 = **1080×1080** 디자인. 5장을 **가로 필름스트립**으로(컨테이너에서 `transform:scale(--s)` ≈ .3 축소 → 한 화면에 5장 미리보기).
- 장당 한 메시지·큰 디스플레이 폰트·포인트색 강조 1곳·하단 워드마크. 1·5번은 주색 배경.
- 1 표지(슬로건) · 2 문제 · 3 전환 · 4 약속/제품 · 5 CTA+링크.

## ④ namecard.html — 명함 + 메일 서명
- 크림/밝은 배경 위 세로 배치: 명함 **앞면(900×500≈90×50mm, 주색 배경+밝은 워드마크+슬로건)** + **뒷면(밝은 배경, 이름 디스플레이 폰트·직함·연락처)** 가로 나란히 → 아래 **메일 서명 블록**(워드마크 작게 | 구분선 | 이름·직함·브랜드·슬로건·링크).
- 사장님이 말하지 않은 정보(가격·영업시간·주소·전화번호 등)는 지어내지 않는다. 비어 있으면 자리만 두고 사장님께 알린다.

## 빌더 게이트 체크 (제출 전)
- □ 4종 토큰이 `color-typography-specs.md`와 1:1 동일한가
- □ 포인트색이 주색과 다른 색상 계열이고 화면당 1곳인가
- □ 어두운 배경 위 강조어가 묻히지 않는가(밝은 변형 사용)
- □ ppt가 hash #1~#6로 점프되고 auto-fit 되는가
- □ 외부 이미지 0·폰트 CDN만인가
- □ 4종 모두 "처음으로" 버튼(→ index.html)이 있는가 (ppt·cardnews·namecard 는 Esc 키도)
