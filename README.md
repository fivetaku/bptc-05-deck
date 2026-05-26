# 5회차 강의 덱 — 토큰 절약 + 데이터베이스

> BPTC AX 5회차 · 2026-05-27 (수)
> 토큰 절약 1시간 + 데이터베이스 실습 3시간 + 1:1 코칭 3.5시간

## 파일

```
deck.md                  Marp 마크다운 (수정용)
bptc-light-theme.css     테마 (참고)
README.md                이 파일
```

## 빌드 방법

### HTML 슬라이드
```bash
npx @marp-team/marp-cli@latest deck.md \
  --theme-set bptc-light-theme.css \
  --html
```

### PDF
```bash
npx @marp-team/marp-cli@latest deck.md \
  --theme-set bptc-light-theme.css \
  --pdf --allow-local-files
```

### 단일 발표용 (서버 + 화면)
```bash
npx @marp-team/marp-cli@latest deck.md \
  --theme-set bptc-light-theme.css \
  --server
# → http://localhost:8080
```

## 슬라이드 구성 (총 17장)

| # | 슬라이드 | 비고 |
|---|---------|------|
| 1 | 표지 | banner |
| 2 | 오늘 한 줄 (Part 1 + Part 2) | |
| 3 | 시간표 | |
| 4 | PART 1 표지 | banner |
| 5 | 강사 자료 제작 비용 공개 (ccusage 시연) | 킬러 데모 |
| 6 | 캐시가 전부다 (가격표) | |
| 7 | 한글은 두 번 비싸진다 | |
| 8 | 6가지 팁 그리드 | |
| 9 | 실습 1 — CLAUDE.md + /context | |
| 10 | 실습 2 — /btw + @file + /mcp | |
| 11 | 한 줄 메시지 5개 | |
| 12 | PART 2 표지 | banner |
| 13 | 패키지 한눈에 (DB 3종) | |
| 14 | SQL은 외우는 게 아니다 (AI 흐름) | |
| 15 | DB별 예시 9가지 | |
| 16 | 실습 1 — 부산 도시철도 5단계 | |
| 17 | 실습 2 — 부산 부동산 + 학군 5단계 | |
| 18 | 두 파트를 잇는 한 줄 | 핵심 |
| 19 | 1:1 코칭 안내 | |
| 20 | 오늘 가져갈 것 5개 | |
| 21 | 다음 주 6회차 안내 | banner |

## 수정 시 주의

- `<!--  Speaker note: -->` 블록은 강사 노트, 발표 시 별도 노트 모드로 봄
- `bptc-light-theme.css`는 fastcampus 세미나 덱과 공유 — 이 파일은 수정하지 말 것
- 폰트는 시스템 한글 폰트 사용 (별도 설치 불필요)
