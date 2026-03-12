<html lang="es">
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
  html, body { width: 100%; max-width: 100%; overflow-x: hidden; }
</style>

<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dashboard Presupuesto 2025 – OEIT DIRESA</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Segoe+UI:wght@300;400;600;700&display=swap');
  * { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --primary: #042FA5;
    --primary-dark: #021f72;
    --primary-light: #1a47c4;
    --bg: #F3F5FB;
    --surface: #FFFFFF;
    --border: #D8DEF0;
    --text: #1a1a2e;
    --text-muted: #5a6480;
    --green: #63BE7B;
    --yellow: #FFEB84;
    --red: #F8696B;
    --header-h: 56px;
    --sidebar-w: 228px;
  }

  body {
    font-family: 'Segoe UI', system-ui, sans-serif;
    background: var(--bg);
    color: var(--text);
    font-size: 13px;
    min-height: 100vh;
  }

  /* HEADER */
  header {
    position: fixed; top: 0; left: 0; right: 0; z-index: 100;
    height: var(--header-h);
    background: var(--primary);
    display: flex; align-items: center; gap: 14px;
    padding: 0 22px;
    box-shadow: 0 2px 10px rgba(4,47,165,.4);
  }
  .logo-box {
    width: 36px; height: 36px; background: #fff;
    border-radius: 7px; display: flex; align-items: center; justify-content: center;
    flex-shrink: 0;
  }
  .logo-box svg { width: 22px; height: 22px; }
  .header-titles { display: flex; flex-direction: column; }
  .header-titles h1 { color: #fff; font-size: 14.5px; font-weight: 700; letter-spacing: .2px; line-height: 1.2; }
  .header-titles span { color: rgba(255,255,255,.6); font-size: 11px; }
  .header-right { margin-left: auto; display: flex; align-items: center; gap: 16px; }
  .header-date {
    color: rgba(255,255,255,.8); font-size: 11.5px;
    background: rgba(255,255,255,.1); padding: 5px 14px; border-radius: 6px;
  }

  /* SIDEBAR */
  aside {
    position: fixed; top: var(--header-h); left: 0; bottom: 0;
    width: var(--sidebar-w);
    background: var(--surface);
    border-right: 1px solid var(--border);
    padding: 14px 0;
    overflow-y: auto;
    z-index: 90;
  }
  .sidebar-section { padding: 4px 14px; margin-top: 6px; }
  .sidebar-label {
    font-size: 9.5px; text-transform: uppercase; letter-spacing: 1.2px;
    color: var(--text-muted); margin-bottom: 8px; font-weight: 700;
  }
  .filter-group { margin-bottom: 16px; }
  .filter-group label { display: block; font-size: 11px; color: var(--text-muted); margin-bottom: 5px; font-weight: 600; }
  .filter-group input[type=range] { width: 100%; cursor: pointer; accent-color: var(--primary); }
  .range-row { display: flex; justify-content: space-between; font-size: 10px; color: var(--text-muted); margin-top: 3px; }

  .codes-list {
    max-height: 240px; overflow-y: auto;
    border: 1px solid var(--border); border-radius: 6px;
    background: var(--bg);
  }
  .code-item {
    display: flex; align-items: center; gap: 7px;
    padding: 5px 9px; cursor: pointer; border-bottom: 1px solid var(--border);
    transition: background .12s; font-size: 11px; color: var(--text-muted);
  }
  .code-item:last-child { border-bottom: none; }
  .code-item:hover { background: #EBF0FF; color: var(--primary); }
  .code-item.active { background: #EBF0FF; color: var(--primary); font-weight: 600; }
  .code-item input[type=checkbox] { accent-color: var(--primary); cursor: pointer; width: 13px; height: 13px; flex-shrink:0; }
  .code-tag { font-weight: 700; font-size: 11px; flex-shrink:0; }
  .code-desc { font-size: 10px; white-space:nowrap; overflow:hidden; text-overflow:ellipsis; max-width: 130px; }

  .nav-link {
    display: flex; align-items: center; gap: 8px;
    padding: 8px 14px; font-size: 12.5px; color: var(--text-muted);
    cursor: pointer; border-left: 3px solid transparent;
    transition: all .15s;
  }
  .nav-link:hover { background: var(--bg); color: var(--primary); }
  .nav-link.active { background: #EBF0FF; color: var(--primary); font-weight: 600; border-left-color: var(--primary); }
  .nav-link svg { width: 15px; height: 15px; flex-shrink: 0; }
  .divider { height: 1px; background: var(--border); margin: 8px 14px; }

  /* MAIN */
  main {
    margin-left: var(--sidebar-w);
    margin-top: var(--header-h);
    padding: 16px 18px;
    min-height: calc(100vh - var(--header-h));
  }

  .page-header {
    display: flex; align-items: center; justify-content: space-between;
    margin-bottom: 14px;
  }
  .page-header h2 { font-size: 16px; font-weight: 700; color: var(--primary); }
  .page-header .period {
    font-size: 11.5px; color: var(--text-muted);
    background: var(--surface); border: 1px solid var(--border);
    padding: 5px 12px; border-radius: 6px;
  }

  .cards-row { display: grid; grid-template-columns: repeat(5, 1fr); gap: 11px; margin-bottom: 14px; }
  .chart-row { display: grid; gap: 13px; margin-bottom: 13px; }

  /* KPI */
  .kpi {
    background: var(--surface); border: 1px solid var(--border);
    border-radius: 8px; padding: 13px 15px;
    border-top: 3px solid var(--primary);
    display: flex; flex-direction: column; gap: 3px;
  }
  .kpi .kpi-label { font-size: 10px; color: var(--text-muted); text-transform: uppercase; letter-spacing: .6px; }
  .kpi .kpi-value { font-size: 21px; font-weight: 700; color: var(--primary); line-height: 1.1; }
  .kpi .kpi-sub { font-size: 10.5px; color: var(--text-muted); }
  .kpi .kpi-trend { font-size: 10.5px; font-weight: 600; }
  .kpi .kpi-trend.up { color: var(--green); }
  .kpi .kpi-trend.warn { color: #d4900a; }
  .kpi.green-top { border-top-color: var(--green); }
  .kpi.yellow-top { border-top-color: #d4900a; }

  /* PANEL */
  .panel {
    background: var(--surface); border: 1px solid var(--border);
    border-radius: 8px; padding: 15px 16px;
    display: flex; flex-direction: column; gap: 10px;
  }
  .panel-header { display: flex; align-items: center; justify-content: space-between; }
  .panel-title { font-size: 12.5px; font-weight: 700; color: var(--primary); }
  .panel canvas { max-height: 230px; }

  /* MATRIX */
  .matrix-wrap { overflow-x: auto; max-height: 380px; overflow-y: auto; }
  table.matrix { width: 100%; border-collapse: collapse; font-size: 11.5px; }
  table.matrix th {
    background: var(--primary); color: #fff;
    padding: 8px 10px; text-align: left; font-weight: 600;
    position: sticky; top: 0; z-index: 2; white-space: nowrap;
  }
  table.matrix th:not(:first-child) { text-align: right; }
  table.matrix td { padding: 6px 10px; border-bottom: 1px solid var(--border); vertical-align: middle; }
  table.matrix td:not(:first-child) { text-align: right; }
  table.matrix tr:nth-child(even) td { background: #F7F9FF; }
  table.matrix tr:hover td { background: #EBF0FF; }
  .pct-bar-wrap { display: flex; align-items: center; gap: 7px; }
  .pct-bar-bg { flex: 1; height: 10px; background: #e4e8f0; border-radius: 5px; overflow: hidden; min-width: 70px; }
  .pct-bar-fill { height: 100%; border-radius: 5px; transition: width .4s; }
  .pct-num { font-weight: 700; font-size: 12px; min-width: 40px; text-align: right; }

  /* INSIGHTS */
  .insights { display: grid; grid-template-columns: repeat(3,1fr); gap: 10px; margin-bottom: 13px; }
  .insight {
    background: var(--surface); border: 1px solid var(--border); border-radius: 8px;
    padding: 11px 13px; display: flex; gap: 10px; align-items: flex-start;
  }
  .insight-icon {
    width: 30px; height: 30px; border-radius: 7px;
    display: flex; align-items: center; justify-content: center; flex-shrink: 0;
  }
  .insight-icon svg { width: 15px; height: 15px; }
  .insight-icon.green  { background: #D6F5DF; }
  .insight-icon.yellow { background: #FFF3CC; }
  .insight-icon.blue   { background: #E0E8FF; }
  .insight h4 { font-size: 11.5px; font-weight: 700; margin-bottom: 2px; color: var(--text); }
  .insight p  { font-size: 10.5px; color: var(--text-muted); line-height: 1.5; }

  /* FOOTER */
  footer {
    margin-left: var(--sidebar-w);
    background: var(--surface); border-top: 1px solid var(--border);
    padding: 7px 20px; font-size: 10px; color: var(--text-muted);
    display: flex; justify-content: space-between; align-items: center;
  }

  /* scrollbars */
  aside::-webkit-scrollbar, .codes-list::-webkit-scrollbar { width: 4px; }
  aside::-webkit-scrollbar-thumb, .codes-list::-webkit-scrollbar-thumb { background: var(--border); border-radius: 4px; }
  .matrix-wrap::-webkit-scrollbar { width: 5px; height: 5px; }
  .matrix-wrap::-webkit-scrollbar-thumb { background: var(--border); border-radius: 4px; }
</style>
</head>
<body>

<!-- HEADER -->
<header>
  <div class="logo-box">
    <svg viewBox="0 0 32 32" fill="none">
      <path d="M16 4L5 10l11 6 11-6-11-6z" fill="#042FA5"/>
      <path d="M5 22l11 6 11-6" stroke="#042FA5" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"/>
      <path d="M5 16l11 6 11-6" stroke="#4a6dd9" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"/>
    </svg>
  </div>
  <div class="header-titles">
    <h1>Tablero de Ejecución Presupuestal 2025</h1>
    <span>OEIT DIRESA — Dirección Regional de Salud Piura</span>
  </div>
  <div class="header-right">
    <div class="header-date">📅 Ejercicio Fiscal 2025 &nbsp;·&nbsp; Fuente: SIAF-MEF</div>
  </div>
</header>

<!-- SIDEBAR -->
<aside>
  <div class="nav-link active">
    <svg viewBox="0 0 24 24" fill="currentColor"><rect x="3" y="3" width="7" height="7" rx="1"/><rect x="14" y="3" width="7" height="7" rx="1"/><rect x="3" y="14" width="7" height="7" rx="1"/><rect x="14" y="14" width="7" height="7" rx="1"/></svg>
    Resumen General
  </div>
  <div class="nav-link">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><line x1="8" y1="6" x2="21" y2="6"/><line x1="8" y1="12" x2="21" y2="12"/><line x1="8" y1="18" x2="21" y2="18"/><line x1="3" y1="6" x2="3.01" y2="6"/><line x1="3" y1="12" x2="3.01" y2="12"/><line x1="3" y1="18" x2="3.01" y2="18"/></svg>
    Detalle por Programa
  </div>
  <div class="nav-link">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><polyline points="22 12 18 12 15 21 9 3 6 12 2 12"/></svg>
    Tendencia
  </div>
  <div class="divider"></div>

  <div class="sidebar-section">
    <div class="sidebar-label">Filtros</div>

    <div class="filter-group">
      <label>Categoría Presupuestal</label>
      <div class="codes-list" id="codes-list"></div>
    </div>

    <div class="filter-group">
      <label>Rango de Avance (%)</label>
      <input type="range" min="0" max="100" value="0" id="range-avance">
      <div class="range-row">
        <span>0%</span>
        <span id="range-val" style="font-weight:700;color:var(--primary)">≥ 0%</span>
        <span>100%</span>
      </div>
    </div>
  </div>
</aside>

<!-- MAIN -->
<main>

  <div class="page-header">
    <div>
      <h2>Ejecución Presupuestal por Categoría</h2>
      <div style="font-size:11px;color:var(--text-muted);margin-top:2px;">13 categorías presupuestales · DSRSLCC Luciano Castillo Colonna</div>
    </div>
    <div class="period">Al 31 de diciembre de 2025</div>
  </div>

  <!-- KPI Cards -->
  <div class="cards-row">
    <div class="kpi">
      <span class="kpi-label">PIM Total</span>
      <span class="kpi-value" id="kpi-pim">—</span>
      <span class="kpi-sub">Presupuesto Modificado</span>
      <span class="kpi-trend up" id="kpi-pim-sub">▲ PIA: S/ 200.6M</span>
    </div>
    <div class="kpi green-top">
      <span class="kpi-label">Devengado</span>
      <span class="kpi-value" id="kpi-dev">—</span>
      <span class="kpi-sub">Total ejecutado</span>
      <span class="kpi-trend up" id="kpi-dev-sub">—</span>
    </div>
    <div class="kpi green-top">
      <span class="kpi-label">Avance Promedio</span>
      <span class="kpi-value" id="kpi-avg">—</span>
      <span class="kpi-sub">Promedio general</span>
      <span class="kpi-trend up">▲ Nivel óptimo</span>
    </div>
    <div class="kpi yellow-top">
      <span class="kpi-label">Programas &lt; 98%</span>
      <span class="kpi-value" id="kpi-low">—</span>
      <span class="kpi-sub" id="kpi-low-names">—</span>
      <span class="kpi-trend warn">⚠ Requieren seguimiento</span>
    </div>
    <div class="kpi">
      <span class="kpi-label">N° Programas</span>
      <span class="kpi-value" id="kpi-count">—</span>
      <span class="kpi-sub">Categorías activas</span>
      <span class="kpi-trend up">▲ 100% con devengado</span>
    </div>
  </div>

  <!-- Insights -->
  <div class="insights">
    <div class="insight">
      <div class="insight-icon green">
        <svg viewBox="0 0 24 24" fill="#2e7d32"><path d="M9 16.17L4.83 12l-1.42 1.41L9 19 21 7l-1.41-1.41L9 16.17z"/></svg>
      </div>
      <div>
        <h4>100% de avance en 3 programas</h4>
        <p>0104 (Emergencias Médicas), 0129 (Discapacidad) y 1002 (Violencia contra la Mujer) alcanzaron ejecución total del PIM.</p>
      </div>
    </div>
    <div class="insight">
      <div class="insight-icon yellow">
        <svg viewBox="0 0 24 24" fill="#c87800"><path d="M1 21h22L12 2 1 21zm12-3h-2v-2h2v2zm0-4h-2v-4h2v4z"/></svg>
      </div>
      <div>
        <h4>1001 · DIT con menor avance</h4>
        <p>Desarrollo Infantil Temprano registra 95.3% — el nivel más bajo. Quedan S/ 156K sin devengar al cierre del ejercicio.</p>
      </div>
    </div>
    <div class="insight">
      <div class="insight-icon blue">
        <svg viewBox="0 0 24 24" fill="#1a47c4"><path d="M11 17h2v-6h-2v6zm1-15C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8zM11 9h2V7h-2v2z"/></svg>
      </div>
      <div>
        <h4>9001 concentra el 66.5% del PIM</h4>
        <p>Acciones Centrales (S/ 138.8M) representa el mayor componente presupuestario del portafolio institucional.</p>
      </div>
    </div>
  </div>

  <!-- Row 1: Barras + Donut -->
  <div class="chart-row" style="grid-template-columns:2fr 1fr;">
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">PIM vs Devengado por Programa</span>
      </div>
      <canvas id="barChart"></canvas>
    </div>
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">Distribución del PIM</span>
      </div>
      <canvas id="donutChart"></canvas>
      <div id="donut-legend" style="display:flex;flex-wrap:wrap;gap:4px 10px;margin-top:2px;"></div>
    </div>
  </div>

  <!-- Row 2: Columnas + Línea -->
  <div class="chart-row" style="grid-template-columns:1fr 1fr;">
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">Devengado vs Compromiso Anual</span>
      </div>
      <canvas id="columnChart"></canvas>
    </div>
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">Avance % de Ejecución</span>
      </div>
      <canvas id="lineChart"></canvas>
    </div>
  </div>

  <!-- Matriz -->
  <div class="chart-row" style="grid-template-columns:1fr;">
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">Detalle de Ejecución Presupuestal</span>
        <span style="font-size:10px;color:var(--text-muted);display:flex;align-items:center;gap:6px;">
          Avance %:
          <span style="display:flex;align-items:center;gap:3px"><span style="width:10px;height:10px;background:#F8696B;border-radius:2px;display:inline-block"></span>Bajo</span>
          <span style="display:flex;align-items:center;gap:3px"><span style="width:10px;height:10px;background:#e8c830;border-radius:2px;display:inline-block"></span>Medio</span>
          <span style="display:flex;align-items:center;gap:3px"><span style="width:10px;height:10px;background:#63BE7B;border-radius:2px;display:inline-block"></span>Alto</span>
        </span>
      </div>
      <div class="matrix-wrap">
        <table class="matrix">
          <thead>
            <tr>
              <th>Categoría Presupuestal</th>
              <th>PIA (S/)</th>
              <th>PIM (S/)</th>
              <th>Certificación (S/)</th>
              <th>Compromiso (S/)</th>
              <th>Devengado (S/)</th>
              <th>Girado (S/)</th>
              <th style="min-width:140px;text-align:left">Avance %</th>
            </tr>
          </thead>
          <tbody id="matrixBody"></tbody>
        </table>
      </div>
    </div>
  </div>

</main>

<footer>
  <span>OEIT DIRESA Piura · Unidad Funcional de Estadística e Informática en Salud (UFESIS) · DSRSLCC</span>
  <span>Datos: SIAF-MEF · Ejercicio Fiscal 2025</span>
</footer>

<script>
// ── DATA ──
const raw = [
  { code:'0002', name:'SALUD MATERNO NEONATAL',                 pia:20021896,  pim:24087524,  cert:23820493,  comp:23725933,  dev:23714243,  gir:23714113,  pct:98.5 },
  { code:'0016', name:'TBC-VIH/SIDA',                           pia:1246862,   pim:2796202,   cert:2794607,   comp:2794607,   dev:2794607,   gir:2794607,   pct:99.9 },
  { code:'0017', name:'ENF. METAXÉNICAS Y ZOONOSIS',            pia:4908123,   pim:6838823,   cert:6830066,   comp:6785595,   dev:6776207,   gir:6776207,   pct:99.1 },
  { code:'0018', name:'ENF. NO TRANSMISIBLES',                  pia:532785,    pim:759834,    cert:758223,    comp:758223,    dev:758223,    gir:758193,    pct:99.8 },
  { code:'0024', name:'PREV. Y CONTROL DEL CÁNCER',             pia:1186162,   pim:5529643,   cert:5507273,   comp:5506273,   dev:5506273,   gir:5506273,   pct:99.6 },
  { code:'0068', name:'VULN. Y EMERGENCIAS DESASTRES',          pia:223888,    pim:231493,    cert:231228,    comp:231228,    dev:231228,    gir:231228,    pct:99.9 },
  { code:'0104', name:'MORT. EMERGENCIAS Y URGENCIAS MÉDICAS',  pia:13850,     pim:318961,    cert:318960,    comp:318960,    dev:318960,    gir:318960,    pct:100.0 },
  { code:'0129', name:'CONDICIONES SECUNDARIAS · DISCAPACIDAD', pia:77494,     pim:135135,    cert:135133,    comp:135133,    dev:135133,    gir:135133,    pct:100.0 },
  { code:'0131', name:'CONTROL Y PREV. EN SALUD MENTAL',        pia:9429302,   pim:10805378,  cert:10791815,  comp:10788482,  dev:10786982,  gir:10786982,  pct:99.8 },
  { code:'1001', name:'DESARROLLO INFANTIL TEMPRANO (DIT)',     pia:2227178,   pim:3300637,   cert:3144615,   comp:3144615,   dev:3144615,   gir:3144547,   pct:95.3 },
  { code:'1002', name:'REDUCCIÓN VIOLENCIA CONTRA LA MUJER',    pia:752247,    pim:617566,    cert:617368,    comp:617368,    dev:617368,    gir:617368,    pct:100.0 },
  { code:'9001', name:'ACCIONES CENTRALES',                     pia:142999544, pim:138766997, cert:138306902, comp:138303153, dev:138283877, gir:138261026, pct:99.7 },
  { code:'9002', name:'APNOP (SIN PRODUCTOS)',                  pia:3346373,   pim:14508082,  cert:14116002,  comp:14085286,  dev:14077510,  gir:14077510,  pct:97.0 },
];

// ── STATE ──
let selectedCodes = new Set(raw.map(r => r.code));
let minPct = 0;

// ── HELPERS ──
const fmtM = n => 'S/ ' + (n/1e6).toFixed(1) + 'M';
const fmtN = n => n.toLocaleString('es-PE');
const PRIMARY = '#042FA5';
const gridColor = '#E8EDF5';
const donutPalette = [PRIMARY,'#1a47c4','#3558c8','#4a6dd9','#6080e0',
  '#2e7d32','#63BE7B','#c87800','#F8696B','#c62828','#6a1b9a','#0277bd','#00695c'];

function pctColor(pct) {
  const green=[99,190,123], yellow=[232,200,48], red=[248,105,107];
  const t = Math.max(0, Math.min(100, pct)) / 100;
  let r,g,b;
  if (t < 0.5) {
    const s = t / 0.5;
    r = Math.round(red[0] + s*(yellow[0]-red[0]));
    g = Math.round(red[1] + s*(yellow[1]-red[1]));
    b = Math.round(red[2] + s*(yellow[2]-red[2]));
  } else {
    const s = (t - 0.5) / 0.5;
    r = Math.round(yellow[0] + s*(green[0]-yellow[0]));
    g = Math.round(yellow[1] + s*(green[1]-yellow[1]));
    b = Math.round(yellow[2] + s*(green[2]-yellow[2]));
  }
  return `rgb(${r},${g},${b})`;
}

// ── SLICER CODES ──
function buildCodesList() {
  const list = document.getElementById('codes-list');
  list.innerHTML = '';

  // ALL row
  const allEl = document.createElement('div');
  const allChecked = selectedCodes.size === raw.length;
  allEl.className = 'code-item' + (allChecked ? ' active' : '');
  allEl.innerHTML = `<input type="checkbox" ${allChecked?'checked':''}><span class="code-tag">TODOS</span>`;
  allEl.querySelector('input').addEventListener('change', function() {
    selectedCodes = this.checked ? new Set(raw.map(r=>r.code)) : new Set();
    buildCodesList(); refreshAll();
  });
  list.appendChild(allEl);

  raw.forEach(d => {
    const el = document.createElement('div');
    const checked = selectedCodes.has(d.code);
    el.className = 'code-item' + (checked ? ' active' : '');
    el.innerHTML = `<input type="checkbox" ${checked?'checked':''}><span class="code-tag">${d.code}</span><span class="code-desc" title="${d.name}">${d.name}</span>`;
    el.querySelector('input').addEventListener('change', function() {
      this.checked ? selectedCodes.add(d.code) : selectedCodes.delete(d.code);
      buildCodesList(); refreshAll();
    });
    list.appendChild(el);
  });
}
buildCodesList();

// ── FILTERED ──
function getFiltered() {
  return raw.filter(r => selectedCodes.has(r.code) && r.pct >= minPct);
}

// ── KPI ──
function updateKpis(data) {
  if (!data.length) { ['kpi-pim','kpi-dev','kpi-avg','kpi-low','kpi-count'].forEach(id=>document.getElementById(id).textContent='—'); return; }
  const totalPim = data.reduce((s,r)=>s+r.pim,0);
  const totalDev = data.reduce((s,r)=>s+r.dev,0);
  const avg = (data.reduce((s,r)=>s+r.pct,0)/data.length).toFixed(1);
  const low = data.filter(r=>r.pct<98);
  document.getElementById('kpi-pim').textContent    = fmtM(totalPim);
  document.getElementById('kpi-dev').textContent    = fmtM(totalDev);
  document.getElementById('kpi-avg').textContent    = avg + '%';
  document.getElementById('kpi-low').textContent    = low.length;
  document.getElementById('kpi-low-names').textContent = low.map(r=>r.code).join(' · ') || 'Ninguno';
  document.getElementById('kpi-count').textContent  = data.length;
  document.getElementById('kpi-dev-sub').textContent = '▲ ' + (totalDev/totalPim*100).toFixed(1) + '% del PIM';
}

// ── MATRIX ──
function renderMatrix(data) {
  const tbody = document.getElementById('matrixBody');
  tbody.innerHTML = '';
  data.forEach(d => {
    const color = pctColor(d.pct);
    tbody.innerHTML += `<tr>
      <td style="font-weight:600"><span style="color:var(--primary);font-weight:700;margin-right:6px">${d.code}</span>${d.name}</td>
      <td>${fmtN(d.pia)}</td>
      <td style="font-weight:600">${fmtN(d.pim)}</td>
      <td>${fmtN(d.cert)}</td>
      <td>${fmtN(d.comp)}</td>
      <td style="font-weight:600">${fmtN(d.dev)}</td>
      <td>${fmtN(d.gir)}</td>
      <td style="text-align:left">
        <div class="pct-bar-wrap">
          <div class="pct-bar-bg"><div class="pct-bar-fill" style="width:${d.pct}%;background:${color}"></div></div>
          <span class="pct-num" style="color:${color}">${d.pct}%</span>
        </div>
      </td>
    </tr>`;
  });
}

// ── CHARTS ──
Chart.defaults.font.family = "'Segoe UI', system-ui, sans-serif";
Chart.defaults.font.size   = 11;
Chart.defaults.color       = '#5a6480';

let barChart, donutChart, columnChart, lineChart;

function buildCharts(data) {
  const codes = data.map(r => r.code);
  const totalPim = data.reduce((s,r)=>s+r.pim,0) || 1;

  // BAR horizontal
  if (barChart) barChart.destroy();
  barChart = new Chart(document.getElementById('barChart'), {
    type: 'bar',
    data: {
      labels: codes,
      datasets: [
        { label: 'PIM',       data: data.map(r=>+(r.pim/1e6).toFixed(2)), backgroundColor: PRIMARY+'cc', borderRadius: 3 },
        { label: 'Devengado', data: data.map(r=>+(r.dev/1e6).toFixed(2)), backgroundColor: '#63BE7B',    borderRadius: 3 },
      ]
    },
    options: {
      indexAxis: 'y', responsive: true, maintainAspectRatio: true,
      aspectRatio: Math.max(1.5, data.length * 0.23),
      plugins: { legend: { position: 'top' }, tooltip: { callbacks: { label: ctx => ` S/ ${ctx.raw.toFixed(1)}M` } } },
      scales: {
        x: { grid: { color: gridColor }, ticks: { callback: v => 'S/' + v + 'M' } },
        y: { grid: { display: false } }
      }
    }
  });

  // DONUT
  if (donutChart) donutChart.destroy();
  donutChart = new Chart(document.getElementById('donutChart'), {
    type: 'doughnut',
    data: {
      labels: data.map(r => r.code),
      datasets: [{ data: data.map(r=>+(r.pim/1e6).toFixed(2)),
        backgroundColor: donutPalette.slice(0, data.length),
        borderWidth: 2, borderColor: '#fff', hoverOffset: 8 }]
    },
    options: {
      responsive: true, cutout: '60%',
      plugins: {
        legend: { display: false },
        tooltip: { callbacks: { label: ctx => ` ${data[ctx.dataIndex].code}: S/${ctx.raw.toFixed(1)}M · ${(ctx.raw*1e6/totalPim*100).toFixed(1)}%` } }
      }
    }
  });

  // Donut legend
  const legEl = document.getElementById('donut-legend');
  legEl.innerHTML = '';
  data.forEach((d,i) => {
    legEl.innerHTML += `<span style="font-size:10px;display:flex;align-items:center;gap:3px">
      <span style="width:8px;height:8px;background:${donutPalette[i]};border-radius:2px;display:inline-block;flex-shrink:0"></span>${d.code}
    </span>`;
  });

  // COLUMN
  if (columnChart) columnChart.destroy();
  columnChart = new Chart(document.getElementById('columnChart'), {
    type: 'bar',
    data: {
      labels: codes,
      datasets: [
        { label: 'Devengado',  data: data.map(r=>+(r.dev/1e6).toFixed(2)),  backgroundColor: PRIMARY+'dd', borderRadius: 4 },
        { label: 'Compromiso', data: data.map(r=>+(r.comp/1e6).toFixed(2)), backgroundColor: '#7a9ae8bb',  borderRadius: 4 },
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: true, aspectRatio: 2,
      plugins: { legend: { position: 'top' }, tooltip: { callbacks: { label: ctx => ` S/ ${ctx.raw.toFixed(1)}M` } } },
      scales: {
        y: { grid: { color: gridColor }, ticks: { callback: v => 'S/' + v + 'M' } },
        x: { grid: { display: false } }
      }
    }
  });

  // LINE
  const ptColors = data.map(r => pctColor(r.pct));
  if (lineChart) lineChart.destroy();
  lineChart = new Chart(document.getElementById('lineChart'), {
    type: 'line',
    data: {
      labels: codes,
      datasets: [{
        label: 'Avance %', data: data.map(r => r.pct),
        borderColor: PRIMARY, backgroundColor: PRIMARY + '18',
        fill: true, tension: 0.35,
        pointBackgroundColor: ptColors, pointBorderColor: '#fff',
        pointBorderWidth: 2, pointRadius: 6, pointHoverRadius: 8,
      }]
    },
    options: {
      responsive: true, maintainAspectRatio: true, aspectRatio: 2,
      plugins: {
        legend: { display: false },
        tooltip: { callbacks: { label: ctx => ` Avance: ${ctx.raw}%` } }
      },
      scales: {
        y: {
          min: data.length ? Math.max(0, Math.min(...data.map(r=>r.pct)) - 3) : 0,
          max: 101,
          grid: { color: gridColor },
          ticks: { callback: v => v + '%' }
        },
        x: { grid: { display: false } }
      }
    }
  });
}

// ── REFRESH ──
function refreshAll() {
  const data = getFiltered();
  updateKpis(data);
  renderMatrix(data);
  buildCharts(data);
}
refreshAll();

// ── RANGE ──
document.getElementById('range-avance').addEventListener('input', function() {
  minPct = +this.value;
  document.getElementById('range-val').textContent = '≥ ' + minPct + '%';
  refreshAll();
});
</script>
</body>
</html>
