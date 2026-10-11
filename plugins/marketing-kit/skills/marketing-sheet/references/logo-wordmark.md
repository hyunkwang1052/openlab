# 워드마크 로고 만들기 (이미지 생성 없이)

AI 이미지 생성은 설치 부담·실패·결과 편차가 커서 **라이브엔 안 쓴다.** 대신 **명조 글자(가게 이름) + 작은 SVG 심볼** 락업으로 만든다 — 무설치·안 깨짐·어디든 작게 박힌다.

## 구성
1. **워드마크(글자):** 가게 이름을 제목 폰트(**브랜드 톤대로 명조 또는 굵은 산세리프**)로. 자간 살짝 좁게.
2. **심볼(SVG):** 브랜드를 상징하는 단순 도형 1개. 톤별 예시 — 따뜻/공예: 찻잔·잎·나이테 / **강렬·스포츠·테크: 번개·셰브론·과녁·픽토그램**. 상징물이 없으면 **핵심 동작·재료·결과물** 중 하나를 단순 도형으로(차=찻잔, 목공=나이테, 운동=번개).
   - ⚠️ stroke/fill을 **`currentColor`로** 쓰면 부모 글자색을 따라가 밝은/어두운 배경에 **한 벌로 재사용**(배경마다 색 사본 만들 필요 없음).
3. **락업:** 심볼 + 글자를 가로로 묶는다. 어두운 배경에선 부모 색만 밝게 주면 심볼·글자가 같이 반전(currentColor 덕).

## 심볼 SVG 예시 (한모금: 찻잔+김)
```html
<svg width="30" height="30" viewBox="0 0 64 64" fill="none" aria-label="브랜드">
  <path d="M26 13c-2.2-2.4-2.2-4.8 0-7.2M32 13c-2.2-2.4-2.2-4.8 0-7.2M38 13c-2.2-2.4-2.2-4.8 0-7.2" stroke="#2f4a3c" stroke-width="2.4" stroke-linecap="round"/>
  <path d="M14 22h32v12a16 16 0 0 1-16 16 16 16 0 0 1-16-16V22Z" fill="#2f4a3c"/>
  <path d="M46 26h4a6 6 0 0 1 0 12h-4" stroke="#2f4a3c" stroke-width="3.2" fill="none" stroke-linecap="round"/>
  <path d="M12 56h36" stroke="#2f4a3c" stroke-width="3.2" stroke-linecap="round"/>
</svg>
```
→ 브랜드에 맞게 도형·색만 바꾼다. `mark.svg`로 저장.

## 락업 스니펫 (복붙)
```html
<span style="display:inline-flex;align-items:center;gap:9px">
  <!-- 위 SVG 인라인 -->
  <b style="font-family:var(--serif);font-size:24px;color:var(--primary);letter-spacing:-1px">가게 이름</b>
</span>
```
- 크기만 바꿔 재사용: PPT는 좌하단 작게, 명함은 중앙 크게.
- 어두운(주색) 배경에선 stroke/fill·글자를 배경 크림색으로.
