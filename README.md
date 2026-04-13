<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>학예당 광고 분석 대시보드 — 2026.04.13</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700&family=Noto+Serif+KR:wght@400;700&display=swap" rel="stylesheet">
<style>
:root {
  --bg: #0d0d10;
  --surface: #16161f;
  --surface2: #1e1e2a;
  --border: #26263a;
  --accent: #e8c96d;
  --accent2: #7b9cf0;
  --accent3: #6ddea8;
  --danger: #f07b7b;
  --text: #e8e8f0;
  --muted: #7a7a96;
  --dim: #48486a;
  --patjuk: #f07b7b;
  --hobak: #e8c96d;
  --sikhye: #7b9cf0;
  --gfa: #6ddea8;
}
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
  background: var(--bg);
  color: var(--text);
  font-family: 'Noto Sans KR', sans-serif;
  font-weight: 300;
  line-height: 1.6;
  min-height: 100vh;
}

/* HEADER */
.header {
  background: linear-gradient(135deg, #0f0f18 0%, #1a1a28 100%);
  border-bottom: 1px solid var(--border);
  padding: 40px 0 32px;
  position: relative;
  overflow: hidden;
}
.header::before {
  content: '';
  position: absolute;
  top: -80px; right: -60px;
  width: 300px; height: 300px;
  background: radial-gradient(circle, rgba(232,201,109,.06) 0%, transparent 70%);
  border-radius: 50%;
}
.header-inner { max-width: 1200px; margin: 0 auto; padding: 0 36px; }
.brand { font-size: 11px; letter-spacing: .2em; text-transform: uppercase; color: var(--accent); margin-bottom: 8px; }
.h-title { font-family: 'Noto Serif KR', serif; font-size: 26px; font-weight: 700; letter-spacing: -.02em; margin-bottom: 4px; }
.h-sub { font-size: 13px; color: var(--muted); }
.legend-row { display: flex; gap: 20px; margin-top: 16px; flex-wrap: wrap; }
.leg { display: flex; align-items: center; gap: 7px; font-size: 12px; color: var(--muted); }
.leg-dot { width: 10px; height: 10px; border-radius: 50%; }

/* GRID */
.container { max-width: 1200px; margin: 0 auto; padding: 36px 36px; }
.grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 20px; }
.grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 20px; margin-bottom: 20px; }
.grid-1 { margin-bottom: 20px; }

/* CARD */
.card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 24px;
  position: relative;
  overflow: hidden;
}
.card-title {
  font-size: 12px;
  letter-spacing: .1em;
  text-transform: uppercase;
  color: var(--muted);
  margin-bottom: 4px;
}
.card-sub {
  font-size: 12px;
  color: var(--dim);
  margin-bottom: 18px;
}
.chart-wrap {
  position: relative;
  width: 100%;
}

/* KPI STRIP */
.kpi-strip { display: grid; grid-template-columns: repeat(4, 1fr); gap: 14px; margin-bottom: 20px; }
.kpi {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 18px 16px;
  transition: border-color .2s;
}
.kpi:hover { border-color: rgba(232,201,109,.3); }
.kpi-label { font-size: 10px; letter-spacing: .1em; text-transform: uppercase; color: var(--muted); margin-bottom: 8px; }
.kpi-val { font-size: 22px; font-weight: 700; letter-spacing: -.02em; line-height: 1; margin-bottom: 4px; }
.kpi-sub { font-size: 11px; color: var(--dim); }
.kpi-t1 { border-top: 2px solid var(--patjuk); }
.kpi-t2 { border-top: 2px solid var(--hobak); }
.kpi-t3 { border-top: 2px solid var(--sikhye); }
.kpi-t4 { border-top: 2px solid var(--gfa); }

/* SECTION LABEL */
.section-label {
  font-size: 10px;
  letter-spacing: .18em;
  text-transform: uppercase;
  color: var(--dim);
  margin-bottom: 16px;
  display: flex;
  align-items: center;
  gap: 12px;
}
.section-label::after { content: ''; flex: 1; height: 1px; background: var(--border); }

