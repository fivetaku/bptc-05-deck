---
marp: true
theme: bptc-light
size: 16:9
paginate: false
lang: ko
title: "BPTC 5회차 — 토큰 절약 + 데이터베이스 실습"
description: "Claude Code 토큰 6가지 팁 + 부산 도시철도·부동산·주식 SQL 실습"
author: "이철로 (지피타쿠)"
---

<style>
section.title-lock { justify-content: flex-start; }
section.title-lock h1 { margin-top: 0; margin-bottom: 0; }
section, section p, section li, section div { word-break: keep-all; overflow-wrap: break-word; }
.kpi { font-family: var(--font-latin); font-weight: 800; color: #FC1C49; }
.muted { color: #5B5B5B; }
code { font-size: 0.85em; }
</style>

<!-- _class: banner -->

<div class="tag">[ 부산 신선대감만터미널 · 2026-05-27 ]</div>

# BPTC AX 교육 5회차<br/><span style="font-size:0.42em; color:#5B5B5B;">토큰 절약 + 데이터베이스</span>

<div class="by">By. 이철로 (지피타쿠)</div>

<!--
Speaker note:
오늘 5회차는 두 갈래입니다.
PART 1은 클로드 코드를 더 오래 쓰기 위한 토큰 절약.
PART 2는 부산 도시철도·부동산·주식 데이터로 SQL 실습.
4회차에 도서관 ERD까지 그렸으니, 오늘은 진짜 데이터를 만져봅니다.
-->

---

<!-- _class: title-lock -->

# 오늘 배울 것

<div style="display:flex; flex-direction:column; gap:36px; margin-top:48px;">

<div style="background:#FFE5EB; border-radius:18px; padding:44px 52px;">
<div class="kpi" style="font-size:22px; letter-spacing:0.04em;">PART 1</div>
<div style="font-size:40px; font-weight:800; line-height:1.35; margin-top:10px;">"갑자기 토큰 사용량이 급증했다면?"</div>
<div style="font-size:24px; color:#5B5B5B; margin-top:14px; line-height:1.55;">
ccusage로 본인 사용량을 먼저 확인합니다.<br/>
이후 토큰이 새는 자리 여섯 군데를 어떻게 막는지 알아봅니다.
</div>
</div>

<div style="background:#F5F5F5; border-radius:18px; padding:44px 52px;">
<div class="kpi" style="font-size:22px; letter-spacing:0.04em;">PART 2</div>
<div style="font-size:40px; font-weight:800; line-height:1.35; margin-top:10px;">SQL을 외우지 않고 AI에게 시키는 법</div>
<div style="font-size:24px; color:#5B5B5B; margin-top:14px; line-height:1.55;">
질문을 말로 풀어 설명하면 AI가 쿼리를 만들어준다.<br/>
부산 도시철도·부동산·주식 — 본인 동네·본인 종목 데이터로 직접 만져본다.
</div>
</div>

</div>

---

<!-- _class: banner -->

# PART 1

<div style="font-size:80px; font-weight:800; margin-top:40px;">토큰 절약</div>

<div style="font-size:32px; color:#5B5B5B; margin-top:24px;">왜 토큰이 빠르게 닳고, 어떻게 막을까</div>

<!--
Speaker note:
"한 달에 얼마나 쓰고 계세요?" 질문부터 시작합니다.
대부분 모름. ccusage 설치부터 같이 합니다.
-->

---

<!-- _class: title-lock -->

# 본인 토큰 사용량 보기 <span style="font-size:0.44em; color:#5B5B5B;">ccusage · 함께 실습</span>

<div style="display:grid; grid-template-columns:1.1fr 1fr; gap:28px; margin-top:28px;">

<div style="background:#F5F5F5; border-radius:18px; padding:28px;">
<div class="kpi" style="font-size:20px;">ccusage란?</div>
<div style="font-size:22px; line-height:1.65; margin-top:10px;">
Claude Code가 쌓아둔 사용 기록을<br/>
표·그래프로 보여주는 도구입니다.<br/>
<span style="font-size:18px;">(터미널에 명령어를 입력하는 CLI 방식)</span>
</div>
<div style="font-size:20px; color:#5B5B5B; margin-top:12px; line-height:1.6;">
보여주는 것<br/>
· 일별 / 월별 / 세션별 토큰<br/>
· 모델 종류별 비용 (Opus·Sonnet·Haiku)<br/>
· 캐시 적중률 / 입력·출력 토큰 분해
</div>
</div>

<div style="background:#FFE5EB; border-radius:18px; padding:28px;">
<div class="kpi" style="font-size:20px;">지금 같이 해봅시다</div>
<div style="font-family:monospace; font-size:20px; background:#fff; padding:14px; border-radius:8px; margin-top:12px; line-height:1.75;">
# 설치 (한 번만)<br/>
$ npm install -g ccusage<br/><br/>
# 일별 보기<br/>
$ ccusage daily<br/><br/>
# 세션별 보기<br/>
$ ccusage session<br/><br/>
# 실시간 모니터<br/>
$ ccusage blocks --live
</div>
</div>

</div>

<div style="font-size:22px; color:#5B5B5B; margin-top:22px; text-align:center;">
설치 → <code>ccusage daily</code> 한 번 → 본인 1주일 토큰 사용량 확인 → 옆 사람과 비교
</div>

<!--
Speaker note:
모두 같이 npm install 5분 줍니다.
설치 끝난 분 손 들어주세요.
ccusage daily 결과 띄워보면, 본인이 어느 날 유독 많이 썼는지 보입니다.
-->

---

<!-- _class: title-lock -->

# 어디서 토큰을 흘리고 있을까 <span style="font-size:0.44em; color:#5B5B5B;">vibe-sunsang 세션 분석</span>

<div style="margin-top:28px;">

<div style="background:#F5F5F5; border-radius:14px; padding:24px 32px; margin-bottom:18px;">
<div class="kpi" style="font-size:20px;">ccusage가 "얼마"라면, vibe-sunsang은 "왜"</div>
<div style="font-size:22px; line-height:1.6; margin-top:10px;">
대화 로그를 읽어, 어느 구간에서 토큰이 비정상적으로 튀었는지 짚어준다.<br/>
원인 유형까지 분류 — 매뉴얼 복붙 / 같은 질문 반복 / 사이드 잡담 / 컨텍스트 누적.
</div>
</div>

<div style="display:grid; grid-template-columns:1fr 1fr; gap:18px;">

<div style="background:#FFE5EB; border-radius:14px; padding:22px;">
<div class="kpi" style="font-size:18px;">자주 잡히는 패턴 ①</div>
<div style="font-size:22px; font-weight:800; margin-top:6px;">매뉴얼 복붙 누적</div>
<div style="font-size:18px; color:#5B5B5B; margin-top:6px;">같은 PDF·매뉴얼을 매 세션 첨부 → CLAUDE.md로 옮기면 한 번만 로드</div>
</div>

<div style="background:#FFE5EB; border-radius:14px; padding:22px;">
<div class="kpi" style="font-size:18px;">자주 잡히는 패턴 ②</div>
<div style="font-size:22px; font-weight:800; margin-top:6px;">긴 엑셀 통째 붙여넣기</div>
<div style="font-size:18px; color:#5B5B5B; margin-top:6px;">한 번 60K 토큰. @파일 참조로 바꾸면 즉시 해결</div>
</div>

<div style="background:#FFE5EB; border-radius:14px; padding:22px;">
<div class="kpi" style="font-size:18px;">자주 잡히는 패턴 ③</div>
<div style="font-size:22px; font-weight:800; margin-top:6px;">사이드 잡담을 메인에서</div>
<div style="font-size:18px; color:#5B5B5B; margin-top:6px;">"이거 뭐야?" 같은 짧은 질문이 이전 대화 재사용을 끊음 → /btw로 분리</div>
</div>

<div style="background:#FFE5EB; border-radius:14px; padding:22px;">
<div class="kpi" style="font-size:18px;">자주 잡히는 패턴 ④</div>
<div style="font-size:22px; font-weight:800; margin-top:6px;">/clear 없이 다주제</div>
<div style="font-size:18px; color:#5B5B5B; margin-top:6px;">한 세션에 3주제 → 컨텍스트 200K 누적. 주제 바꿀 때 /clear</div>
</div>

</div>
</div>

---

<!-- _class: title-lock -->

# 왜 캐시가 중요한가 <span style="font-size:0.44em; color:#5B5B5B;">Anthropic 공식</span>

<div style="display:grid; grid-template-columns:1fr 1fr; gap:28px; margin-top:40px;">

<div style="background:#F5F5F5; border-radius:18px; padding:40px;">
<div class="kpi" style="font-size:20px;">모델은 비싼 순서</div>
<div style="font-size:34px; font-weight:800; line-height:1.5; margin-top:18px;">
Opus &gt; Sonnet &gt; Haiku
</div>
<div style="font-size:24px; color:#5B5B5B; margin-top:16px;">
똑똑한 모델일수록 비쌈<br/>
Opus는 Sonnet의 약 5배
</div>
</div>

<div style="background:#FFE5EB; border-radius:18px; padding:40px;">
<div class="kpi" style="font-size:20px;">★ 캐시를 쓰면</div>
<div style="font-size:64px; font-weight:800; line-height:1.3; margin-top:18px; color:#FC1C49;">
90% 절감
</div>
<div style="font-size:24px; color:#5B5B5B; margin-top:16px;">
한 번 읽은 내용을<br/>
다시 안 읽어도 되니까
</div>
</div>

</div>

<div style="font-size:26px; margin-top:36px; text-align:center;">
<strong>비유</strong> — "어제 본 서류는 다시 안 봐도 됨" = 캐시 / 매번 인삿말부터 다시 = 캐시 깨짐
</div>

---

<!-- _class: title-lock -->

# 6가지 팁

<div style="display:grid; grid-template-columns:repeat(3, 1fr); gap:20px; margin-top:32px;">

<div style="background:#F5F5F5; border-radius:14px; padding:24px; min-height:280px;">
<div class="kpi" style="font-size:18px;">팁 1</div>
<div style="font-size:30px; font-weight:800; margin-top:10px;">CLAUDE.md<br/>다이어트</div>
<div style="font-size:20px; color:#5B5B5B; margin-top:12px;">매 세션마다 로드. 200줄 이하 권장.</div>
</div>

<div style="background:#F5F5F5; border-radius:14px; padding:24px; min-height:280px;">
<div class="kpi" style="font-size:18px;">팁 2</div>
<div style="font-size:30px; font-weight:800; margin-top:10px;">/context로<br/>점검</div>
<div style="font-size:20px; color:#5B5B5B; margin-top:12px;">지금 가방에 뭐가 들었는지 확인.</div>
</div>

<div style="background:#F5F5F5; border-radius:14px; padding:24px; min-height:280px;">
<div class="kpi" style="font-size:18px;">팁 3</div>
<div style="font-size:30px; font-weight:800; margin-top:10px;">추론 모드<br/>끄기</div>
<div style="font-size:20px; color:#5B5B5B; margin-top:12px;">단순 작업에 thinking 풀로 쓰면 5~10배.</div>
</div>

<div style="background:#FFE5EB; border-radius:14px; padding:24px; min-height:280px;">
<div class="kpi" style="font-size:18px;">팁 4 ★</div>
<div style="font-size:30px; font-weight:800; margin-top:10px;">/btw로<br/>잡담</div>
<div style="font-size:20px; color:#5B5B5B; margin-top:12px;">사이드 질문, 대화 히스토리에 안 남음.</div>
</div>

<div style="background:#FFE5EB; border-radius:14px; padding:24px; min-height:280px;">
<div class="kpi" style="font-size:18px;">팁 5 ★</div>
<div style="font-size:30px; font-weight:800; margin-top:10px;">붙여넣기 대신<br/>@파일명</div>
<div style="font-size:20px; color:#5B5B5B; margin-top:12px;">500줄 복붙 = 매 턴 500줄 재전송.</div>
</div>

<div style="background:#F5F5F5; border-radius:14px; padding:24px; min-height:280px;">
<div class="kpi" style="font-size:18px;">팁 6</div>
<div style="font-size:30px; font-weight:800; margin-top:10px;">MCP 정리</div>
<div style="font-size:20px; color:#5B5B5B; margin-top:12px;">4개 = 7,000 토큰/턴 오버헤드.</div>
</div>

</div>

<!--
Speaker note:
6가지 중 별표 두 개는 가장 즉시 효과 큰 것.
/btw 한 번 써보면 "이걸 왜 몰랐지" 반응 옵니다.
-->

---

<!-- _class: title-lock -->

# 한 줄 메시지 5개 <span style="font-size:0.44em; color:#5B5B5B;">집에 가져갈 것</span>

<div style="display:flex; flex-direction:column; gap:14px; margin-top:36px;">

<div style="background:#F5F5F5; padding:24px 32px; border-radius:14px; border-left:8px solid #FC1C49;">
<div style="font-size:28px; font-weight:800;">1. AI 비서에게 매일 처음 자기소개 시키지 마세요</div>
<div style="font-size:20px; color:#5B5B5B; margin-top:6px;">CLAUDE.md 200줄 이하</div>
</div>

<div style="background:#F5F5F5; padding:24px 32px; border-radius:14px; border-left:8px solid #FC1C49;">
<div style="font-size:28px; font-weight:800;">2. ccusage로 본인 사용량 매주 점검</div>
<div style="font-size:20px; color:#5B5B5B; margin-top:6px;">모르면 새지 않는다</div>
</div>

<div style="background:#F5F5F5; padding:24px 32px; border-radius:14px; border-left:8px solid #FC1C49;">
<div style="font-size:28px; font-weight:800;">3. 붙여넣기 대신 @파일명</div>
<div style="font-size:20px; color:#5B5B5B; margin-top:6px;">서류 통째로 들지 말고 캐비넷 위치만</div>
</div>

<div style="background:#F5F5F5; padding:24px 32px; border-radius:14px; border-left:8px solid #FC1C49;">
<div style="font-size:28px; font-weight:800;">4. 잡담은 /btw, 본격 작업은 메인</div>
<div style="font-size:20px; color:#5B5B5B; margin-top:6px;">캐시 보존</div>
</div>

<div style="background:#F5F5F5; padding:24px 32px; border-radius:14px; border-left:8px solid #FC1C49;">
<div style="font-size:28px; font-weight:800;">5. MCP는 도구 가방 — 안 쓰는 도구는 빼세요</div>
<div style="font-size:20px; color:#5B5B5B; margin-top:6px;">/mcp로 점검</div>
</div>

</div>

---

<!-- _class: banner -->

# PART 2

<div style="font-size:80px; font-weight:800; margin-top:40px;">데이터베이스 실습</div>

<div style="font-size:32px; color:#5B5B5B; margin-top:24px;">부산 도시철도 · 부동산 · 주식 — 240만 행</div>

<!--
Speaker note:
PART 1에서 비서 가방을 가볍게 했으니, 이제 진짜 비서한테 일을 시킵니다.
지난 주에 도서관 ERD 그렸죠. 오늘은 진짜 데이터로.
-->

---

<!-- _class: title-lock -->

# 데이터베이스를 다루는 방식, 무엇이 달라졌나

<div style="display:grid; grid-template-columns:1fr 1fr; gap:32px; margin-top:36px;">

<div style="background:#F5F5F5; border-radius:18px; padding:32px;">
<div class="kpi" style="font-size:20px;">기존</div>
<div style="font-size:24px; line-height:1.9; margin-top:14px;">
SELECT·JOIN 문법<br/>
정규화·인덱스<br/>
트랜잭션·서브쿼리<br/>
ERD 작성
</div>
<div style="font-size:22px; color:#5B5B5B; margin-top:22px;">
이걸 다 익혀야 시작
</div>
</div>

<div style="background:#FFE5EB; border-radius:18px; padding:32px;">
<div class="kpi" style="font-size:20px;">이제</div>
<div style="font-size:24px; line-height:1.7; margin-top:14px;">
무엇을 물을지 정한다<br/>
&nbsp;&nbsp;↓<br/>
AI에게 자연어로 말한다<br/>
&nbsp;&nbsp;↓<br/>
돌아온 답을 검토한다
</div>
<div style="font-size:22px; color:#5B5B5B; margin-top:22px;">
질문 하나로 차트까지
</div>
</div>

</div>

<div style="font-size:24px; margin-top:30px; text-align:center;">
익혀야 할 것은 줄었습니다.<br/>
무엇을 묻고, 받은 답을 어떻게 볼지가 남았습니다.
</div>

---

<!-- _class: title-lock -->

# 샘플 데이터베이스 3종

<div style="display:grid; grid-template-columns:repeat(3, 1fr); gap:18px; margin-top:24px;">

<div style="background:#F5F5F5; border-radius:14px; padding:22px; min-height:420px;">
<div class="kpi" style="font-size:18px;">DB 1 · 부산 도시철도</div>
<div style="font-size:24px; font-weight:800; margin-top:6px;">bptc_busan.db</div>
<div style="font-size:18px; color:#5B5B5B; margin-top:4px;">66 MB · 5 테이블</div>

<div style="font-size:19px; line-height:1.7; margin-top:14px;">
<strong>lines</strong> 4개 노선<br/>
<strong>stations</strong> 112개 역 + 좌표<br/>
<strong>daily_passengers</strong><br/>
&nbsp;&nbsp;645,120행 · 시간별 승하차<br/>
<strong>transfers</strong> 환승 매트릭스<br/>
<strong>line_stats</strong> 노선별 통계
</div>

<div style="font-size:18px; color:#5B5B5B; margin-top:14px; line-height:1.55;">
컬럼: date · hour · station_name · line_no · direction(board/alight) · passengers
</div>
</div>

<div style="background:#F5F5F5; border-radius:14px; padding:22px; min-height:420px;">
<div class="kpi" style="font-size:18px;">DB 2 · 한국 주식</div>
<div style="font-size:24px; font-weight:800; margin-top:6px;">bptc_stocks.db</div>
<div style="font-size:18px; color:#5B5B5B; margin-top:4px;">83 MB · 7 테이블</div>

<div style="font-size:19px; line-height:1.7; margin-top:14px;">
<strong>stocks_master</strong> 시가총액 TOP 200<br/>
<strong>daily_price</strong> 일별 OHLCV 250K행<br/>
<strong>daily_marcap</strong> 외국인 보유율<br/>
<strong>fundamental_monthly</strong> PER·PBR·EPS<br/>
<strong>investor_flow</strong> 외국인·기관·개인 매매<br/>
<strong>indexes</strong> 코스피·코스닥<br/>
<strong>index_ohlcv</strong> 지수 일별
</div>

<div style="font-size:18px; color:#5B5B5B; margin-top:14px; line-height:1.55;">
기간: 2021-01 ~ 2026-05 (5년)
</div>
</div>

<div style="background:#FFE5EB; border-radius:14px; padding:22px; min-height:420px;">
<div class="kpi" style="font-size:18px;">DB 3 · 부산 부동산</div>
<div style="font-size:24px; font-weight:800; margin-top:6px;">bptc_realestate.db</div>
<div style="font-size:18px; color:#5B5B5B; margin-top:4px;">149 MB · 11 테이블</div>

<div style="font-size:19px; line-height:1.7; margin-top:14px;">
<strong>apt_trade</strong> 매매 165K행<br/>
<strong>apt_rent</strong> 전월세 351K행<br/>
<strong>apt_complex</strong> 단지 마스터<br/>
<strong>schools</strong> 학교 615개<br/>
<strong>academies</strong> 학원 6,725개 · 입시강좌<br/>
<strong>medical</strong> 의료기관 5,623개<br/>
<strong>population</strong> 16자치구 인구
</div>

<div style="font-size:18px; color:#5B5B5B; margin-top:14px; line-height:1.55;">
컬럼: aptNm · gu_name · dealAmount · excluUseAr · deal_date
</div>
</div>

</div>

<div style="font-size:22px; margin-top:18px; text-align:center; color:#5B5B5B;">
합계 <strong>240만 행 · 23개 테이블</strong> — gu_name·code로 DB 간 JOIN 가능
</div>

---

<!-- _class: title-lock -->

# DB가 답해주는 다섯 가지 질문

<div style="display:grid; grid-template-columns:repeat(3, 1fr); gap:18px; margin-top:30px;">

<div style="background:#F5F5F5; border-radius:14px; padding:22px; min-height:170px;">
<div class="kpi" style="font-size:18px;">① 몇 개?</div>
<div style="font-size:24px; font-weight:800; margin-top:10px;">동래구 학원<br/>몇 개?</div>
</div>

<div style="background:#F5F5F5; border-radius:14px; padding:22px; min-height:170px;">
<div class="kpi" style="font-size:18px;">② 평균·최대</div>
<div style="font-size:24px; font-weight:800; margin-top:10px;">해운대구<br/>평균 매매가?</div>
</div>

<div style="background:#F5F5F5; border-radius:14px; padding:22px; min-height:170px;">
<div class="kpi" style="font-size:18px;">③ 순위</div>
<div style="font-size:24px; font-weight:800; margin-top:10px;">외국인 매수<br/>TOP 10은?</div>
</div>

</div>

<div style="display:grid; grid-template-columns:repeat(3, 1fr); gap:18px; margin-top:18px;">

<div style="background:#FFE5EB; border-radius:14px; padding:22px; min-height:170px;">
<div class="kpi" style="font-size:18px;">④ 시간 변화</div>
<div style="font-size:24px; font-weight:800; margin-top:10px;">우리 동네<br/>5년 시세는?</div>
</div>

<div style="background:#FFE5EB; border-radius:14px; padding:22px; min-height:170px;">
<div class="kpi" style="font-size:18px;">⑤ 집단 비교</div>
<div style="font-size:24px; font-weight:800; margin-top:10px;">자치구별<br/>평균은?</div>
</div>

<div style="padding:22px;"></div>

</div>

<div style="font-size:22px; margin-top:26px; text-align:center;">
이 다섯 가지를 알고 있으면, 어떤 질문이 DB로 풀리는지 판단됩니다.<br/>
<span style="color:#5B5B5B;">AI에게 무엇을 시킬지가 명확해집니다.</span>
</div>

---

<!-- _class: title-lock -->

# ERD 보는 법 <span style="font-size:0.44em; color:#5B5B5B;">표들이 어떻게 연결되는가</span>

<div style="display:grid; grid-template-columns:1.2fr 1fr; gap:30px; margin-top:24px;">

<div>

<div style="background:#F5F5F5; border-radius:14px; padding:22px; font-family:monospace; font-size:20px; line-height:1.6;">
<strong>stations</strong><br/>
─────────────<br/>
★ station_id<br/>
&nbsp;&nbsp;station_name<br/>
&nbsp;&nbsp;line_no<br/>
&nbsp;&nbsp;lat, lng
</div>

<div style="text-align:center; font-size:22px; margin:8px 0; color:#FC1C49; font-weight:800;">
1 ─────── N
</div>

<div style="background:#F5F5F5; border-radius:14px; padding:22px; font-family:monospace; font-size:20px; line-height:1.6;">
<strong>daily_passengers</strong><br/>
─────────────<br/>
&nbsp;&nbsp;id<br/>
&nbsp;&nbsp;date<br/>
&nbsp;&nbsp;hour<br/>
&nbsp;&nbsp;station_name (FK)<br/>
&nbsp;&nbsp;passengers
</div>

</div>

<div style="background:#FFE5EB; border-radius:14px; padding:24px;">
<div class="kpi" style="font-size:18px;">읽는 법</div>
<div style="font-size:20px; line-height:1.85; margin-top:12px;">
· <strong>박스</strong> — 표(테이블)<br/>
· <strong>★</strong> — 기본키, 한 줄을 식별<br/>
· <strong>선</strong> — 두 표의 연결<br/>
· <strong>1:N</strong> — 한 역에 여러 기록<br/>
· <strong>FK</strong> — 다른 표를 가리키는 컬럼
</div>
</div>

</div>

<div style="font-size:22px; margin-top:22px; text-align:center;">
ERD 작성은 AI에게 시킵니다. 사람은 그림에서 표끼리 어떻게 연결되는지 읽으면 됩니다.
</div>

---

<!-- _class: title-lock -->

# SQL은 외우는 게 아니다 <span style="font-size:0.44em; color:#5B5B5B;">AI에게 시키는 흐름</span>

<div style="margin-top:32px; font-size:26px; line-height:1.9;">

<div style="background:#F5F5F5; padding:24px; border-radius:12px; margin-bottom:14px;">
<strong>1. AI에게 DB 구조 파악시키기</strong><br/>
<code>03_AI활용/CLAUDE.md</code> 읽혀 → 표·컬럼 구조 인식
</div>

<div style="background:#F5F5F5; padding:24px; border-radius:12px; margin-bottom:14px;">
<strong>2. 자연어로 질문</strong><br/>
"해운대구 2025년 매매 TOP 10 단지 평균 거래금액 보여줘"
</div>

<div style="background:#F5F5F5; padding:24px; border-radius:12px; margin-bottom:14px;">
<strong>3. AI가 알아서 조회 → 결과 표</strong><br/>
쿼리는 AI가 작성. 수강생은 결과만 받음
</div>

<div style="background:#FFE5EB; padding:24px; border-radius:12px;">
<strong>4. "이 결과를 Plotly HTML 차트로" 요청</strong><br/>
산출물 즉시 생성, 카톡 공유 가능
</div>

</div>

---

<!-- _class: title-lock -->

# DB별 예시 9가지

<div style="display:grid; grid-template-columns:repeat(3, 1fr); gap:18px; margin-top:32px;">

<div style="background:#F5F5F5; border-radius:12px; padding:20px;">
<div class="kpi" style="font-size:16px;">도시철도 1</div>
<div style="font-size:22px; font-weight:800; margin-top:6px;">서면역 시간대별 패턴</div>
<div style="font-size:18px; color:#5B5B5B; margin-top:8px;">8시 925명 (6시의 3배)</div>
</div>

<div style="background:#F5F5F5; border-radius:12px; padding:20px;">
<div class="kpi" style="font-size:16px;">도시철도 2</div>
<div style="font-size:22px; font-weight:800; margin-top:6px;">노선별 누적 비교</div>
<div style="font-size:18px; color:#5B5B5B; margin-top:8px;">1호선 = 부산 대중교통 49%</div>
</div>

<div style="background:#F5F5F5; border-radius:12px; padding:20px;">
<div class="kpi" style="font-size:16px;">도시철도 3</div>
<div style="font-size:22px; font-weight:800; margin-top:6px;">서면역 출근 vs 퇴근</div>
<div style="font-size:18px; color:#5B5B5B; margin-top:8px;">퇴근 하차 18시 3,001명 (출근 승차의 3배)</div>
</div>

<div style="background:#FFE5EB; border-radius:12px; padding:20px;">
<div class="kpi" style="font-size:16px;">부동산 1</div>
<div style="font-size:22px; font-weight:800; margin-top:6px;">자치구 5년 시세</div>
<div style="font-size:18px; color:#5B5B5B; margin-top:8px;">부산 평균 3.1억 → 4.5억 (+45%)</div>
</div>

<div style="background:#FFE5EB; border-radius:12px; padding:20px;">
<div class="kpi" style="font-size:16px;">부동산 2</div>
<div style="font-size:22px; font-weight:800; margin-top:6px;">엘시티 5년 변동</div>
<div style="font-size:18px; color:#5B5B5B; margin-top:8px;">43.5억 → 31.2억 (-28%)</div>
</div>

<div style="background:#FFE5EB; border-radius:12px; padding:20px;">
<div class="kpi" style="font-size:16px;">부동산 3</div>
<div style="font-size:22px; font-weight:800; margin-top:6px;">학군 1위는?</div>
<div style="font-size:18px; color:#5B5B5B; margin-top:8px;">동래구 입시강좌 6,228개</div>
</div>

<div style="background:#F5F5F5; border-radius:12px; padding:20px;">
<div class="kpi" style="font-size:16px;">주식 1</div>
<div style="font-size:22px; font-weight:800; margin-top:6px;">5/6 폭등일 분석</div>
<div style="font-size:18px; color:#5B5B5B; margin-top:8px;">외국인 3조 → 삼성 +14.41%</div>
</div>

<div style="background:#F5F5F5; border-radius:12px; padding:20px;">
<div class="kpi" style="font-size:16px;">주식 2</div>
<div style="font-size:22px; font-weight:800; margin-top:6px;">외국인 보유율 추이</div>
<div style="font-size:18px; color:#5B5B5B; margin-top:8px;">삼성전자 5년 -7.4%p (55.7→48.3%)</div>
</div>

<div style="background:#F5F5F5; border-radius:12px; padding:20px;">
<div class="kpi" style="font-size:16px;">주식 3</div>
<div style="font-size:22px; font-weight:800; margin-top:6px;">코스피 vs 코스닥</div>
<div style="font-size:18px; color:#5B5B5B; margin-top:8px;">5년 +165% vs +13%</div>
</div>

</div>

---

<!-- _class: title-lock -->

# AI가 자주 범하는 오류

<div style="font-size:24px; margin-top:24px;">
같은 질문 — <strong>"동래구 입시강좌가 몇 개입니까?"</strong>
</div>

<div style="display:grid; grid-template-columns:1fr 1fr; gap:32px; margin-top:24px;">

<div style="background:#F5F5F5; border-radius:18px; padding:36px;">
<div class="kpi" style="font-size:20px;">표 하나로 셌을 때</div>
<div style="font-size:60px; font-weight:800; margin-top:18px; text-align:center;">6,228 <span style="font-size:30px;">개</span></div>
<div style="font-size:24px; color:#16A34A; font-weight:800; text-align:center; margin-top:18px;">✓ 맞는 답</div>
</div>

<div style="background:#FFE5EB; border-radius:18px; padding:36px;">
<div class="kpi" style="font-size:20px;">학교 표를 잘못 합쳤을 때</div>
<div style="font-size:60px; font-weight:800; margin-top:18px; text-align:center;">311,400 <span style="font-size:30px;">개</span></div>
<div style="font-size:24px; color:#FC1C49; font-weight:800; text-align:center; margin-top:18px;">✗ 50배 부풀어짐</div>
<div style="font-size:18px; color:#5B5B5B; text-align:center; margin-top:6px;">학교 50곳만큼 중복 합산</div>
</div>

</div>

<div style="font-size:22px; margin-top:28px; line-height:1.7;">
둘 다 <strong>오류 메시지는 없습니다.</strong> 둘 다 그럴듯한 숫자입니다.<br/>
비개발자 입장에서는 어느 쪽이 맞는지 구분되지 않습니다.
</div>

<div style="font-size:22px; color:#5B5B5B; margin-top:14px;">
→ 받은 답은 한 번 더 확인합니다 (다음 페이지)
</div>

<!--
Speaker note:
실측 검증 완료:
표 1개 = SELECT SUM(ipsi_count) FROM academies WHERE gu_name='동래구' → 6,228
학교 JOIN = + JOIN schools ON gu_name → 311,400 (50배)
강의장에서 라이브 실행 가능. 약 1초.
-->

---

<!-- _class: title-lock -->

# 받은 답을 다시 검토하게 시키는 법

<div style="margin-top:20px;">

<div class="kpi" style="font-size:20px;">한 문장으로 시키기 — 그대로 복붙</div>

<div style="background:#FFE5EB; border-radius:14px; padding:24px 28px; font-size:22px; line-height:1.75; margin-top:10px;">
"이 결과 다시 봐줘.<br/>
표 합치면서 숫자가 부풀려진 건 아닌지,<br/>
평균이 한쪽으로 쏠린 건 아닌지,<br/>
조건 이름이 데이터랑 정확히 맞는지,<br/>
단위가 맞는지 확인해서 알려줘."
</div>

</div>

<div style="margin-top:22px;">

<div class="kpi" style="font-size:20px;">검토 프롬프트가 잡아내는 것</div>

<div style="font-size:20px; line-height:1.85; margin-top:8px;">
· 표 두 개를 합칠 때 줄이 중복돼 합계가 부풀려진다<br/>
· 비싸거나 싼 몇 건에 평균이 끌려간다<br/>
· "해운대" vs "해운대구" 같은 표기 차이로 결과가 0건이 된다<br/>
· 만원·억원 단위가 섞여 답이 100배 어긋난다
</div>

</div>

<div style="font-size:22px; color:#5B5B5B; margin-top:20px;">
AI가 "맞습니다"만 반복하면 → "숫자로 다시 보여줘"를 덧붙입니다.
</div>

---

<!-- _class: title-lock -->

# 실습 1 <span style="font-size:0.44em; color:#5B5B5B;">부산 도시철도 · 90분</span>

<div style="margin-top:32px;">

<div style="background:#F5F5F5; padding:24px 32px; border-radius:14px; margin-bottom:18px;">
<div class="kpi" style="font-size:20px;">1️⃣ 질문하기</div>
<div style="font-size:26px; margin-top:6px;">"내가 매일 타는 역, 진짜 8시가 제일 붐비나?"</div>
</div>

<div style="background:#F5F5F5; padding:24px 32px; border-radius:14px; margin-bottom:18px;">
<div class="kpi" style="font-size:20px;">2️⃣ 기획하기</div>
<div style="font-size:26px; margin-top:6px;">4분할 대시보드 — 시간대별 라인 · 요일별 막대 · TOP10 · 노선별</div>
</div>

<div style="background:#FFE5EB; padding:24px 32px; border-radius:14px; margin-bottom:18px;">
<div class="kpi" style="font-size:20px;">3️⃣ 만들기 — 35분</div>
<div style="font-size:24px; margin-top:6px;">"내 역 시간대별 패턴 보여줘" → AI가 조회 → Folium HTML 지도 생성</div>
</div>

<div style="background:#F5F5F5; padding:24px 32px; border-radius:14px;">
<div class="kpi" style="font-size:20px;">4️⃣ 검토하기 / 5️⃣ 개선하기</div>
<div style="font-size:24px; margin-top:6px;">통계 의미 검증 + 날씨·인구 결합 아이디어</div>
</div>

</div>

<div style="font-size:22px; color:#5B5B5B; margin-top:20px; text-align:center;">
산출물: <strong>본인 역 중심 Folium HTML 1개</strong> (카톡 공유)
</div>

---

<!-- _class: title-lock -->

# 실습 2 <span style="font-size:0.44em; color:#5B5B5B;">부산 부동산 + 학군 · 60분</span>

<div style="font-size:20px; color:#5B5B5B; margin-top:16px;">실습 1에서 5단계를 익혔으니, 여기서는 질문·만들기·개선 세 단계에 집중합니다 (기획·검토는 같은 흐름).</div>

<div style="margin-top:20px;">

<div style="background:#F5F5F5; padding:24px 32px; border-radius:14px; margin-bottom:18px;">
<div class="kpi" style="font-size:20px;">1️⃣ 질문하기</div>
<div style="font-size:26px; margin-top:6px;">"내가 사는 자치구 아파트, 5년 동안 얼마나 변했어?"</div>
</div>

<div style="background:#FFE5EB; padding:24px 32px; border-radius:14px; margin-bottom:18px;">
<div class="kpi" style="font-size:20px;">3️⃣ 만들기 — 25분</div>
<div style="font-size:24px; margin-top:6px; line-height:1.6;">
"연도별 평균 거래금액 추이랑 단지 TOP 10 보여줘"<br/>
<span style="color:#5B5B5B; font-size:20px;">→ 쿼리는 AI가 작성, 결과만 차트로 받음</span>
</div>
</div>

<div style="background:#F5F5F5; padding:24px 32px; border-radius:14px;">
<div class="kpi" style="font-size:20px;">5️⃣ 개선하기 — 학군 검증</div>
<div style="font-size:24px; margin-top:6px;">단순 거리 X · <strong>입시강좌 수</strong>로 학군 강도 측정 · academies 테이블 활용</div>
</div>

</div>

<div style="font-size:22px; color:#5B5B5B; margin-top:20px; text-align:center;">
산출물: <strong>본인 동네 시세 Plotly HTML 1개</strong>
</div>

---

<!-- _class: title-lock -->

# 두 파트를 잇는 한 줄

<div style="display:flex; flex-direction:column; gap:32px; margin-top:48px;">

<div style="background:#F5F5F5; border-radius:18px; padding:36px 48px;">
<div class="kpi" style="font-size:22px;">PART 1에서 배운 것</div>
<div style="font-size:36px; font-weight:800; margin-top:10px;">AI 비서 가방을 가볍게 만든다</div>
<div style="font-size:22px; color:#5B5B5B; margin-top:8px;">CLAUDE.md · @파일 · /btw · /mcp 정리</div>
</div>

<div style="font-size:36px; text-align:center; color:#FC1C49; font-weight:800;">↓</div>

<div style="background:#FFE5EB; border-radius:18px; padding:36px 48px;">
<div class="kpi" style="font-size:22px;">PART 2에서 적용</div>
<div style="font-size:36px; font-weight:800; margin-top:10px;">가벼운 비서에게 일을 시킨다</div>
<div style="font-size:22px; color:#5B5B5B; margin-top:8px;">CLAUDE.md 첨부 · 자연어로 SQL 요청 · 차트까지 한 흐름</div>
</div>

</div>

---

<!-- _class: title-lock -->

# 1:1 코칭 안내 <span style="font-size:0.44em; color:#5B5B5B;">15:30 ~ 19:00 · 부서별 25분</span>

<div style="display:grid; grid-template-columns:1fr 1fr; gap:32px; margin-top:32px;">

<div style="background:#F5F5F5; border-radius:18px; padding:32px;">
<div class="kpi" style="font-size:20px;">준비해 오시면 좋은 것</div>
<div style="font-size:24px; line-height:1.9; margin-top:14px;">
1. 본인 부서 데이터 (엑셀 1개)<br/>
2. "데이터로 답하고 싶은 질문" 1개<br/>
3. 오전 만든 산출물 HTML
</div>
</div>

<div style="background:#FFE5EB; border-radius:18px; padding:32px;">
<div class="kpi" style="font-size:20px;">코칭에서 할 일</div>
<div style="font-size:24px; line-height:1.9; margin-top:14px;">
부서 엑셀 → SQLite 변환<br/>
질문 → AI에게 SQL 시키기<br/>
6회차 발표 자료 윤곽
</div>
</div>

</div>

---

<!-- _class: title-lock -->

# 오늘 가져갈 것

<div style="display:flex; flex-direction:column; gap:20px; margin-top:36px;">

<div style="display:flex; align-items:center; gap:24px;">
<div class="kpi" style="font-size:32px; min-width:60px;">①</div>
<div style="font-size:30px;">ccusage 설치 — 본인 1주일 토큰 추적</div>
</div>

<div style="display:flex; align-items:center; gap:24px;">
<div class="kpi" style="font-size:32px; min-width:60px;">②</div>
<div style="font-size:30px;">CLAUDE.md 다이어트 — 200줄 이하</div>
</div>

<div style="display:flex; align-items:center; gap:24px;">
<div class="kpi" style="font-size:32px; min-width:60px;">③</div>
<div style="font-size:30px;">본인 역 Folium HTML — 부산 도시철도 분석</div>
</div>

<div style="display:flex; align-items:center; gap:24px;">
<div class="kpi" style="font-size:32px; min-width:60px;">④</div>
<div style="font-size:30px;">본인 동네 Plotly HTML — 부동산 5년 추이</div>
</div>

<div style="display:flex; align-items:center; gap:24px;">
<div class="kpi" style="font-size:32px; min-width:60px;">⑤</div>
<div style="font-size:30px;">SQL은 외우는 게 아니라 AI에게 시키는 것</div>
</div>

</div>

---

<!-- _class: banner -->

<div style="font-size:36px; color:#5B5B5B; margin-bottom:32px;">5회차 끝</div>

# 다음 주<br/>6회차 발표

<div style="font-size:28px; color:#5B5B5B; margin-top:32px;">
부서별 프로젝트 + 사내 확산 계획
</div>

<!--
Speaker note:
오늘 만든 두 HTML이 6회차 발표 자료의 기초가 됩니다.
1:1 코칭에서 본인 부서 데이터까지 SQLite로 변환해두면, 다음 주에 풀 분석 보고서 만들 수 있습니다.
-->
