<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>학예당 광고 분석 대시보드 — 2026.04.13</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700;900&family=DM+Serif+Display:ital@0;1&display=swap" rel="stylesheet">
<style>
:root {
  --ink: #111118;
  --paper: #f4f1eb;
  --paper2: #ede9e0;
  --paper3: #e4dfd3;
  --border: #d4cfc3;
  --accent-gold: #c9971a;
  --accent-rust: #c45c3a;
  --accent-sage: #4a7c59;
  --accent-slate: #3d5a73;
  --accent-muted: #8a7d6b;
  --hobak: #c9971a;
  --patjuk: #c45c3a;
  --sikhye: #3d5a73;
  --gfa: #4a7c59;
  --coupang: #c45c3a;
  --warn: #d4572a;
  --ok: #4a7c59;
}

* { box-sizing: border-box; margin: 0; padding: 0; }

body {
  background: var(--paper);
  color: var(--ink);
  font-family: 'Noto Sans KR', sans-serif;
  font-weight: 300;
  line-height: 1.6;
  min-height: 100vh;
}

/* HEADER */
.header {
  background: var(--ink);
  color: var(--paper);
  padding: 48px 48px 40px;
  position: relative;
  overflow: hidden;
}
.header::before {
  content: '학예당';
  position: absolute;
  right: -20px; bottom: -40px;
  font-family: 'DM Serif Display', serif;
  font-size: 180px;
  color: rgba(255,255,255,0.04);
  white-space: nowrap;
  pointer-events: none;
}
.header-tag {
  display: inline-block;
  border: 1px solid rgba(255,255,255,0.25);
  padding: 3px 10px;
  border-radius: 2px;
  font-size: 10px;
  letter-spacing: .16em;
  text-transform: uppercase;
  color: rgba(255,255,255,0.5);
  margin-bottom: 16px;
}
.header h1 {
  font-family: 'DM Serif Display', serif;
  font-size: 36px;
  font-weight: 400;
  letter-spacing: -.01em;
  margin-bottom: 6px;
}
.header-meta {
  font-size: 12px;
  color: rgba(255,255,255,0.45);
  margin-bottom: 28px;
}