/* ANNOTATION */
.anno {
  background: var(--surface2);
  border-left: 3px solid var(--accent);
  padding: 10px 14px;
  border-radius: 0 8px 8px 0;
  font-size: 12px;
  color: var(--muted);
  margin-top: 14px;
  line-height: 1.6;
}
.anno strong { color: var(--accent); font-weight: 500; }
.anno .g { color: var(--accent3); }
.anno .r { color: var(--danger); }

/* BADGE */
.badge { display: inline-block; padding: 2px 8px; border-radius: 8px; font-size: 10px; font-weight: 500; margin-left: 6px; }
.b-new { background: rgba(109,222,168,.12); color: var(--accent3); }
.b-warn { background: rgba(240,123,123,.12); color: var(--danger); }

/* FOOTER */
.footer { border-top: 1px solid var(--border); padding: 20px 36px; text-align: center; font-size: 11px; color: var(--dim); margin-top: 20px; }

/* ANIM */
@keyframes fadeUp { from { opacity:0; transform:translateY(16px); } to { opacity:1; transform:translateY(0); } }
.card { animation: fadeUp .5s ease both; }
.card:nth-child(2) { animation-delay: .06s; }
.card:nth-child(3) { animation-delay: .12s; }
</style>
</head>
<body>

<div class="header">
  <div class="header-inner">
    <div class="brand">학예당 · 광고 분석 대시보드</div>
    <div class="h-title">광고 성과 시각화 — 2026년 4월 12일</div>
    <div class="h-sub">분석 기간: 최근 7일 (4/6~4/12) · 최근 30일 (3/14~4/12)</div>
    <div class="legend-row">
      <span class="leg"><span class="leg-dot" style="background:var(--patjuk)"></span>팥죽</span>
      <span class="leg"><span class="leg-dot" style="background:var(--hobak)"></span>호박죽</span>
      <span class="leg"><span class="leg-dot" style="background:var(--sikhye)"></span>식혜</span>
      <span class="leg"><span class="leg-dot" style="background:var(--gfa)"></span>GFA / 네이버</span>
    </div>
  </div>
</div>

