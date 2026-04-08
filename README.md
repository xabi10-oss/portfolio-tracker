[index.html](https://github.com/user-attachments/files/26572779/index.html)
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Portfolio Benchmarks</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=DM+Mono:wght@300;400;500&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
<style>
  :root {
    --bg:      #1a1f2e;
    --bg2:     #222840;
    --bg3:     #2a3150;
    --bg4:     #313a5a;
    --border:  rgba(255,255,255,0.13);
    --border2: rgba(255,255,255,0.22);
    --text:    #eef0f8;
    --text2:   #a8aec8;
    --text3:   #6a728f;
    --green:   #2edb96;
    --green-dim: rgba(46,219,150,0.15);
    --red:     #ff5e6d;
    --red-dim: rgba(255,94,109,0.15);
    --accent:  #6b9fff;
    --gold:    #ffd166;
    --radius:  14px;
    --radius-sm: 9px;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    min-height: 100vh;
    padding: 28px 24px 48px;
    max-width: 1200px;
    margin: 0 auto;
  }

  /* ── header ── */
  .header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 28px;
    flex-wrap: wrap;
    gap: 14px;
  }
  .header-left h1 { font-size: 21px; font-weight: 600; letter-spacing: -0.4px; color: var(--text); }
  .header-left p  { font-size: 12px; color: var(--text3); margin-top: 4px; }
  .header-right   { display: flex; align-items: center; gap: 10px; }

  .refresh-btn {
    background: var(--bg3);
    border: 1px solid var(--border2);
    color: var(--text2);
    font-family: 'DM Sans', sans-serif;
    font-size: 13px;
    padding: 8px 16px;
    border-radius: var(--radius-sm);
    cursor: pointer;
    display: flex;
    align-items: center;
    gap: 6px;
    transition: all 0.2s;
  }
  .refresh-btn:hover { border-color: var(--accent); color: var(--accent); }
  #refreshIcon.spinning { animation: spin 1s linear infinite; }
  @keyframes spin { to { transform: rotate(360deg); } }

  .countdown {
    font-family: 'DM Mono', monospace;
    font-size: 12px;
    color: var(--text3);
    background: var(--bg3);
    border: 1px solid var(--border);
    padding: 8px 14px;
    border-radius: var(--radius-sm);
  }
  .countdown span { color: var(--accent); }

  /* ── summary ── */
  .summary {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
    gap: 10px;
    margin-bottom: 26px;
  }
  .sum-card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 14px 16px;
    position: relative;
    overflow: hidden;
  }
  .sum-card::before { content:''; position:absolute; top:0;left:0;right:0; height:3px; background: var(--border); }
  .sum-card.pos::before { background: var(--green); }
  .sum-card.neg::before { background: var(--red); }
  .sum-lbl { font-size: 10px; color: var(--text3); text-transform: uppercase; letter-spacing: 0.07em; margin-bottom: 6px; }
  .sum-val { font-family: 'DM Mono', monospace; font-size: 19px; font-weight: 500; color: var(--text3); }
  .sum-val.pos { color: var(--green); }
  .sum-val.neg { color: var(--red); }
  .sum-sub { font-size: 10px; color: var(--text3); margin-top: 3px; font-family: 'DM Mono', monospace; }

  /* ── section label ── */
  .sec-lbl { font-size: 10px; text-transform: uppercase; letter-spacing: 0.08em; color: var(--text3); margin-bottom: 12px; }

  /* ── cards ── */
  .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(290px, 1fr)); gap: 12px; margin-bottom: 28px; }

  .card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 18px 20px 16px;
    transition: border-color 0.2s;
  }
  .card:hover { border-color: var(--border2); }

  .card-top { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 14px; }
  .card-name { font-size: 13px; font-weight: 500; color: var(--text); line-height: 1.4; }
  .card-isin { font-family: 'DM Mono', monospace; font-size: 10px; color: var(--text3); margin-top: 3px; }

  .badge { font-size: 10px; font-weight: 500; padding: 3px 9px; border-radius: 99px; white-space: nowrap; border: 1px solid; letter-spacing: 0.03em; }
  .b-blue   { color:#6aaaf7; border-color:rgba(106,170,247,0.3); background:rgba(106,170,247,0.12); }
  .b-teal   { color:#42d4b4; border-color:rgba(66,212,180,0.3);  background:rgba(66,212,180,0.12); }
  .b-amber  { color:#ffd166; border-color:rgba(255,209,102,0.3); background:rgba(255,209,102,0.12); }
  .b-purple { color:#c4a8ff; border-color:rgba(196,168,255,0.3); background:rgba(196,168,255,0.12); }
  .b-pink   { color:#ff8fc8; border-color:rgba(255,143,200,0.3); background:rgba(255,143,200,0.12); }

  .divider { border:none; border-top:1px solid var(--border); margin:12px 0; }

  .card-rows { display:flex; flex-direction:column; gap:7px; }
  .card-row  { display:flex; justify-content:space-between; align-items:center; }
  .row-lbl   { font-size:12px; color:var(--text3); }
  .row-val   { font-size:13px; color:var(--text2); font-family:'DM Mono',monospace; }
  .row-price { font-size:15px; font-weight:500; color:var(--text); font-family:'DM Mono',monospace; }

  .chg { font-size:14px; font-weight:500; font-family:'DM Mono',monospace; padding:2px 9px; border-radius:5px; }
  .chg.pos { color:var(--green); background:var(--green-dim); }
  .chg.neg { color:var(--red);   background:var(--red-dim); }
  .chg.neu { color:var(--text3); background:var(--bg3); font-size:12px; }

  /* ── sparkline under card ── */
  .sparkline-wrap { margin-top:14px; height:52px; position:relative; }

  /* ── bar chart section ── */
  .chart-section {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 20px 24px;
    margin-bottom: 28px;
  }
  .chart-title { font-size:13px; color:var(--text2); margin-bottom:18px; }
  .chart-wrap  { position:relative; height:200px; }

  /* ── footer ── */
  .footer { font-size:11px; color:var(--text3); line-height:1.8; border-top:1px solid var(--border); padding-top:16px; }

  /* ── toast ── */
  .toast {
    position:fixed; bottom:24px; right:24px;
    background:var(--bg3); border:1px solid var(--red); color:var(--red);
    font-size:12px; padding:10px 16px; border-radius:var(--radius-sm);
    display:none; z-index:99;
  }
</style>
</head>
<body>

<div class="header">
  <div class="header-left">
    <h1>Portfolio Benchmarks</h1>
    <p>Fondos Vanguard · Invesco Gold · Bitcoin — auto-refresco cada 60 s</p>
  </div>
  <div class="header-right">
    <div class="countdown">próxima actualización: <span id="countdown">60</span>s</div>
    <button class="refresh-btn" onclick="loadAll()">
      <svg id="refreshIcon" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <polyline points="23 4 23 10 17 10"/><polyline points="1 20 1 14 7 14"/>
        <path d="M3.51 9a9 9 0 0 1 14.85-3.36L23 10M1 14l4.64 4.36A9 9 0 0 0 20.49 15"/>
      </svg>
      Actualizar
    </button>
  </div>
</div>

<!-- summary strip -->
<div class="summary">
  <div class="sum-card" id="sc-sp500">  <div class="sum-lbl">S&amp;P 500</div>   <div class="sum-val" id="sv-sp500">—</div>  <div class="sum-sub">SPY</div></div>
  <div class="sum-card" id="sc-europe"> <div class="sum-lbl">MSCI Europe</div> <div class="sum-val" id="sv-europe">—</div> <div class="sum-sub">VGK</div></div>
  <div class="sum-card" id="sc-em">     <div class="sum-lbl">MSCI EM</div>     <div class="sum-val" id="sv-em">—</div>     <div class="sum-sub">EEM</div></div>
  <div class="sum-card" id="sc-japan">  <div class="sum-lbl">MSCI Japan</div>  <div class="sum-val" id="sv-japan">—</div>  <div class="sum-sub">EWJ</div></div>
  <div class="sum-card" id="sc-gold">   <div class="sum-lbl">Oro</div>         <div class="sum-val" id="sv-gold">—</div>   <div class="sum-sub">GLD</div></div>
  <div class="sum-card" id="sc-btc">    <div class="sum-lbl">Bitcoin</div>     <div class="sum-val" id="sv-btc">—</div>    <div class="sum-sub">BTC/USDT</div></div>
</div>

<div class="sec-lbl">Detalle por posición</div>
<div class="grid" id="grid"></div>

<!-- daily bar chart -->
<div class="chart-section">
  <div class="chart-title">Variación diaria por benchmark (%)</div>
  <div class="chart-wrap"><canvas id="barChart"></canvas></div>
</div>

<div class="footer">
  Datos: <strong>Finnhub.io</strong> · Tickers: SPY, VGK, EEM, IWM, EWJ, GLD, BINANCE:BTCUSDT ·
  Los fondos Vanguard se valoran a cierre (D+1); estos índices son orientación intradiaria.
</div>

<div class="toast" id="toast"></div>

<script>
const KEY = 'd7b6k8pr01ql9e4lgr2gd7b6k8pr01ql9e4lgr30';
const BASE = 'https://finnhub.io/api/v1';

const POSITIONS = [
  { id:'sp500',   name:'Vanguard U.S. 500',        isin:'IE00BFPM9V94', short:'RV USA',     bench:'S&P 500',       badge:'b-blue',   ticker:'SPY',              sumId:'sp500',  type:'stock' },
  { id:'europe',  name:'Vanguard European Stock',   isin:'IE00BFPM9L96', short:'RV Europa',  bench:'MSCI Europe',   badge:'b-teal',   ticker:'VGK',              sumId:'europe', type:'stock' },
  { id:'em',      name:'Vanguard Emerging Markets', isin:'IE00BFPM9J74', short:'Emergentes', bench:'MSCI EM',       badge:'b-amber',  ticker:'EEM',              sumId:'em',     type:'stock' },
  { id:'small',   name:'Vanguard Global Small-Cap', isin:'IE00BFRTDD83', short:'Small Caps', bench:'MSCI World SC', badge:'b-purple', ticker:'IWM',              sumId:null,     type:'stock' },
  { id:'japan',   name:'Vanguard Japan Stock',      isin:'IE00BFPM9P35', short:'RV Japón',   bench:'MSCI Japan',    badge:'b-pink',   ticker:'EWJ',              sumId:'japan',  type:'stock' },
  { id:'gold',    name:'Invesco Physical Gold ETC', isin:'IE00B579F325', short:'Oro',        bench:'Gold (GLD)',     badge:'b-amber',  ticker:'GLD',              sumId:'gold',   type:'stock' },
  { id:'btc',     name:'Bitcoin',                   isin:'BTC-USD',      short:'Bitcoin',    bench:'BTC/USD',       badge:'b-purple', ticker:'BINANCE:BTCUSDT',  sumId:'btc',    type:'crypto' },
];

const state   = {};   // quote data
const history = {};   // candle data (12m monthly closes)
let barChart  = null;
const sparkCharts = {};

// ── API calls ──
async function getQuote(ticker) {
  const r = await fetch(`${BASE}/quote?symbol=${encodeURIComponent(ticker)}&token=${KEY}`);
  const d = await r.json();
  if (!d.c || d.c === 0) return null;
  return { price: d.c, prev: d.pc, chg: d.dp, diff: d.d };
}

async function getCandles(ticker, type) {
  const to   = Math.floor(Date.now() / 1000);
  const from = to - 60 * 60 * 24 * 365; // 12 months
  let url;
  if (type === 'crypto') {
    url = `${BASE}/crypto/candle?symbol=${encodeURIComponent(ticker)}&resolution=M&from=${from}&to=${to}&token=${KEY}`;
  } else {
    url = `${BASE}/stock/candle?symbol=${encodeURIComponent(ticker)}&resolution=M&from=${from}&to=${to}&token=${KEY}`;
  }
  const r = await fetch(url);
  const d = await r.json();
  if (d.s !== 'ok' || !d.c) return null;
  // pair timestamps with close prices, take last 12
  const pairs = d.t.map((t, i) => ({ t, c: d.c[i] })).slice(-12);
  return pairs;
}

// ── format ──
function fp(v) {
  if (v == null) return '—';
  if (v > 10000) return v.toLocaleString('es-ES', { maximumFractionDigits: 0 });
  return v.toLocaleString('es-ES', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
}
function fc(chg) {
  if (chg == null) return '···';
  return (chg >= 0 ? '+' : '') + chg.toFixed(2) + '%';
}
function cc(chg) { return chg == null ? 'neu' : chg >= 0 ? 'pos' : 'neg'; }

// ── render summary ──
function renderSummary() {
  POSITIONS.forEach(p => {
    if (!p.sumId) return;
    const d  = state[p.id];
    const el = document.getElementById('sv-' + p.sumId);
    const sc = document.getElementById('sc-' + p.sumId);
    if (!el) return;
    if (d?.chg != null) {
      el.textContent  = fc(d.chg);
      el.className    = 'sum-val ' + cc(d.chg);
      sc.className    = 'sum-card ' + cc(d.chg);
    } else {
      el.textContent = '—';
      el.className   = 'sum-val';
    }
  });
}

// ── render cards + sparklines ──
function renderCards() {
  const grid = document.getElementById('grid');
  grid.innerHTML = POSITIONS.map(p => {
    const d  = state[p.id];
    const chgStr  = d ? fc(d.chg)  : '···';
    const cls     = d ? cc(d.chg)  : 'neu';
    const priceStr = d ? fp(d.price) : '—';
    const prevStr  = d ? fp(d.prev)  : '—';
    return `
    <div class="card">
      <div class="card-top">
        <div>
          <div class="card-name">${p.name}</div>
          <div class="card-isin">${p.isin}</div>
        </div>
        <span class="badge ${p.badge}">${p.short}</span>
      </div>
      <div class="divider"></div>
      <div class="card-rows">
        <div class="card-row"><span class="row-lbl">Benchmark</span><span class="row-val">${p.bench}</span></div>
        <div class="card-row"><span class="row-lbl">Precio</span><span class="row-price">${priceStr}</span></div>
        <div class="card-row"><span class="row-lbl">Cambio hoy</span><span class="chg ${cls}">${chgStr}</span></div>
        <div class="card-row"><span class="row-lbl">Cierre anterior</span><span class="row-val">${prevStr}</span></div>
      </div>
      <div class="sparkline-wrap">
        <canvas id="spark-${p.id}"></canvas>
      </div>
    </div>`;
  }).join('');

  // draw sparklines after DOM is ready
  requestAnimationFrame(() => drawSparklines());
}

// ── sparklines ──
function drawSparklines() {
  POSITIONS.forEach(p => {
    const canvas = document.getElementById('spark-' + p.id);
    if (!canvas) return;
    const h = history[p.id];

    if (sparkCharts[p.id]) { sparkCharts[p.id].destroy(); delete sparkCharts[p.id]; }

    if (!h || h.length < 2) {
      // show "loading" placeholder
      const ctx2 = canvas.getContext('2d');
      ctx2.clearRect(0, 0, canvas.width, canvas.height);
      return;
    }

    const labels = h.map(x => {
      const d = new Date(x.t * 1000);
      return d.toLocaleString('es-ES', { month: 'short' });
    });
    const values = h.map(x => x.c);
    const first  = values[0];
    const trend  = values[values.length - 1] >= first;
    const lineColor = trend ? '#2edb96' : '#ff5e6d';
    const fillColor = trend ? 'rgba(46,219,150,0.08)' : 'rgba(255,94,109,0.08)';

    sparkCharts[p.id] = new Chart(canvas.getContext('2d'), {
      type: 'line',
      data: {
        labels,
        datasets: [{
          data: values,
          borderColor: lineColor,
          borderWidth: 1.5,
          backgroundColor: fillColor,
          fill: true,
          tension: 0.4,
          pointRadius: 0,
          pointHoverRadius: 3,
        }]
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
        animation: false,
        plugins: {
          legend: { display: false },
          tooltip: {
            backgroundColor: '#2a3150',
            borderColor: 'rgba(255,255,255,0.1)',
            borderWidth: 1,
            titleColor: '#a8aec8',
            bodyColor: '#eef0f8',
            bodyFont: { family: 'DM Mono', size: 12 },
            callbacks: {
              title: (items) => labels[items[0].dataIndex],
              label: (item)  => fp(item.raw),
            }
          }
        },
        scales: {
          x: {
            grid: { display: false },
            border: { display: false },
            ticks: { color: '#6a728f', font: { size: 9, family: 'DM Sans' }, maxRotation: 0 }
          },
          y: {
            display: false,
          }
        }
      }
    });
  });
}

// ── bar chart ──
function renderBarChart() {
  const labels = POSITIONS.map(p => p.short);
  const data   = POSITIONS.map(p => state[p.id]?.chg ?? null);
  const colors = data.map(v => v == null ? 'rgba(255,255,255,0.06)' : v >= 0 ? 'rgba(46,219,150,0.7)' : 'rgba(255,94,109,0.7)');
  const bords  = data.map(v => v == null ? 'rgba(255,255,255,0.1)'  : v >= 0 ? '#2edb96' : '#ff5e6d');

  if (barChart) barChart.destroy();
  barChart = new Chart(document.getElementById('barChart').getContext('2d'), {
    type: 'bar',
    data: {
      labels,
      datasets: [{ data, backgroundColor: colors, borderColor: bords, borderWidth: 1, borderRadius: 5, borderSkipped: false }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: { display: false },
        tooltip: {
          backgroundColor: '#2a3150',
          borderColor: 'rgba(255,255,255,0.1)',
          borderWidth: 1,
          titleColor: '#a8aec8',
          bodyColor: '#eef0f8',
          bodyFont: { family: 'DM Mono', size: 13 },
          callbacks: { label: c => c.raw != null ? (c.raw >= 0 ? '+' : '') + c.raw.toFixed(2) + '%' : '—' }
        }
      },
      scales: {
        x: {
          grid: { display: false },
          border: { display: false },
          ticks: { color: '#6a728f', font: { family: 'DM Sans', size: 11 }, maxRotation: 20 }
        },
        y: {
          grid: { color: 'rgba(255,255,255,0.05)', drawTicks: false },
          border: { display: false },
          ticks: { color: '#6a728f', font: { family: 'DM Mono', size: 11 }, callback: v => v.toFixed(1) + '%', padding: 8 }
        }
      }
    }
  });
}

// ── countdown ──
let cdVal = 60;
let cdInt = null;
function startCountdown() {
  clearInterval(cdInt);
  cdVal = 60;
  document.getElementById('countdown').textContent = cdVal;
  cdInt = setInterval(() => {
    cdVal--;
    document.getElementById('countdown').textContent = cdVal;
    if (cdVal <= 0) loadAll();
  }, 1000);
}

// ── toast ──
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.style.display = 'block';
  setTimeout(() => { t.style.display = 'none'; }, 4000);
}

// ── main ──
async function loadAll() {
  const icon = document.getElementById('refreshIcon');
  icon.classList.add('spinning');

  // quotes
  const qResults = await Promise.allSettled(
    POSITIONS.map(p => getQuote(p.ticker).then(d => ({ id: p.id, data: d })))
  );
  qResults.forEach(r => {
    if (r.status === 'fulfilled' && r.value.data) state[r.value.id] = r.value.data;
  });

  renderSummary();
  renderCards();       // draws cards with sparkline canvases
  renderBarChart();

  // candles (after cards so canvases exist)
  const cResults = await Promise.allSettled(
    POSITIONS.map(p => getCandles(p.ticker, p.type).then(d => ({ id: p.id, data: d })))
  );
  cResults.forEach(r => {
    if (r.status === 'fulfilled' && r.value.data) history[r.value.id] = r.value.data;
  });
  drawSparklines();

  icon.classList.remove('spinning');

  const failed = POSITIONS.filter(p => !state[p.id]);
  if (failed.length) showToast(`Sin datos: ${failed.map(p => p.ticker).join(', ')}`);

  startCountdown();
}

loadAll();
</script>
</body>
</html>