.channel-chips { display: flex; gap: 8px; flex-wrap: wrap; }
.chip {
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 11px;
  font-weight: 500;
  border: 1px solid;
}
.chip-meta { background: rgba(201,151,26,.15); color: #e8c96d; border-color: rgba(201,151,26,.3); }
.chip-gfa  { background: rgba(74,124,89,.15); color: #7dc49a; border-color: rgba(74,124,89,.3); }
.chip-cp   { background: rgba(196,92,58,.15); color: #e8956d; border-color: rgba(196,92,58,.3); }

/* LAYOUT */
.body { max-width: 1280px; margin: 0 auto; padding: 40px 40px 60px; }

.section-head {
  display: flex; align-items: center; gap: 14px;
  margin-bottom: 20px; margin-top: 44px;
}
.section-head:first-child { margin-top: 0; }
.section-num {
  width: 32px; height: 32px; border-radius: 50%;
  background: var(--ink); color: var(--paper);
  display: flex; align-items: center; justify-content: center;
  font-size: 12px; font-weight: 700; flex-shrink: 0;
}
.section-title { font-family: 'DM Serif Display', serif; font-size: 20px; font-weight: 400; }
.section-sub { font-size: 11px; color: var(--accent-muted); margin-left: auto; }

/* GRID */
.g2 { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin-bottom: 16px; }
.g3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 16px; margin-bottom: 16px; }
.g4 { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; margin-bottom: 16px; }
.g1 { margin-bottom: 16px; }
.span2 { grid-column: span 2; }

/* CARD */
.card {
  background: white;
  border: 1px solid var(--border);
  border-radius: 6px;
  padding: 22px 22px 18px;
  position: relative;
}
.card-label {
  font-size: 10px;
  letter-spacing: .12em;
  text-transform: uppercase;
  color: var(--accent-muted);
  margin-bottom: 14px;
  display: flex; align-items: center; gap: 8px;
}
.card-label::after {
  content: ''; flex: 1; height: 1px; background: var(--border);
}
.chart-wrap { position: relative; width: 100%; }

/* KPI */
.kpi-grid { display: grid; grid-template-columns: repeat(4,1fr); gap: 12px; margin-bottom: 16px; }
.kpi {
  background: white;
  border: 1px solid var(--border);
  border-top: 3px solid;
  border-radius: 6px;
  padding: 16px 16px 14px;
}
.kpi.k-hobak { border-top-color: var(--hobak); }
.kpi.k-patjuk { border-top-color: var(--patjuk); }
.kpi.k-sikhye { border-top-color: var(--sikhye); }
.kpi.k-gfa    { border-top-color: var(--gfa); }
.kpi-label { font-size: 10px; letter-spacing: .1em; text-transform: uppercase; color: var(--accent-muted); margin-bottom: 6px; }
.kpi-val { font-size: 24px; font-weight: 700; letter-spacing: -.02em; line-height: 1; margin-bottom: 3px; }
.kpi-sub { font-size: 11px; color: var(--accent-muted); }

/* ANNO */
.anno {
  background: var(--paper2);
  border-left: 3px solid var(--accent-gold);
  padding: 10px 14px;
  font-size: 12px;
  color: #555;
  margin-top: 12px;
  border-radius: 0 4px 4px 0;
  line-height: 1.7;
}
.anno strong { color: var(--ink); font-weight: 600; }
.anno .warn { color: var(--warn); font-weight: 500; }
.anno .ok { color: var(--ok); font-weight: 500; }
.anno .note { color: var(--accent-muted); font-style: italic; }

/* KEYWORD TABLE */
.kw-table { width: 100%; border-collapse: collapse; font-size: 12px; }
.kw-table th {
  background: var(--paper2);
  border: 1px solid var(--border);
  padding: 8px 12px;
  font-size: 10px;
  font-weight: 600;
  letter-spacing: .08em;
  text-transform: uppercase;
  color: var(--accent-muted);
  text-align: left;
}
.kw-table td {
  border: 1px solid var(--border);
  padding: 8px 12px;
  color: var(--ink);
  vertical-align: middle;
}
.kw-table tr:hover td { background: var(--paper); }
.tag {
  display: inline-block; padding: 2px 8px; border-radius: 3px;
  font-size: 10px; font-weight: 600; white-space: nowrap;
}
.tag-a { background: rgba(74,124,89,.12); color: #2d6b40; }
.tag-b { background: rgba(201,151,26,.12); color: #8a6400; }
.tag-c { background: rgba(196,92,58,.12); color: #9b3a1a; }
.tag-ok { background: rgba(74,124,89,.12); color: #2d6b40; }
.tag-warn { background: rgba(196,92,58,.12); color: #9b3a1a; }
.tag-watch { background: rgba(201,151,26,.12); color: #8a6400; }

/* MATRIX */
.matrix { width: 100%; border-collapse: collapse; font-size: 12px; }
.matrix th, .matrix td {
  border: 1px solid var(--border);
  padding: 10px 14px;
  text-align: center;
}
.matrix th { background: var(--ink); color: var(--paper); font-size: 11px; font-weight: 500; letter-spacing: .06em; }
.matrix td:first-child { background: var(--paper2); font-weight: 600; font-size: 13px; text-align: left; }
.cell-strong { background: rgba(74,124,89,.1); color: #2d6b40; font-weight: 600; }
.cell-mid    { background: rgba(201,151,26,.1); color: #7a5800; }
.cell-weak   { background: rgba(196,92,58,.1); color: #8a2800; }
.cell-watch  { background: rgba(61,90,115,.1); color: #1e3a52; }

/* FOOTER */
.footer {
  border-top: 1px solid var(--border);
  padding: 20px 40px;
  font-size: 11px;
  color: var(--accent-muted);
  text-align: center;
}

/* ANIM */
@keyframes up { from { opacity:0; transform:translateY(20px); } to { opacity:1; transform:translateY(0); } }
.card, .kpi { animation: up .4s ease both; }
</style>
</head>
<body>

<div class="header">
  <div class="header-tag">Performance Report</div>
  <h1>학예당 광고 분석 대시보드</h1>
  <div class="header-meta">기준일: 2026.04.13 &nbsp;·&nbsp; Meta 7일(4/7~4/13) · GFA 주차별 · 쿠팡 30일(3/1~3/31) · 키워드(3/15~4/13)</div>
  <div class="channel-chips">
    <span class="chip chip-meta">Meta · 유입 채널</span>
    <span class="chip chip-gfa">네이버 GFA · 전환 채널</span>
    <span class="chip chip-cp">쿠팡 · 구매 채널</span>
  </div>
</div>

<div class="body">

  <!-- KPI -->
  <div class="kpi-grid">
    <div class="kpi k-hobak">
      <div class="kpi-label">호박죽 Meta CPC</div>
      <div class="kpi-val" style="color:var(--hobak)">104원</div>
      <div class="kpi-sub">클릭 657회 · 7일</div>
    </div>
    <div class="kpi k-patjuk">
      <div class="kpi-label">팥죽 Meta CTR</div>
      <div class="kpi-val" style="color:var(--patjuk)">5.79%</div>
      <div class="kpi-sub">클릭 400회 · 7일</div>
    </div>
    <div class="kpi k-gfa">
      <div class="kpi-label">GFA 메인배너 ROAS</div>
      <div class="kpi-val" style="color:var(--gfa)">203%</div>
      <div class="kpi-sub">3종 합산 · 지난주</div>
    </div>
    <div class="kpi k-sikhye">
      <div class="kpi-label">쿠팡 파워링크 ROAS</div>
      <div class="kpi-val" style="color:var(--sikhye)">723%</div>
      <div class="kpi-sub">P01.식혜 · 30일</div>
    </div>
  </div>

  <!-- ① META -->
  <div class="section-head">
    <div class="section-num">1</div>
    <div class="section-title">Meta 광고 성과</div>
    <div class="section-sub">4/7~4/13 · 유입 효율 중심 해석</div>
  </div>

  <div class="g2">
    <div class="card">
      <div class="card-label">캠페인별 CPC — 7일</div>
      <div class="chart-wrap" style="height:220px"><canvas id="metaCpc"></canvas></div>
      <div class="anno">호박죽 <strong>104원</strong>으로 가장 낮음. 팥죽 152원이지만 CTR <span class="ok">5.79%</span>로 반응성 최고.</div>
    </div>
    <div class="card">
      <div class="card-label">캠페인별 CTR — 7일</div>
      <div class="chart-wrap" style="height:220px"><canvas id="metaCtr"></canvas></div>
      <div class="anno">팥죽 CTR <strong>5.79%</strong> · 호박죽 3.69% · 식혜 전체 0.82% (신규 소재 학습 포함).</div>
    </div>
  </div>

  <div class="g3">
    <div class="card">
      <div class="card-label">호박죽 소재별 CTR</div>
      <div class="chart-wrap" style="height:200px"><canvas id="hobakCtr"></canvas></div>
      <div class="anno"><strong>숟가락·이게바로</strong> 5.37~5.44%로 동급. <span class="warn">3대가이어온 0.71%</span> 정리 후보.</div>
    </div>
    <div class="card">
      <div class="card-label">호박죽 소재별 CPC</div>
      <div class="chart-wrap" style="height:200px"><canvas id="hobakCpc"></canvas></div>
      <div class="anno">숟가락 <strong>94원</strong>·이게바로 <strong>97원</strong>으로 최저. <span class="warn">3대가이어온 228원</span> 비활성화 권장.</div>
    </div>
    <div class="card">
      <div class="card-label">식혜 신규 소재 5종 CTR <small style="color:var(--warn);font-size:10px">집행 5~7일차</small></div>
      <div class="chart-wrap" style="height:200px"><canvas id="sikhyeNew"></canvas></div>
      <div class="anno"><span class="note">학습 초기 — 최종 판단은 10~14일 후.</span> 갓만들어 <strong>1.46%</strong>가 상대적 우위.</div>
    </div>
  </div>

  <!-- ② GFA -->
  <div class="section-head">
    <div class="section-num">2</div>
    <div class="section-title">네이버 GFA 성과</div>
    <div class="section-sub">지난주(4/6~4/12) 확정 수치 · 이번주 초반은 2일치</div>
  </div>

  <div class="g2">
    <div class="card">
      <div class="card-label">메인 배너 3종 ROAS — 지난주(4/6~4/12)</div>
      <div class="chart-wrap" style="height:230px"><canvas id="gfaMainRoas"></canvas></div>
      <div class="anno">
        <strong>식혜_배너</strong> 179.5% · <strong>단팥죽_배너</strong> 202.1% · <strong>호박죽_배너</strong> 210.7%<br>
        메인 배너 3종 합산: 광고비 882,854원 / 구매 45건 / ROAS <span class="ok">203.2%</span>
      </div>
    </div>
    <div class="card">
      <div class="card-label">메인 배너 3종 — 광고비 vs 전환매출 (지난주)</div>
      <div class="chart-wrap" style="height:230px"><canvas id="gfaMainBar"></canvas></div>
      <div class="anno">광고비 대비 전환매출이 모두 플러스 구간. 단팥죽 809,760원 · 호박죽 799,810원 · 식혜 184,050원.</div>
    </div>
  </div>

  <div class="g2">
    <div class="card">
      <div class="card-label">주차별 ROAS 비교 — 지난주 vs 이번주 초반</div>
      <div class="chart-wrap" style="height:220px"><canvas id="gfaWeekly"></canvas></div>
      <div class="anno">
        <span class="warn">호박죽 107.5%</span>로 급락 — 단, 이번주 2일치 데이터. 단정 금지, 관찰 필요.<br>
        <span class="ok">단팥죽 444.7%</span> 급등 — 역시 2일치, 방향성 참고만.
      </div>
    </div>
    <div class="card">
      <div class="card-label">ADVoost vs 메인 배너 ROAS 비교 (지난주) <small style="color:var(--warn);font-size:10px">ADVoost: 보조 채널</small></div>
      <div class="chart-wrap" style="height:220px"><canvas id="gfaAdvRoas"></canvas></div>
      <div class="anno">
        ADVoost ROAS 수치가 높게 보이는 것은 자동화 채널 특성 때문. <span class="note">메인 배너와 동일 기준 비교 주의.</span><br>
        <span class="warn">카탈로그 86% → 0%</span> 지속 — 즉시 축소 필요.
      </div>
    </div>
  </div>

  <!-- ③ COUPANG -->
  <div class="section-head">
    <div class="section-num">3</div>
    <div class="section-title">쿠팡 키워드 전환 인사이트</div>
    <div class="section-sub">파워링크+브랜드검색(3/15~4/13) · AI 스마트(3/1~3/31)</div>
  </div>

  <div class="g2">
    <div class="card">
      <div class="card-label">쿠팡 AI 스마트 캠페인별 ROAS (3/1~3/31)</div>
      <div class="chart-wrap" style="height:230px"><canvas id="cpCampRoas"></canvas></div>
      <div class="anno">AI 스마트_로켓 <span class="ok">520%</span> · 호박죽 412% · 식혜_매최 311% · 단팥죽 251% · <span class="warn">죽-매출최적화 111%</span> 정리 검토.</div>
    </div>
    <div class="card">
      <div class="card-label">쿠팡 AI 스마트 주차별 ROAS 추이 (3/1~3/31)</div>
      <div class="chart-wrap" style="height:230px"><canvas id="cpWeekly"></canvas></div>
      <div class="anno">3/8~3/14 피크 <strong>602%</strong> → 3/22 이후 458%로 하락. 계절성 또는 수요 변동 가능성.</div>
    </div>
  </div>

  <div class="g1">
    <div class="card">
      <div class="card-label">쿠팡 파워링크 · 브랜드검색 — 캠페인별 30일 성과</div>
      <div class="chart-wrap" style="height:200px"><canvas id="cpKwCamp"></canvas></div>
      <div class="anno">
        <strong>P01.식혜 파워링크</strong> 광고비 342,309원 / 전환매출 2,475,980원 / ROAS <span class="ok">723%</span><br>
        B01.브랜드검색: 광고비 0원 / 구매전환 53건 / 구매매출 2,016,650원 → 광고 성과가 아닌 <strong>브랜드 수요 지표</strong>로 해석
      </div>
    </div>
  </div>

  <!-- KEYWORD TABLE -->
  <div class="g1">
    <div class="card">
      <div class="card-label">쿠팡 키워드 전환 분류 (P01.식혜 파워링크 · 3/15~4/13)</div>
      <table class="kw-table">
        <thead>
          <tr>
            <th>분류</th>
            <th>패턴/날짜</th>
            <th>비용</th>
            <th>클릭</th>
            <th>전환수</th>
            <th>전환매출</th>
            <th>구매매출</th>
            <th>판단</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td><span class="tag tag-a">A. 매출 만드는</span></td>
            <td>4/7(화) — 전환 발생</td>
            <td>9,480원</td>
            <td>19회</td>
            <td>1건</td>
            <td>31,350원</td>
            <td>31,350원</td>
            <td><span class="tag tag-ok">유지</span></td>
          </tr>
          <tr>
            <td><span class="tag tag-a">A. 매출 만드는</span></td>
            <td>4/10(금) — 전환 집중</td>
            <td>11,905원</td>
            <td>15회</td>
            <td>3건</td>
            <td>141,750원</td>
            <td>15,750원</td>
            <td><span class="tag tag-ok">유지</span></td>
          </tr>
          <tr>
            <td><span class="tag tag-a">A. 매출 만드는</span></td>
            <td>4/11(토) — 전환 발생</td>
            <td>9,672원</td>
            <td>17회</td>
            <td>2건</td>
            <td>47,120원</td>
            <td>26,120원</td>
            <td><span class="tag tag-ok">유지</span></td>
          </tr>
          <tr>
            <td><span class="tag tag-b">B. 클릭 있는데 약함</span></td>
            <td>4/8(수) — 전환 0건</td>
            <td>12,133원</td>
            <td>15회</td>
            <td>0건</td>
            <td>0원</td>
            <td>0원</td>
            <td><span class="tag tag-warn">상세페이지 보완</span></td>
          </tr>
          <tr>
            <td><span class="tag tag-b">B. 클릭 있는데 약함</span></td>
            <td>4/9(목) — 전환 0건</td>
            <td>8,278원</td>
            <td>16회</td>
            <td>0건</td>
            <td>0원</td>
            <td>0원</td>
            <td><span class="tag tag-warn">상세페이지 보완</span></td>
          </tr>
          <tr>
            <td><span class="tag tag-c">C. 비효율</span></td>
            <td>4/12(일) — 전환 0건</td>
            <td>11,381원</td>
            <td>23회</td>
            <td>0건</td>
            <td>0원</td>
            <td>0원</td>
            <td><span class="tag tag-warn">키워드 제외 검토</span></td>
          </tr>
          <tr>
            <td><span class="tag tag-c">C. 비효율</span></td>
            <td>4/13(월) — 전환 0건</td>
            <td>13,148원</td>
            <td>21회</td>
            <td>0건</td>
            <td>0원</td>
            <td>0원</td>
            <td><span class="tag tag-warn">키워드 제외 검토</span></td>
          </tr>
        </tbody>
      </table>
      <div class="anno">
        30일 중 <strong>전환 발생 14일 / 없음 16일.</strong> 클릭→구매 전환율 3.5%(543클릭 중 19건).<br>
        <span class="note">상세페이지 보완 포인트:</span> "100% 국내산·무첨가·숙성 기준"을 상단에 배치 → 탐색 의도 유입자의 구매 전환 유도.
      </div>
    </div>
  </div>

  <!-- ④ MATRIX -->
  <div class="section-head">
    <div class="section-num">4</div>
    <div class="section-title">상품별 채널 역할 매트릭스</div>
    <div class="section-sub">수치 기준 실질 회수 채널 정리</div>
  </div>

  <div class="g1">
    <div class="card">
      <div class="card-label">상품 × 채널 역할 정리</div>
      <table class="matrix">
        <thead>
          <tr>
            <th style="text-align:left">상품</th>
            <th>Meta (유입)</th>
            <th>GFA (직접전환)</th>
            <th>쿠팡 (구매전환)</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>🎃 호박죽</td>
            <td class="cell-strong">CPC 104원 / 클릭 657회<br><small>클릭 효율 최강</small></td>
            <td class="cell-mid">ROAS 211% (지난주)<br><small>이번주 107% — 관찰 중</small></td>
            <td class="cell-strong">AI 스마트 ROAS 412%<br><small>구매 15건</small></td>
          </tr>
          <tr>
            <td>🍲 팥죽/단팥죽</td>
            <td class="cell-strong">CTR 5.79% / 클릭 400회<br><small>반응성 최고</small></td>
            <td class="cell-strong">ROAS 202% (지난주)<br><small>구매 17건</small></td>
            <td class="cell-mid">ROAS 251%<br><small>구매 11건</small></td>
          </tr>
          <tr>
            <td>🍵 식혜</td>
            <td class="cell-watch">소재 재편 중<br><small>신규 5종 학습 5~7일차</small></td>
            <td class="cell-weak">ROAS 180% (지난주)<br><small>상대적으로 약함</small></td>
            <td class="cell-strong">파워링크 ROAS 723%<br><small>브랜드 수요 확인</small></td>
          </tr>
        </tbody>
      </table>
      <div class="anno">
        <span class="ok">회수 확인 채널:</span> GFA 메인 배너 3종 합산 ROAS 203% / 쿠팡 P01.식혜 ROAS 723% / 쿠팡 AI 스마트 ROAS 520%<br>
        <span class="warn">정리 대상:</span> GFA 카탈로그(ROAS 86%→0%) · 쿠팡 죽-매출최적화(ROAS 111%)
      </div>
    </div>
  </div>

  <!-- ⑤ ACTION -->
  <div class="section-head">
    <div class="section-num">5</div>
    <div class="section-title">예산 판단 요약</div>
    <div class="section-sub">수치 기준 즉시 / 관찰 / 정리</div>
  </div>

  <div class="g3">
    <div class="card">
      <div class="card-label" style="border-left:3px solid var(--warn);padding-left:8px">🔴 즉시 액션</div>
      <table class="kw-table">
        <tbody>
          <tr><td><strong>GFA 카탈로그</strong></td><td><span class="tag tag-c">ROAS 86%→0%</span></td></tr>
          <tr><td><strong>쿠팡 죽-매출최적화</strong></td><td><span class="tag tag-c">ROAS 111%, 전환 3건</span></td></tr>
          <tr><td><strong>Meta 3대가이어온</strong></td><td><span class="tag tag-c">CTR 0.71%, CPC 228원</span></td></tr>
          <tr><td><strong>팥죽 소재 바리에이션</strong></td><td><span class="tag tag-warn">단일 소재 피로도 대비</span></td></tr>
        </tbody>
      </table>
    </div>
    <div class="card">
      <div class="card-label" style="border-left:3px solid var(--hobak);padding-left:8px">🟡 관찰 / 단기</div>
      <table class="kw-table">
        <tbody>
          <tr><td><strong>GFA 호박죽_배너</strong></td><td><span class="tag tag-b">이번주 전체 수치 확인</span></td></tr>
          <tr><td><strong>식혜 신규 소재</strong></td><td><span class="tag tag-b">10~14일차 판단</span></td></tr>
          <tr><td><strong>ADVoost(3039)</strong></td><td><span class="tag tag-b">방향성 참고 — 표본 소</span></td></tr>
          <tr><td><strong>이게바로 소재</strong></td><td><span class="tag tag-b">독립 세트 분리 테스트</span></td></tr>
        </tbody>
      </table>
    </div>
    <div class="card">
      <div class="card-label" style="border-left:3px solid var(--gfa);padding-left:8px">🟢 유지 / 중기</div>
      <table class="kw-table">
        <tbody>
          <tr><td><strong>GFA 단팥죽_배너</strong></td><td><span class="tag tag-ok">ROAS 202% 유지</span></td></tr>
          <tr><td><strong>GFA 호박죽_배너</strong></td><td><span class="tag tag-ok">추이 확인 후 유지</span></td></tr>
          <tr><td><strong>쿠팡 P01.식혜</strong></td><td><span class="tag tag-ok">ROAS 723% 유지</span></td></tr>
          <tr><td><strong>키워드 비전환 제외</strong></td><td><span class="tag tag-ok">상세페이지 보완 병행</span></td></tr>
        </tbody>
      </table>
    </div>
  </div>

</div>

<div class="footer">
  학예당 광고 분석 대시보드 v3 &nbsp;·&nbsp; 2026.04.13 &nbsp;·&nbsp; Meta($1=1,370원) · GFA result_5 · 쿠팡 export · 쿠팡 키워드 보고서<br>
  GFA 이번주 초반 수치(4/13~)는 2일치 — 단정 금지. ADVoost ROAS는 자동화 채널 특성 보정 필요.
</div>

<script>
const C = {
  hobak:'#c9971a', patjuk:'#c45c3a', sikhye:'#3d5a73',
  gfa:'#4a7c59', ink:'#111118', muted:'#8a7d6b',
  border:'rgba(0,0,0,0.08)',
};
const tooltip = {
  backgroundColor:'#111118', borderColor:'rgba(255,255,255,0.1)',
  borderWidth:1, titleColor:'#f4f1eb', bodyColor:'#a09585', padding:10,
};
const base = {
  responsive:true, maintainAspectRatio:false,
  plugins:{ legend:{display:false}, tooltip },
  scales:{
    x:{ ticks:{color:C.muted,font:{size:10,family:'Noto Sans KR'}}, grid:{color:'rgba(0,0,0,0.05)'} },
    y:{ ticks:{color:C.muted,font:{size:10,family:'Noto Sans KR'}}, grid:{color:'rgba(0,0,0,0.05)'} }
  }
};

// 1. Meta CPC
new Chart(document.getElementById('metaCpc'),{
  type:'bar',
  data:{
    labels:['호박죽','팥죽','식혜(기존소재3)','식혜(신규5종)'],
    datasets:[{
      data:[104,152,108,215],
      backgroundColor:['rgba(201,151,26,0.8)','rgba(196,92,58,0.8)','rgba(61,90,115,0.6)','rgba(61,90,115,0.35)'],
      borderColor:[C.hobak,C.patjuk,C.sikhye,C.sikhye],
      borderWidth:1.5, borderRadius:4,
    }]
  },
  options:{...base,scales:{...base.scales,y:{...base.scales.y,title:{display:true,text:'원(CPC)',color:C.muted,font:{size:10}}}}}
});

// 2. Meta CTR
new Chart(document.getElementById('metaCtr'),{
  type:'bar',
  data:{
    labels:['팥죽','호박죽','식혜(기존)','식혜(신규)'],
    datasets:[{
      data:[5.79,3.69,1.09,0.49],
      backgroundColor:['rgba(196,92,58,0.8)','rgba(201,151,26,0.8)','rgba(61,90,115,0.6)','rgba(61,90,115,0.3)'],
      borderColor:[C.patjuk,C.hobak,C.sikhye,C.sikhye],
      borderWidth:1.5, borderRadius:4,
    }]
  },
  options:{...base,scales:{...base.scales,y:{...base.scales.y,title:{display:true,text:'CTR(%)',color:C.muted,font:{size:10}}}}}
});

// 3. 호박죽 소재 CTR
new Chart(document.getElementById('hobakCtr'),{
  type:'bar',
  data:{
    labels:['숟가락이\n쓰러지지않는','이게바로','호박물에','3대가이어온'],
    datasets:[{
      data:[5.37,5.44,3.23,0.71],
      backgroundColor:['rgba(201,151,26,0.85)','rgba(74,124,89,0.75)','rgba(201,151,26,0.45)','rgba(196,92,58,0.6)'],
      borderColor:[C.hobak,C.gfa,C.hobak,C.patjuk],
      borderWidth:1.5, borderRadius:4,
    }]
  },
  options:{...base,scales:{...base.scales,y:{...base.scales.y,max:7,title:{display:true,text:'CTR(%)',color:C.muted,font:{size:10}}}}}
});

// 4. 호박죽 소재 CPC
new Chart(document.getElementById('hobakCpc'),{
  type:'bar',
  data:{
    labels:['숟가락이\n쓰러지지않는','이게바로','호박물에','3대가이어온'],
    datasets:[{
      data:[94,97,149,228],
      backgroundColor:['rgba(74,124,89,0.8)','rgba(74,124,89,0.65)','rgba(201,151,26,0.5)','rgba(196,92,58,0.7)'],
      borderColor:[C.gfa,C.gfa,C.hobak,C.patjuk],
      borderWidth:1.5, borderRadius:4,
    }]
  },
  options:{...base,scales:{...base.scales,y:{...base.scales.y,title:{display:true,text:'원(CPC)',color:C.muted,font:{size:10}}}}}
});

// 5. 식혜 신규 소재
new Chart(document.getElementById('sikhyeNew'),{
  type:'bar',
  data:{
    labels:['10_야구장','11_갓만들어','9_아이스팩','8_3대가','12_가족'],
    datasets:[{
      data:[2.30,1.46,0.91,0.35,0.26],
      backgroundColor:['rgba(61,90,115,0.8)','rgba(61,90,115,0.65)','rgba(61,90,115,0.45)','rgba(196,92,58,0.4)','rgba(196,92,58,0.4)'],
      borderColor:[C.sikhye,C.sikhye,C.sikhye,C.patjuk,C.patjuk],
      borderWidth:1.5, borderRadius:4,
    }]
  },
  options:{...base,scales:{...base.scales,y:{...base.scales.y,title:{display:true,text:'CTR(%)',color:C.muted,font:{size:10}}}}}
});

// 6. GFA 메인 배너 ROAS
new Chart(document.getElementById('gfaMainRoas'),{
  type:'bar',
  data:{
    labels:['식혜_배너','단팥죽_배너','호박죽_배너'],
    datasets:[{
      label:'지난주 ROAS',
      data:[179.5,202.1,210.7],
      backgroundColor:['rgba(61,90,115,0.75)','rgba(196,92,58,0.75)','rgba(201,151,26,0.75)'],
      borderColor:[C.sikhye,C.patjuk,C.hobak],
      borderWidth:1.5, borderRadius:4,
    }]
  },
  options:{...base,scales:{...base.scales,y:{...base.scales.y,title:{display:true,text:'ROAS(%)',color:C.muted,font:{size:10}}}}}
});

// 7. GFA 메인 배너 광고비 vs 전환매출
new Chart(document.getElementById('gfaMainBar'),{
  type:'bar',
  data:{
    labels:['식혜_배너','단팥죽_배너','호박죽_배너'],
    datasets:[
      {label:'광고비',data:[102534,400660,379660],backgroundColor:'rgba(0,0,0,0.1)',borderColor:'rgba(0,0,0,0.2)',borderWidth:1,borderRadius:4},
      {label:'전환매출',data:[184050,809760,799810],
        backgroundColor:['rgba(61,90,115,0.7)','rgba(196,92,58,0.7)','rgba(201,151,26,0.7)'],
        borderColor:[C.sikhye,C.patjuk,C.hobak],borderWidth:1.5,borderRadius:4}
    ]
  },
  options:{...base,
    plugins:{...base.plugins,legend:{display:true,labels:{color:C.muted,font:{size:10},boxWidth:10,padding:12}}},
    scales:{...base.scales,y:{...base.scales.y,title:{display:true,text:'원',color:C.muted,font:{size:10}}}}
  }
});

// 8. GFA 주차별 ROAS 비교 (지난주 vs 이번주)
new Chart(document.getElementById('gfaWeekly'),{
  type:'bar',
  data:{
    labels:['식혜','단팥죽','호박죽'],
    datasets:[
      {label:'지난주(4/6~12)',data:[179.5,202.1,210.7],backgroundColor:'rgba(0,0,0,0.12)',borderColor:'rgba(0,0,0,0.2)',borderWidth:1,borderRadius:4},
      {label:'이번주초반(4/13~, 2일치)',
        data:[207.6,444.7,107.5],
        backgroundColor:['rgba(61,90,115,0.7)','rgba(196,92,58,0.7)','rgba(201,151,26,0.7)'],
        borderColor:[C.sikhye,C.patjuk,C.hobak],borderWidth:1.5,borderRadius:4}
    ]
  },
  options:{...base,
    plugins:{...base.plugins,legend:{display:true,labels:{color:C.muted,font:{size:10},boxWidth:10,padding:12}}},
    scales:{...base.scales,y:{...base.scales.y,title:{display:true,text:'ROAS(%)',color:C.muted,font:{size:10}}}}
  }
});

// 9. GFA ADVoost vs 메인배너 ROAS
new Chart(document.getElementById('gfaAdvRoas'),{
  type:'bar',
  data:{
    labels:['식혜_배너','단팥죽_배너','호박죽_배너','ADVoost\n40~99','ADVoost\n3039','카탈로그'],
    datasets:[{
      data:[179.5,202.1,210.7,436.5,469.2,86.0],
      backgroundColor:[
        'rgba(61,90,115,0.7)','rgba(196,92,58,0.7)','rgba(201,151,26,0.7)',
        'rgba(74,124,89,0.45)','rgba(74,124,89,0.45)','rgba(196,92,58,0.35)',
      ],
      borderColor:[C.sikhye,C.patjuk,C.hobak,C.gfa,C.gfa,C.patjuk],
      borderWidth:1.5, borderRadius:4,
    }]
  },
  options:{...base,scales:{...base.scales,y:{...base.scales.y,title:{display:true,text:'ROAS(%)',color:C.muted,font:{size:10}}}}}
});

// 10. 쿠팡 AI 스마트 캠페인 ROAS
new Chart(document.getElementById('cpCampRoas'),{
  type:'bar',
  data:{
    labels:['AI스마트_로켓','식혜_매최','호박죽','단팥죽','식혜수동_1','죽-매출최적화'],
    datasets:[{
      data:[520.41,310.86,412.43,251.02,168.94,110.7],
      backgroundColor:[
        'rgba(74,124,89,0.85)','rgba(61,90,115,0.7)','rgba(201,151,26,0.75)',
        'rgba(196,92,58,0.7)','rgba(61,90,115,0.45)','rgba(196,92,58,0.35)',
      ],
      borderColor:[C.gfa,C.sikhye,C.hobak,C.patjuk,C.sikhye,C.patjuk],
      borderWidth:1.5, borderRadius:4,
    }]
  },
  options:{...base,scales:{...base.scales,y:{...base.scales.y,title:{display:true,text:'ROAS(%)',color:C.muted,font:{size:10}}}}}
});

// 11. 쿠팡 주차별 ROAS
new Chart(document.getElementById('cpWeekly'),{
  type:'line',
  data:{
    labels:['3/1~3/7','3/8~3/14','3/15~3/21','3/22~3/28','3/29~3/31'],
    datasets:[{
      data:[499.26,602.09,562.23,458.08,426.26],
      borderColor:C.patjuk, backgroundColor:'rgba(196,92,58,0.08)',
      borderWidth:2.5, pointRadius:5, pointBackgroundColor:'white',
      pointBorderColor:C.patjuk, pointBorderWidth:2,
      tension:0.3, fill:true,
    }]
  },
  options:{...base,scales:{...base.scales,y:{...base.scales.y,min:300,title:{display:true,text:'ROAS(%)',color:C.muted,font:{size:10}}}}}
});

// 12. 쿠팡 파워링크+브랜드검색 성과
new Chart(document.getElementById('cpKwCamp'),{
  type:'bar',
  data:{
    labels:['P01.식혜\n파워링크','P99.벌크\n파워링크','B01.브랜드검색\n(브랜드 수요 지표)'],
    datasets:[
      {label:'광고비',data:[342309,4345,0],backgroundColor:'rgba(0,0,0,0.1)',borderColor:'rgba(0,0,0,0.2)',borderWidth:1,borderRadius:4},
      {label:'전환매출',data:[2475980,156360,4330650],
        backgroundColor:['rgba(61,90,115,0.75)','rgba(74,124,89,0.6)','rgba(201,151,26,0.55)'],
        borderColor:[C.sikhye,C.gfa,C.hobak],borderWidth:1.5,borderRadius:4}
    ]
  },
  options:{...base,
    plugins:{...base.plugins,legend:{display:true,labels:{color:C.muted,font:{size:10},boxWidth:10,padding:12}}},
    scales:{...base.scales,y:{...base.scales.y,title:{display:true,text:'원',color:C.muted,font:{size:10}}}}
  }
});
</script>
</body>
</html>