<div class="container">

  <!-- KPI -->
  <div class="section-label">이번 주 핵심 지표</div>
  <div class="kpi-strip">
    <div class="kpi kpi-t1">
      <div class="kpi-label">팥죽 CPC</div>
      <div class="kpi-val">150원</div>
      <div class="kpi-sub">지난주 143원 → +5%</div>
    </div>
    <div class="kpi kpi-t2">
      <div class="kpi-label">호박죽 CPC</div>
      <div class="kpi-val" style="color:var(--accent3)">102원</div>
      <div class="kpi-sub">지난주 101원 → 유지 ✅</div>
    </div>
    <div class="kpi kpi-t3">
      <div class="kpi-label">식혜 CPC</div>
      <div class="kpi-val" style="color:var(--danger)">115원</div>
      <div class="kpi-sub">지난주 84원 → +37% ⚠️</div>
    </div>
    <div class="kpi kpi-t4">
      <div class="kpi-label">GFA 성과형 결제</div>
      <div class="kpi-val">36건</div>
      <div class="kpi-sub">7일 매출 1,489,100원</div>
    </div>
  </div>

  <!-- 1. CPC 추이 -->
  <div class="section-label">캠페인별 CPC 추이</div>
  <div class="grid-1">
    <div class="card">
      <div class="card-title">캠페인별 CPC 비교 — 지난주 vs 이번주</div>
      <div class="card-sub">단위: 원 / 낮을수록 효율적</div>
      <div class="chart-wrap" style="height:260px">
        <canvas id="cpcChart"></canvas>
      </div>
      <div class="anno">
        <strong>호박죽</strong>은 101→102원으로 사실상 유지. <span class="g">가장 안정적.</span><br>
        <strong>식혜</strong>는 84→115원으로 +37% 상승 — 신규 소재 4종(학습 초기) 집행이 평균을 끌어올린 것. <span class="r">1주 후 재평가 필요.</span><br>
        <strong>팥죽</strong>은 143→150원으로 소폭 상승. 단일 소재 피로도 감지 시작 가능성.
      </div>
    </div>
  </div>

  <!-- 2. CTR 비교 -->
  <div class="section-label">소재별 CTR 비교</div>
  <div class="grid-2">
    <div class="card">
      <div class="card-title">캠페인별 CTR — 7일 기준</div>
      <div class="card-sub">단위: % / 높을수록 소재 반응성 좋음</div>
      <div class="chart-wrap" style="height:240px">
        <canvas id="ctrCampChart"></canvas>
      </div>
      <div class="anno">
        팥죽 CTR <strong>5.85%</strong>로 3개 캠페인 중 <span class="g">최고.</span><br>
        식혜 전체 CTR은 1.52%로 낮지만, 신규 소재 중 <strong>11_갓만들어</strong>는 4.54%로 주목.
      </div>
    </div>
    <div class="card">
      <div class="card-title">식혜 신규 소재 4종 CTR <span class="badge b-new">2~3일차</span></div>
      <div class="card-sub">단위: % / 4/11(금) 집행 시작 — 학습 초기</div>
      <div class="chart-wrap" style="height:240px">
        <canvas id="ctrNewChart"></canvas>
      </div>
      <div class="anno">
        <strong>11_갓만들어</strong> CTR <span class="g">4.54%</span>로 신규 4종 중 유일하게 의미 있는 반응.<br>
        나머지 3종은 학습 중 — 1주일 후 판단.
      </div>
    </div>
  </div>

  <!-- 3. 지출 분포 + 채널별 결제 -->
  <div class="section-label">지출 분포 & 채널별 결제금액</div>
  <div class="grid-2">
    <div class="card">
      <div class="card-title">Meta 캠페인별 지출 비중 — 7일</div>
      <div class="card-sub">총 지출 170,058원</div>
      <div class="chart-wrap" style="height:240px">
        <canvas id="spendChart"></canvas>
      </div>
      <div class="anno">
        세 캠페인이 비슷한 비중으로 분산 중. 성과 기준으로 <strong>호박죽·팥죽 비중 확대</strong>, 식혜는 신규 소재 평가 후 재편 예정.
      </div>
    </div>
    <div class="card">
      <div class="card-title">채널별 결제금액 — 7일 (네이버 애널리틱스)</div>
      <div class="card-sub">단위: 만원 / N+스토어앱 단체주문 추정 제외</div>
      <div class="chart-wrap" style="height:240px">
        <canvas id="channelChart"></canvas>
      </div>
      <div class="anno">
        GFA 성과형이 <strong>148.9만원</strong>으로 압도적 1위. 브랜드검색은 유입 100명에 6건으로 <span class="g">CVR 6%</span> — 소규모지만 질 높은 채널.
      </div>
    </div>
  </div>

  <!-- 4. 30일 누적 -->
  <div class="section-label">30일 누적 추이</div>
  <div class="grid-2">
    <div class="card">
      <div class="card-title">캠페인별 누적 지출 — 30일</div>
      <div class="card-sub">총 626,665원 / 3/14~4/12</div>
      <div class="chart-wrap" style="height:220px">
        <canvas id="spend30Chart"></canvas>
      </div>
    </div>
    <div class="card">
      <div class="card-title">캠페인별 누적 CTR — 30일 vs 7일</div>
      <div class="card-sub">30일 누적 vs 이번주 7일 비교</div>
      <div class="chart-wrap" style="height:220px">
        <canvas id="ctr30Chart"></canvas>
      </div>
      <div class="anno">
        팥죽 30일 CTR <strong>6.60%</strong>로 일관성 유지. 식혜 30일 1.51% — 신규 소재로 반등 기대 중.
      </div>
    </div>
  </div>

  <!-- 5. 호박죽 소재별 -->
  <div class="section-label">호박죽 소재별 상세</div>
  <div class="grid-2">
    <div class="card">
      <div class="card-title">호박죽 소재별 CPC — 7일</div>
      <div class="card-sub">단위: 원</div>
      <div class="chart-wrap" style="height:220px">
        <canvas id="hobakCpcChart"></canvas>
      </div>
      <div class="anno">
        <strong>숟가락</strong> 소재 CPC 92원으로 최저. 전체 클릭의 88% 견인. <span class="g">예산 집중 유지.</span><br>
        <span class="r">3대가이어온</span> CPC 228원으로 비효율 — 비활성화 검토.
      </div>
    </div>
    <div class="card">
      <div class="card-title">호박죽 소재별 CTR — 7일</div>
      <div class="card-sub">단위: %</div>
      <div class="chart-wrap" style="height:220px">
        <canvas id="hobakCtrChart"></canvas>
      </div>
    </div>
  </div>

