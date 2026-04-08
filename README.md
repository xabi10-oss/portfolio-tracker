# portfolio-tracker[portfolio_dashboard.html](https://github.com/user-attachments/files/26572153/portfolio_dashboard.html)
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
    --bg: #0a0c0f;
    --bg2: #111318;
    --bg3: #181c23;
    --border: rgba(255,255,255,0.07);
    --border2: rgba(255,255,255,0.12);
    --text: #e8eaf0;
    --text2: #8a8f9e;
    --text3: #4a4f5e;
    --green: #22c98b;
    --green-dim: rgba(34,201,139,0.12);
    --red: #f04f5a;
    --red-dim: rgba(240,79,90,0.12);
    --accent: #4f8ef7;
    --accent-dim: rgba(79,142,247,0.1);
    --gold: #f5c842;
    --radius: 12px;
    --radius-sm: 8px;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    min-height: 100vh;
    padding: 32px 24px;
  }

  /* ── header ── */
  .header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 32px;
    flex-wrap: wrap;
    gap: 16px;
  }
  .header-left h1 {
    font-size: 22px;
    font-weight: 600;
    letter-spacing: -0.5px;
    color: var(--text);
  }
  .header-left p {
    font-size: 13px;
    color: var(--text2);
    margin-top: 4px;
  }
  .header-right {
    display: flex;
    align-items: center;
    gap: 12px;
  }
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
  .refresh-btn.spinning svg { animation: spin 1s linear infinite; }
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

  /* ── summary strip ── */
  .summary {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
    gap: 10px;
    margin-bottom: 28px;
  }
  .summary-card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 14px 16px;
    position: relative;
    overflow: hidden;
  }
  .summary-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: var(--border2);
  }
  .summary-card.pos::before { background: var(--green); }
  .summary-card.neg::before { background: var(--red); }
  .summary-lbl { font-size: 11px; color: var(--text3); text-transform: uppercase; letter-spacing: 0.06em; margin-bottom: 6px; }
  .summary-val { font-family: 'DM Mono', monospace; font-size: 18px; font-weight: 500; }
  .summary-val.pos { color: var(--green); }
  .summary-val.neg { color: var(--red); }
  .summary-val.loading { color: var(--text3); font-size: 14px; }
  .summary-sub { font-size: 11px; color: var(--text3); margin-top: 4px; font-family: 'DM Mono', monospace; }

  /* ── cards grid ── */
  .section-label {
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: var(--text3);
    margin-bottom: 12px;
  }
  .grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 10px;
    margin-bottom: 28px;
  }
  .card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 18px 20px;
    transition: border-color 0.2s;
    position: relative;
    overflow: hidden;
  }
  .card:hover { border-color: var(--border2); }
  .card-glow {
    position: absolute;
    top: -30px; right: -30px;
    width: 80px; height: 80px;
    border-radius: 50%;
    opacity: 0.06;
    pointer-events: none;
  }
  .card-glow.pos { background: var(--green); }
  .card-glow.neg { background: var(--red); }

  .card-top {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 14px;
  }
  .card-name { font-size: 13px; font-weight: 500; color: var(--text); line-height: 1.4; }
  .card-isin { font-family: 'DM Mono', monospace; font-size: 10px; color: var(--text3); margin-top: 3px; }
  .badge {
    font-size: 10px;
    font-weight: 500;
    padding: 3px 9px;
    border-radius: 99px;
    white-space: nowrap;
    border: 1px solid;
    letter-spacing: 0.03em;
  }
  .badge-blue   { color: #6aaaf7; border-color: rgba(106,170,247,0.25); background: rgba(106,170,247,0.08); }
  .badge-teal   { color: #42d4b4; border-color: rgba(66,212,180,0.25); background: rgba(66,212,180,0.08); }
  .badge-amber  { color: #f5c842; border-color: rgba(245,200,66,0.25); background: rgba(245,200,66,0.08); }
  .badge-purple { color: #b79af7; border-color: rgba(183,154,247,0.25); background: rgba(183,154,247,0.08); }
  .badge-pink   { color: #f783c2; border-color: rgba(247,131,194,0.25); background: rgba(247,131,194,0.08); }

  .divider { border: none; border-top: 1px solid var(--border); margin: 12px 0; }

  .card-rows { display: flex; flex-direction: column; gap: 7px; }
  .card-row { display: flex; justify-content: space-between; align-items: center; }
  .row-lbl { font-size: 12px; color: var(--text3); }
  .row-val { font-size: 13px; color: var(--text2); font-family: 'DM Mono', monospace; }
  .row-price { font-size: 15px; font-weight: 500; color: var(--text); font-family: 'DM Mono', monospace; }
  .chg { font-size: 14px; font-weight: 500; font-family: 'DM Mono', monospace; padding: 2px 8px; border-radius: 5px; }
  .chg.pos { color: var(--green); background: var(--green-dim); }
  .chg.neg { color: var(--red); background: var(--red-dim); }
  .chg.loading { color: var(--text3); background: var(--bg3); font-size: 12px; }

  /* ── chart ── */
  .chart-section { background: var(--bg2); border: 1px solid var(--border); border-radius: var(--radius); padding: 20px 24px; margin-bottom: 20px; }
  .chart-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; }
  .chart-title { font-size: 13px; color: var(--text2); }
  .chart-wrap { position: relative; height: 200px; }

  /* ── footer ── */
  .footer {
    font-size: 11px;
    color: var(--text3);
    line-height: 1.7;
    border-top: 1px solid var(--border);
    padding-top: 16px;
  }
  .footer a { color: var(--text3); text-decoration: none; }
  .footer a:hover { color: var(--text2); }

  /* ── error toast ── */
  .toast {
    position: fixed;
    bottom: 24px; right: 24px;
    background: var(--bg3);
    border: 1px solid var(--red);
    color: var(--red);
    font-size: 12px;
    padding: 10px 16px;
    border-radius: var(--radius-sm);
    display: none;
    z-index: 99;
  }
</style>
</head>
<body>

<div class="header">
  <div class="header-left">
    <h1>Portfolio Benchmarks</h1>
    <p>Fondos Vanguard · Invesco Gold · Bitcoin — actualización automática cada 60 s</p>
  </div>
  <div class="header-right">
    <div class="countdown">próxima actualización: <span id="countdown">60</span>s</div>
    <button class="refresh-btn" id="refreshBtn" onclick="loadAll()">
      <svg id="refreshIcon" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <polyline points="23 4 23 10 17 10"/><polyline points="1 20 1 14 7 14"/>
        <path d="M3.51 9a9 9 0 0 1 14.85-3.36L23 10M1 14l4.64 4.36A9 9 0 0 0 20.49 15"/>
      </svg>
      Actualizar
    </button>
  </div>
</div>

<!-- summary -->
<div class="summary" id="summary">
  <div class="summary-card" id="sum-sp500">
    <div class="summary-lbl">S&amp;P 500</div>
    <div class="summary-val loading" id="sum-sp500-val">—</div>
    <div class="summary-sub" id="sum-sp500-sub">hoy</div>
  </div>
  <div class="summary-card" id="sum-mscieur">
    <div class="summary-lbl">MSCI Europe</div>
    <div class="summary-val loading" id="sum-mscieur-val">—</div>
    <div class="summary-sub" id="sum-mscieur-sub">EXV1.DE</div>
  </div>
  <div class="summary-card" id="sum-em">
    <div class="summary-lbl">MSCI EM</div>
    <div class="summary-val loading" id="sum-em-val">—</div>
    <div class="summary-sub" id="sum-em-sub">EEM</div>
  </div>
  <div class="summary-card" id="sum-japan">
    <div class="summary-lbl">MSCI Japan</div>
    <div class="summary-val loading" id="sum-japan-val">—</div>
    <div class="summary-sub" id="sum-japan-sub">EWJ</div>
  </div>
  <div class="summary-card" id="sum-gold">
    <div class="summary-lbl">Oro</div>
    <div class="summary-val loading" id="sum-gold-val">—</div>
    <div class="summary-sub" id="sum-gold-sub">XAU/USD</div>
  </div>
  <div class="summary-card" id="sum-btc">
    <div class="summary-lbl">Bitcoin</div>
    <div class="summary-val loading" id="sum-btc-val">—</div>
    <div class="summary-sub" id="sum-btc-sub">BTC/USD</div>
  </div>
</div>

<div class="section-label">Detalle por posición</div>

<div class="grid" id="grid"></div>

<div class="chart-section">
  <div class="chart-header">
    <span class="chart-title">Variación diaria por benchmark (%)</span>
  </div>
  <div class="chart-wrap">
    <canvas id="chart"></canvas>
  </div>
</div>

<div class="footer">
  Fuente: <a href="https://finnhub.io" target="_blank">Finnhub.io</a> · Tickers de referencia: ^GSPC, EXV1.DE, EEM, WSML, EWJ, OANDA:XAU_USD, BINANCE:BTCUSDT ·
  Los fondos Vanguard se valoran a cierre (D+1); estos índices son orientación intradiaria. ·
  Auto-refresco cada 60 segundos durante horario de mercado.
</div>

<div class="toast" id="toast">Error al obtener datos. Reintentando...</div>

<script>
const API_KEY = 'd7b6k8pr01ql9e4lgr2gd7b6k8pr01ql9e4lgr30';

const POSITIONS = [
  {
    id: 'sp500', name: 'Vanguard U.S. 500', isin: 'IE00BFPM9V94',
    short: 'RV USA', benchmark: 'S&P 500', badge: 'badge-blue',
    ticker: 'SPY', type: 'stock', sumId: 'sum-sp500'
  },
  {
    id: 'mscieur', name: 'Vanguard European Stock', isin: 'IE00BFPM9L96',
    short: 'RV Europa', benchmark: 'MSCI Europe', badge: 'badge-teal',
    ticker: 'EXV1.DE', type: 'stock', sumId: 'sum-mscieur'
  },
  {
    id: 'em', name: 'Vanguard Emerging Markets', isin: 'IE00BFPM9J74',
    short: 'Emergentes', benchmark: 'MSCI EM', badge: 'badge-amber',
    ticker: 'EEM', type: 'stock', sumId: 'sum-em'
  },
  {
    id: 'smallcap', name: 'Vanguard Global Small-Cap', isin: 'IE00BFRTDD83',
    short: 'Small Caps', benchmark: 'MSCI World SC', badge: 'badge-purple',
    ticker: 'WSML', type: 'stock', sumId: null
  },
  {
    id: 'japan', name: 'Vanguard Japan Stock', isin: 'IE00BFPM9P35',
    short: 'RV Japón', benchmark: 'MSCI Japan', badge: 'badge-pink',
    ticker: 'EWJ', type: 'stock', sumId: 'sum-japan'
  },
  {
    id: 'gold', name: 'Invesco Physical Gold ETC', isin: 'IE00B579F325',
    short: 'Oro', benchmark: 'Gold XAU/USD', badge: 'badge-amber',
    ticker: 'OANDA:XAU_USD', type: 'forex', sumId: 'sum-gold'
  },
  {
    id: 'btc', name: 'Bitcoin', isin: 'BTC-USD',
    short: 'Bitcoin', benchmark: 'BTC/USD', badge: 'badge-purple',
    ticker: 'BINANCE:BTCUSDT', type: 'crypto', sumId: 'sum-btc'
  },
];

const state = {};
let myChart = null;

// ── fetch one quote from Finnhub ──
async function fetchQuote(pos) {
  const base = 'https://finnhub.io/api/v1';
  let url;
  if (pos.type === 'crypto' || pos.type === 'forex') {
    url = `${base}/quote?symbol=${encodeURIComponent(pos.ticker)}&token=${API_KEY}`;
  } else {
    url = `${base}/quote?symbol=${encodeURIComponent(pos.ticker)}&token=${API_KEY}`;
  }
  const r = await fetch(url);
  if (!r.ok) throw new Error(`HTTP ${r.status}`);
  const d = await r.json();
  // Finnhub quote: { c: current, pc: prev close, d: change, dp: % change }
  if (!d.c) return null;
  return { price: d.c, prev: d.pc, chg: d.dp, diff: d.d, currency: pos.type === 'crypto' ? 'USD' : pos.type === 'forex' ? 'USD' : '' };
}

// ── format helpers ──
function fmtPrice(price, pos) {
  if (price == null) return '—';
  if (price > 10000) return price.toLocaleString('es-ES', { maximumFractionDigits: 0 });
  if (price > 100) return price.toLocaleString('es-ES', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
  return price.toLocaleString('es-ES', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
}

function fmtChg(chg) {
  if (chg == null) return null;
  return (chg >= 0 ? '+' : '') + chg.toFixed(2) + '%';
}

function chgClass(chg) { return chg == null ? 'loading' : chg >= 0 ? 'pos' : 'neg'; }

// ── render cards ──
function renderCards() {
  const grid = document.getElementById('grid');
  grid.innerHTML = POSITIONS.map(pos => {
    const d = state[pos.id];
    const priceStr = d ? fmtPrice(d.price, pos) + (d.currency ? ' ' + d.currency : '') : '—';
    const chgStr = d ? fmtChg(d.chg) : '···';
    const cc = d ? chgClass(d.chg) : 'loading';
    const glowCls = d ? (d.chg >= 0 ? 'pos' : 'neg') : '';
    return `
    <div class="card">
      <div class="card-glow ${glowCls}"></div>
      <div class="card-top">
        <div>
          <div class="card-name">${pos.name}</div>
          <div class="card-isin">${pos.isin}</div>
        </div>
        <span class="badge ${pos.badge}">${pos.short}</span>
      </div>
      <div class="divider"></div>
      <div class="card-rows">
        <div class="card-row">
          <span class="row-lbl">Benchmark</span>
          <span class="row-val">${pos.benchmark}</span>
        </div>
        <div class="card-row">
          <span class="row-lbl">Precio</span>
          <span class="row-price">${priceStr}</span>
        </div>
        <div class="card-row">
          <span class="row-lbl">Cambio hoy</span>
          <span class="chg ${cc}">${chgStr}</span>
        </div>
        ${d?.prev != null ? `<div class="card-row"><span class="row-lbl">Cierre anterior</span><span class="row-val">${fmtPrice(d.prev, pos)}</span></div>` : ''}
      </div>
    </div>`;
  }).join('');
}

// ── render summary ──
function renderSummary() {
  POSITIONS.forEach(pos => {
    if (!pos.sumId) return;
    const d = state[pos.id];
    const valEl = document.getElementById(pos.sumId + '-val');
    const cardEl = document.getElementById(pos.sumId);
    if (!valEl) return;
    if (d?.chg != null) {
      valEl.textContent = fmtChg(d.chg);
      valEl.className = 'summary-val ' + chgClass(d.chg);
      cardEl.className = 'summary-card ' + chgClass(d.chg);
    } else {
      valEl.textContent = '···';
      valEl.className = 'summary-val loading';
    }
  });
}

// ── render chart ──
function renderChart() {
  const labels = POSITIONS.map(p => p.short);
  const data = POSITIONS.map(p => state[p.id]?.chg ?? null);
  const colors = data.map(v => v == null ? 'rgba(255,255,255,0.05)' : v >= 0 ? 'rgba(34,201,139,0.75)' : 'rgba(240,79,90,0.75)');
  const borderColors = data.map(v => v == null ? 'rgba(255,255,255,0.1)' : v >= 0 ? '#22c98b' : '#f04f5a');

  if (myChart) myChart.destroy();
  const ctx = document.getElementById('chart').getContext('2d');
  myChart = new Chart(ctx, {
    type: 'bar',
    data: {
      labels,
      datasets: [{
        data,
        backgroundColor: colors,
        borderColor: borderColors,
        borderWidth: 1,
        borderRadius: 5,
        borderSkipped: false,
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: { display: false },
        tooltip: {
          backgroundColor: '#181c23',
          borderColor: 'rgba(255,255,255,0.1)',
          borderWidth: 1,
          titleColor: '#8a8f9e',
          bodyColor: '#e8eaf0',
          bodyFont: { family: 'DM Mono', size: 13 },
          callbacks: {
            label: ctx => ctx.raw != null ? (ctx.raw >= 0 ? '+' : '') + ctx.raw.toFixed(2) + '%' : '—'
          }
        }
      },
      scales: {
        x: {
          grid: { display: false },
          border: { display: false },
          ticks: { color: '#4a4f5e', font: { family: 'DM Sans', size: 11 }, maxRotation: 20 }
        },
        y: {
          grid: { color: 'rgba(255,255,255,0.04)', drawTicks: false },
          border: { display: false, dash: [4, 4] },
          ticks: {
            color: '#4a4f5e',
            font: { family: 'DM Mono', size: 11 },
            callback: v => v.toFixed(1) + '%',
            padding: 8
          }
        }
      }
    }
  });
}

// ── countdown timer ──
let countdownVal = 60;
let countdownInterval = null;

function startCountdown() {
  clearInterval(countdownInterval);
  countdownVal = 60;
  document.getElementById('countdown').textContent = countdownVal;
  countdownInterval = setInterval(() => {
    countdownVal--;
    document.getElementById('countdown').textContent = countdownVal;
    if (countdownVal <= 0) {
      loadAll();
    }
  }, 1000);
}

// ── main load ──
async function loadAll() {
  const btn = document.getElementById('refreshBtn');
  const icon = document.getElementById('refreshIcon');
  btn.classList.add('spinning');
  icon.style.animation = 'spin 1s linear infinite';

  // render skeleton
  renderCards();
  renderSummary();

  const results = await Promise.allSettled(
    POSITIONS.map(pos => fetchQuote(pos).then(d => ({ id: pos.id, data: d })))
  );

  let anyError = false;
  results.forEach(r => {
    if (r.status === 'fulfilled' && r.value.data) {
      state[r.value.id] = r.value.data;
    } else {
      anyError = true;
    }
  });

  renderCards();
  renderSummary();
  renderChart();

  btn.classList.remove('spinning');
  icon.style.animation = '';

  if (anyError) {
    const toast = document.getElementById('toast');
    toast.style.display = 'block';
    setTimeout(() => { toast.style.display = 'none'; }, 4000);
  }

  startCountdown();
}

// ── boot ──
loadAll();
</script>
</body>
</html>
