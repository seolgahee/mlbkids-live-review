# MLB KIDS 라이브방송 성과보고 생성 스킬

MLB KIDS 라이브방송 월간 성과보고 HTML 슬라이드를 새로 생성하거나 업데이트합니다.
기존 템플릿(`MLB_KIDS_라이브방송_성과보고_2026_04_최종.html`)의 디자인 시스템을 그대로 유지하면서 데이터만 교체합니다.

## 사용법

```
/live-report 2026년 5월 보고서 생성해줘
/live-report 공식몰 3회, 네이버 2회, 총 실매출 12억
```

---

## 보고서 규격

### 기술 스택
- 순수 HTML + CSS + Vanilla JS (프레임워크 없음)
- Chart.js 4.4.0 (CDN)
- Google Fonts: Pretendard
- 외부 의존성: Chart.js 1개만

### 슬라이드 구조 (10슬라이드 기준)

| # | ID | 제목 | 주요 콘텐츠 |
|---|-----|------|------------|
| 01 | `s-cover` | 표지 | 로고 필, 대제목, stat-pill × 4, 날짜/팀 |
| 02 | `s-toc` | 목차 | toc-item 클릭 이동 링크 |
| 03 | `s-official` | 공식몰 성과 | KPI 그리드, ROI/이익 차트, 테이블, 인사이트 |
| 04 | `s-naver` | 네이버 성과 | KPI 그리드, ROI 복합 차트, 테이블, 인사이트 |
| 05 | `s-yoy` | YoY 비교 | 검색량 트렌드 차트, YoY 바 차트, 채널별 ROAS |
| 06 | `s-april` | 당월 효율 분석 | 광고 ROAS 차트, 광고비 vs 실매출 차트, 요약 카드 |
| 07 | `s-creative` | 소재 & 숏클립 | CTR 비교 차트, 영상 클립 카드, 제품 태그 |
| 08 | `s-crm` | CRM & 후속매출 | 깔때기 차트, 회원 통계, 히든쿠폰 계획 |
| 09 | `s-trend` | 월별 실적 추이 | 월별 바 차트, KPI 요약, YoY 인사이트 |
| 10 | `s-strategy` | 향후 전략 | 액션 그리드, 전략 카드, 운영 원칙 |

---

## CSS 디자인 시스템

### CSS 변수 (라이트 모드 기준)

```css
:root {
  --text-primary: #1A1C1F;  --text-strong: #2B2E33;
  --text-active:  #4D5157;  --text-muted:  #888D96;
  --text-disabled:#D1D8E2;
  --border:       #E4E7EB;  --border-strong:#C8CED6;
  --bg:           #FFFFFF;  --bg-subtle:   #F7F8FA;  --bg-chip: #F1F3F6;
  --brand:        #0F1116;
  --accent:       #3D5AFE;  --accent-soft: #EEF1FF;
  --success:      #12B76A;  --danger:      #F04438;  --warning: #F79009;
}
```

### 차트 팔레트

```javascript
const C = {
  accent:  '#3D5AFE',   // 파랑 (메인)
  success: '#12B76A',   // 초록 (긍정)
  danger:  '#F04438',   // 빨강 (부정)
  warning: '#F79009',   // 주황 (주의)
  muted:   '#C8CED6',   // 회색 (비교 기준)
  darkText:'#4D5157'    // 차트 레이블 텍스트
};
```

### 주요 컴포넌트 클래스

| 클래스 | 용도 |
|--------|------|
| `.kpi-card` | KPI 지표 카드 (accent-bar + label + val + sub) |
| `.kpi-grid.cols4/5/6` | KPI 카드 그리드 (4/5/6열) |
| `.stat-pill` | 표지용 대형 지표 (sp-val + sp-label) |
| `.insight-box.pos/neg/info/warn` | 컬러 인사이트 박스 (왼쪽 border bar) |
| `.toc-item` | 목차 클릭 항목 |
| `.clip-card` | 숏클립 영상 카드 |
| `.flow-step` | 프로세스 단계 박스 |
| `.strat-card` | 전략 카드 (sc-title + ul) |
| `.action-item` | 번호형 액션 플랜 항목 |
| `.product-tag` | 방송 제품 태그 (pt-date + pt-theme + pt-product) |
| `.two-col` | 2열 레이아웃 |
| `.slide-inner` | 슬라이드 내부 최대너비 1120px 래퍼 |

### 레이아웃 패턴

```html
<!-- 표준 슬라이드 구조 -->
<section class="slide [bg-subtle]" id="s-xxx" data-label="탭 레이블">
  <div class="slide-inner reveal">
    <div style="display:grid;gap:8px;justify-items:center;text-align:center">
      <span class="eyebrow"><span class="dot"></span>섹션명</span>
      <h2 class="headline">제목</h2>
    </div>
    <!-- 콘텐츠 -->
  </div>
  <div class="slide-brand">MLB KIDS · 라이브방송 성과보고</div>
  <div class="slide-pg">NN / 10</div>
</section>
```

---

## 작업 절차

보고서 생성을 요청받으면 다음 순서로 진행합니다.

1. **데이터 수집** — 월/연도, 방송 일정·채널·회차, 핵심 지표(실매출, ROAS, ROI, 광고비, 직접이익), YoY 비교값, 인사이트 텍스트
2. **파일 복사** — `MLB_KIDS_라이브방송_성과보고_2026_04_최종.html`을 새 파일명으로 복사 (`YYYY_MM` 변경)
3. **데이터 교체** — 슬라이드별로 숫자·날짜·차트 배열·테이블 데이터·인사이트 문구 교체
4. **디자인 유지** — CSS, JavaScript 구조, 컴포넌트 클래스는 절대 변경하지 않음
5. **파일명 규칙** — `MLB_KIDS_라이브방송_성과보고_YYYY_MM_최종.html`

---

## 주의사항

- 인라인 스타일(`style="..."`)의 배경색은 다크모드에서 `!important`로 덮어쓰여지므로 슬라이드 개별 bg는 수정 불필요
- 차트 ID는 슬라이드 ID 패턴을 따름 (예: `chartNaverRoi`, `chartAprilRoas`)
- `data-label` 속성은 사이드 네비게이션 tooltip에 사용됨
- `reveal` 클래스는 IntersectionObserver로 자동 관리됨 (수동 조작 불필요)
- 다크모드 상태는 `localStorage('reportMode')`에 유지됨