</div>

<div class="footer">
  학예당 광고 분석 대시보드 · Meta Business Center + 네이버 애널리틱스 · 2026.04.12<br>
  식혜 신규 소재 4종: 4/11(금) 집행 시작, 2~3일차 초기 데이터
</div>

<script>
const C = {
  patjuk: '#f07b7b',
  hobak:  '#e8c96d',
  sikhye: '#7b9cf0',
  gfa:    '#6ddea8',
  dim:    'rgba(255,255,255,0.06)',
  grid:   'rgba(255,255,255,0.07)',
  text:   '#7a7a96',
};

const defOpts = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: { display: false },
    tooltip: {
      backgroundColor: '#16161f',
      borderColor: '#26263a',
      borderWidth: 1,
      titleColor: '#e8e8f0',
      bodyColor: '#7a7a96',
      padding: 10,
    }
  },
  scales: {
    x: { ticks: { color: C.text, font: { size: 11, family: 'Noto Sans KR' } }, grid: { color: C.grid } },
    y: { ticks: { color: C.text, font: { size: 11, family: 'Noto Sans KR' } }, grid: { color: C.grid } }
  }
};

// 1. CPC 추이 — 지난주 vs 이번주
new Chart(document.getElementById('cpcChart'), {
  type: 'bar',
  data: {
    labels: ['팥죽', '호박죽', '식혜'],
    datasets: [
      {
        label: '지난주 CPC',
        data: [143, 101, 84],
        backgroundColor: 'rgba(255,255,255,0.12)',
        borderColor: 'rgba(255,255,255,0.25)',
        borderWidth: 1,
        borderRadius: 6,
      },
      {
        label: '이번주 CPC',
        data: [150, 102, 115],
        backgroundColor: [
          'rgba(240,123,123,0.7)',
          'rgba(232,201,109,0.7)',
          'rgba(123,156,240,0.7)',
        ],
        borderColor: [C.patjuk, C.hobak, C.sikhye],
        borderWidth: 1.5,
        borderRadius: 6,
      }
    ]
  },
  options: {
    ...defOpts,
    plugins: {
      ...defOpts.plugins,
      legend: {
        display: true,
        labels: { color: C.text, font: { size: 11 }, boxWidth: 12, padding: 16 }
      }
    },
    scales: {
      ...defOpts.scales,
      y: { ...defOpts.scales.y, title: { display: true, text: '원 (CPC)', color: C.text, font: { size: 11 } } }
    }
  }
});

// 2. 캠페인 CTR 7일
new Chart(document.getElementById('ctrCampChart'), {
  type: 'bar',
  data: {
    labels: ['팥죽', '호박죽', '식혜(전체)', '식혜(소재3)'],
    datasets: [{
      data: [5.85, 3.66, 1.52, 1.45],
      backgroundColor: [
        'rgba(240,123,123,0.7)',
        'rgba(232,201,109,0.7)',
        'rgba(123,156,240,0.4)',
        'rgba(123,156,240,0.7)',
      ],
      borderColor: [C.patjuk, C.hobak, C.sikhye, C.sikhye],
      borderWidth: 1.5,
      borderRadius: 6,
    }]
  },
  options: {
    ...defOpts,
    scales: {
      ...defOpts.scales,
      y: { ...defOpts.scales.y, title: { display: true, text: 'CTR (%)', color: C.text, font: { size: 11 } } }
    }
  }
});

// 3. 식혜 신규 소재 CTR
new Chart(document.getElementById('ctrNewChart'), {
  type: 'bar',
  data: {
    labels: ['11_갓만들어', '9_아이스팩', '8_3대가 이어온', '12_가족과함께'],
    datasets: [{
      data: [4.54, 0.91, 0.39, 0.29],
      backgroundColor: [
        'rgba(109,222,168,0.75)',
        'rgba(123,156,240,0.45)',
        'rgba(123,156,240,0.45)',
        'rgba(123,156,240,0.45)',
      ],
      borderColor: [C.gfa, C.sikhye, C.sikhye, C.sikhye],
      borderWidth: 1.5,
      borderRadius: 6,
    }]
  },
  options: {
    ...defOpts,
    scales: {
      ...defOpts.scales,
      y: { ...defOpts.scales.y, title: { display: true, text: 'CTR (%)', color: C.text, font: { size: 11 } } }
    }
  }
});

// 4. Meta 지출 도넛
new Chart(document.getElementById('spendChart'), {
  type: 'doughnut',
  data: {
    labels: ['팥죽 58,691원', '호박죽 65,733원', '식혜 45,635원'],
    datasets: [{
      data: [58691, 65733, 45635],
      backgroundColor: [
        'rgba(240,123,123,0.8)',
        'rgba(232,201,109,0.8)',
        'rgba(123,156,240,0.8)',
      ],
      borderColor: ['#0d0d10'],
      borderWidth: 3,
      hoverOffset: 6,
    }]
  },
  options: {
    responsive: true,
    maintainAspectRatio: false,
    plugins: {
      legend: {
        display: true,
        position: 'bottom',
        labels: { color: C.text, font: { size: 11, family: 'Noto Sans KR' }, boxWidth: 12, padding: 12 }
      },
      tooltip: defOpts.plugins.tooltip,
    },
    cutout: '62%',
  }
});

// 5. 채널별 결제금액 (단체주문 제외)
new Chart(document.getElementById('channelChart'), {
  type: 'bar',
  data: {
    labels: ['GFA 성과형', 'ADVoost쇼핑', '가격비교-통합', '브랜드검색', '검색광고', '인스타그램'],
    datasets: [{
      data: [148.91, 50.65, 59.01, 17.53, 19.65, 7.72],
      backgroundColor: [
        'rgba(109,222,168,0.8)',
        'rgba(109,222,168,0.45)',
        'rgba(109,222,168,0.45)',
        'rgba(232,201,109,0.7)',
        'rgba(232,201,109,0.45)',
        'rgba(123,156,240,0.45)',
      ],
      borderColor: [C.gfa, C.gfa, C.gfa, C.hobak, C.hobak, C.sikhye],
      borderWidth: 1.5,
      borderRadius: 6,
    }]
  },
  options: {
    ...defOpts,
    indexAxis: 'y',
    scales: {
      x: { ...defOpts.scales.x, title: { display: true, text: '만원', color: C.text, font: { size: 11 } } },
      y: { ticks: { color: C.text, font: { size: 10, family: 'Noto Sans KR' } }, grid: { color: C.grid } }
    }
  }
});

// 6. 30일 지출
new Chart(document.getElementById('spend30Chart'), {
  type: 'bar',
  data: {
    labels: ['팥죽', '호박죽', '식혜'],
    datasets: [{
      label: '30일 누적',
      data: [306195, 211281, 109189],
      backgroundColor: [
        'rgba(240,123,123,0.7)',
        'rgba(232,201,109,0.7)',
        'rgba(123,156,240,0.7)',
      ],
      borderColor: [C.patjuk, C.hobak, C.sikhye],
      borderWidth: 1.5,
      borderRadius: 6,
    }]
  },
  options: {
    ...defOpts,
    scales: {
      ...defOpts.scales,
      y: { ...defOpts.scales.y, title: { display: true, text: '원', color: C.text, font: { size: 11 } } }
    }
  }
});

// 7. 30일 vs 7일 CTR 비교
new Chart(document.getElementById('ctr30Chart'), {
  type: 'bar',
  data: {
    labels: ['팥죽', '호박죽', '식혜'],
    datasets: [
      {
        label: '30일 CTR',
        data: [6.60, 3.32, 1.51],
        backgroundColor: 'rgba(255,255,255,0.12)',
        borderColor: 'rgba(255,255,255,0.25)',
        borderWidth: 1,
        borderRadius: 6,
      },
      {
        label: '7일 CTR',
        data: [5.85, 3.66, 1.52],
        backgroundColor: [
          'rgba(240,123,123,0.7)',
          'rgba(232,201,109,0.7)',
          'rgba(123,156,240,0.7)',
        ],
        borderColor: [C.patjuk, C.hobak, C.sikhye],
        borderWidth: 1.5,
        borderRadius: 6,
      }
    ]
  },
  options: {
    ...defOpts,
    plugins: {
      ...defOpts.plugins,
      legend: {
        display: true,
        labels: { color: C.text, font: { size: 11 }, boxWidth: 12, padding: 16 }
      }
    },
    scales: {
      ...defOpts.scales,
      y: { ...defOpts.scales.y, title: { display: true, text: 'CTR (%)', color: C.text, font: { size: 11 } } }
    }
  }
});

// 8. 호박죽 소재 CPC
new Chart(document.getElementById('hobakCpcChart'), {
  type: 'bar',
  data: {
    labels: ['숟가락이\n쓰러지지 않는', '이게바로', '호박물에', '3대가이어온'],
    datasets: [{
      data: [92, 89, 155, 228],
      backgroundColor: [
        'rgba(232,201,109,0.85)',
        'rgba(232,201,109,0.6)',
        'rgba(232,201,109,0.35)',
        'rgba(240,123,123,0.6)',
      ],
      borderColor: [C.hobak, C.hobak, C.hobak, C.patjuk],
      borderWidth: 1.5,
      borderRadius: 6,
    }]
  },
  options: {
    ...defOpts,
    scales: {
      ...defOpts.scales,
      y: { ...defOpts.scales.y, title: { display: true, text: '원 (CPC)', color: C.text, font: { size: 11 } } }
    }
  }
});

// 9. 호박죽 소재 CTR
new Chart(document.getElementById('hobakCtrChart'), {
  type: 'bar',
  data: {
    labels: ['숟가락이\n쓰러지지 않는', '이게바로', '호박물에', '3대가이어온'],
    datasets: [{
      data: [5.38, 5.44, 3.09, 0.71],
      backgroundColor: [
        'rgba(232,201,109,0.85)',
        'rgba(109,222,168,0.7)',
        'rgba(232,201,109,0.4)',
        'rgba(240,123,123,0.5)',
      ],
      borderColor: [C.hobak, C.gfa, C.hobak, C.patjuk],
      borderWidth: 1.5,
      borderRadius: 6,
    }]
  },
  options: {
    ...defOpts,
    scales: {
      ...defOpts.scales,
      y: { ...defOpts.scales.y, title: { display: true, text: 'CTR (%)', color: C.text, font: { size: 11 } } }
    }
  }
});
</script>
</body>
</html>
