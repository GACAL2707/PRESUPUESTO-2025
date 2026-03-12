# PRESUPUESTO 2025
<html lang="es">
<head>
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
    --sidebar-w: 220px;
  }

  body {
    font-family: 'Segoe UI', system-ui, sans-serif;
    background: var(--bg);
    color: var(--text);
    font-size: 13px;
    min-height: 100vh;
  }

  /* ── HEADER ── */
  header {
    position: fixed; top: 0; left: 0; right: 0; z-index: 100;
    height: var(--header-h);
    background: var(--primary);
    display: flex; align-items: center; gap: 14px;
    padding: 0 20px;
    box-shadow: 0 2px 8px rgba(4,47,165,.35);
  }
  .logo-box {
    width: 34px; height: 34px; background: #fff;
    border-radius: 6px; display: flex; align-items: center; justify-content: center;
  }
  .logo-box svg { width: 22px; height: 22px; }
  header h1 { color: #fff; font-size: 15px; font-weight: 600; letter-spacing: .3px; }
  header .sub { color: rgba(255,255,255,.65); font-size: 12px; margin-left: auto; }
  .pbi-badge {
    background: rgba(255,255,255,.12); color: #fff;
    font-size: 11px; padding: 3px 10px; border-radius: 12px; border: 1px solid rgba(255,255,255,.25);
    letter-spacing: .5px;
  }

  /* ── SIDEBAR ── */
  aside {
    position: fixed; top: var(--header-h); left: 0; bottom: 0;
    width: var(--sidebar-w);
    background: var(--surface);
    border-right: 1px solid var(--border);
    padding: 16px 0;
    overflow-y: auto;
    z-index: 90;
  }
  .sidebar-section { padding: 6px 16px; margin-top: 8px; }
  .sidebar-label {
    font-size: 10px; text-transform: uppercase; letter-spacing: 1px;
    color: var(--text-muted); margin-bottom: 6px; font-weight: 600;
  }
  .filter-group { margin-bottom: 14px; }
  .filter-group label { display: block; font-size: 11px; color: var(--text-muted); margin-bottom: 4px; }
  .filter-group select, .filter-group input[type=range] {
    width: 100%; border: 1px solid var(--border); border-radius: 4px;
    padding: 5px 8px; font-size: 12px; color: var(--text); background: var(--bg);
    outline: none; cursor: pointer;
  }
  .filter-group select:focus { border-color: var(--primary); }
  .slicer-chips { display: flex; flex-wrap: wrap; gap: 4px; margin-top: 4px; }
  .chip {
    padding: 3px 9px; border-radius: 12px; font-size: 10.5px; cursor: pointer;
    border: 1.5px solid var(--border); background: var(--bg); color: var(--text-muted);
    transition: all .15s;
  }
  .chip.active { background: var(--primary); color: #fff; border-color: var(--primary); }
  .chip:hover:not(.active) { border-color: var(--primary); color: var(--primary); }

  .nav-link {
    display: flex; align-items: center; gap: 8px;
    padding: 8px 16px; font-size: 12.5px; color: var(--text-muted);
    cursor: pointer; border-left: 3px solid transparent;
    transition: all .15s;
  }
  .nav-link:hover { background: var(--bg); color: var(--primary); }
  .nav-link.active {
    background: #EBF0FF; color: var(--primary); font-weight: 600;
    border-left-color: var(--primary);
  }
  .nav-link svg { width: 15px; height: 15px; flex-shrink: 0; }
  .divider { height: 1px; background: var(--border); margin: 10px 16px; }

  /* ── MAIN ── */
  main {
    margin-left: var(--sidebar-w);
    margin-top: var(--header-h);
    padding: 18px 20px;
    min-height: calc(100vh - var(--header-h));
  }

  /* ── PAGE TITLE ── */
  .page-header {
    display: flex; align-items: center; justify-content: space-between;
    margin-bottom: 16px;
  }
  .page-header h2 { font-size: 17px; font-weight: 700; color: var(--primary); }
  .page-header .period {
    font-size: 12px; color: var(--text-muted);
    background: var(--surface); border: 1px solid var(--border);
    padding: 5px 12px; border-radius: 6px;
  }

  /* ── GRID ── */
  .cards-row { display: grid; grid-template-columns: repeat(5, 1fr); gap: 12px; margin-bottom: 16px; }
  .chart-row { display: grid; gap: 14px; margin-bottom: 14px; }
  .r-2 { grid-template-columns: 1fr 1fr; }
  .r-3 { grid-template-columns: 2fr 1.4fr 1.2fr; }
  .r-full { grid-template-columns: 1fr; }

  /* ── KPI CARD ── */
  .kpi {
    background: var(--surface); border: 1px solid var(--border);
    border-radius: 8px; padding: 14px 16px;
    border-top: 3px solid var(--primary);
    display: flex; flex-direction: column; gap: 4px;
  }
  .kpi .kpi-label { font-size: 10.5px; color: var(--text-muted); text-transform: uppercase; letter-spacing: .5px; }
  .kpi .kpi-value { font-size: 22px; font-weight: 700; color: var(--primary); line-height: 1; }
  .kpi .kpi-sub { font-size: 11px; color: var(--text-muted); }
  .kpi .kpi-trend { font-size: 11px; font-weight: 600; }
  .kpi .kpi-trend.up { color: var(--green); }
  .kpi .kpi-trend.down { color: var(--red); }
  .kpi.green-top { border-top-color: var(--green); }
  .kpi.yellow-top { border-top-color: #f0c040; }
  .kpi.red-top { border-top-color: var(--red); }

  /* ── CHART PANEL ── */
  .panel {
    background: var(--surface); border: 1px solid var(--border);
    border-radius: 8px; padding: 16px;
    display: flex; flex-direction: column; gap: 10px;
  }
  .panel-header { display: flex; align-items: center; justify-content: space-between; }
  .panel-title { font-size: 13px; font-weight: 600; color: var(--primary); }
  .panel-hint { font-size: 10px; color: var(--text-muted); }
  .panel canvas { max-height: 240px; }

  /* ── MATRIX TABLE ── */
  .matrix-wrap { overflow-x: auto; }
  table.matrix {
    width: 100%; border-collapse: collapse; font-size: 11.5px;
  }
  table.matrix th {
    background: var(--primary); color: #fff;
    padding: 7px 10px; text-align: left; font-weight: 600;
    position: sticky; top: 0;
  }
  table.matrix td { padding: 6px 10px; border-bottom: 1px solid var(--border); vertical-align: middle; }
  table.matrix tr:nth-child(even) td { background: #F7F9FF; }
  table.matrix tr:hover td { background: #EBF0FF; }
  .pct-bar-wrap { display: flex; align-items: center; gap: 7px; }
  .pct-bar-bg {
    flex: 1; height: 10px; background: #eee; border-radius: 5px; overflow: hidden;
  }
  .pct-bar-fill { height: 100%; border-radius: 5px; transition: width .4s; }
  .pct-num { font-weight: 700; font-size: 12px; min-width: 36px; text-align: right; }

  /* ── INSIGHT BOX ── */
  .insights { display: grid; grid-template-columns: repeat(3,1fr); gap: 10px; margin-bottom: 14px; }
  .insight {
    background: var(--surface); border: 1px solid var(--border); border-radius: 8px;
    padding: 12px 14px; display: flex; gap: 10px; align-items: flex-start;
  }
  .insight-icon {
    width: 32px; height: 32px; border-radius: 8px;
    display: flex; align-items: center; justify-content: center; flex-shrink: 0;
  }
  .insight-icon svg { width: 16px; height: 16px; }
  .insight-icon.green { background: #D6F5DF; }
  .insight-icon.yellow { background: #FFF8D4; }
  .insight-icon.red { background: #FDEAEA; }
  .insight-icon.blue { background: #E0E8FF; }
  .insight h4 { font-size: 12px; font-weight: 600; margin-bottom: 2px; }
  .insight p { font-size: 11px; color: var(--text-muted); line-height: 1.5; }

  /* ── FOOTER ── */
  footer {
    margin-left: var(--sidebar-w);
    background: var(--surface); border-top: 1px solid var(--border);
    padding: 8px 20px;
    font-size: 10.5px; color: var(--text-muted);
    display: flex; justify-content: space-between; align-items: center;
  }

  /* ── TOOLTIP ── */
  .tooltip-label { font-family: 'Segoe UI', system-ui, sans-serif !important; font-size: 12px !important; }
</style>
</head>
<body>

<!-- HEADER -->
<header>
  <div class="logo-box">
    <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M12 2L2 7l10 5 10-5-10-5z" fill="#042FA5"/>
      <path d="M2 17l10 5 10-5" stroke="#042FA5" stroke-width="2" stroke-linecap="round"/>
      <path d="M2 12l10 5 10-5" stroke="#1a47c4" stroke-width="2" stroke-linecap="round"/>
    </svg>
  </div>
  <h1>OEIT DIRESA — Tablero de Ejecución Presupuestal 2025</h1>
  <span class="pbi-badge">Power BI Mockup</span>
  <span class="sub">Período: Enero – Diciembre 2025 &nbsp;|&nbsp; Fuente: SIAF-MEF</span>
</header>

<!-- SIDEBAR -->
<aside>
  <!-- Nav pages -->
  <div class="nav-link active">
    <svg viewBox="0 0 24 24" fill="currentColor"><path d="M3 3h8v8H3V3zm10 0h8v8h-8V3zM3 13h8v8H3v-8zm10 4l4-4 4 4-4 4-4-4z"/></svg>
    Resumen General
  </div>
  <div class="nav-link">
    <svg viewBox="0 0 24 24" fill="currentColor"><path d="M3 3h18v2H3V3zm0 4h12v2H3V7zm0 4h18v2H3v-2zm0 4h12v2H3v-2zm0 4h18v2H3v-2z"/></svg>
    Detalle por Programa
  </div>
  <div class="nav-link">
    <svg viewBox="0 0 24 24" fill="currentColor"><path d="M16 6l2.29 2.29-4.88 4.88-4-4L2 16.59 3.41 18l6-6 4 4 6.3-6.29L22 12V6h-6z"/></svg>
    Tendencia Mensual
  </div>
  <div class="divider"></div>

  <div class="sidebar-section">
    <div class="sidebar-label">Filtros</div>

    <!-- Slicer: Categoría -->
    <div class="filter-group">
      <label>Categoría Presupuestal</label>
      <div class="slicer-chips" id="chips-cat">
        <span class="chip active" data-val="all">Todos</span>
        <span class="chip" data-val="ppr">PPR</span>
        <span class="chip" data-val="apnop">APNOP</span>
        <span class="chip" data-val="acc">Acciones</span>
      </div>
    </div>

    <!-- Slicer: Avance -->
    <div class="filter-group">
      <label>Rango de Avance (%)</label>
      <input type="range" min="0" max="100" value="0" id="range-avance">
      <div style="display:flex;justify-content:space-between;font-size:10px;color:var(--text-muted);margin-top:2px;">
        <span>0%</span><span id="range-val">≥ 0%</span><span>100%</span>
      </div>
    </div>

    <!-- Slicer: Indicador -->
    <div class="filter-group">
      <label>Indicador Financiero</label>
      <select id="sel-indicador">
        <option value="dev">Devengado</option>
        <option value="comp">Compromiso Anual</option>
        <option value="cert">Certificación</option>
        <option value="gir">Girado</option>
      </select>
    </div>
  </div>
</aside>

<!-- MAIN -->
<main>

  <!-- Page header -->
  <div class="page-header">
    <div>
      <h2>📊 Ejecución Presupuestal 2025</h2>
      <div style="font-size:12px;color:var(--text-muted);margin-top:2px;">Dirección Regional de Salud – Piura</div>
    </div>
    <div class="period">🗓 Ejercicio Fiscal 2025 &nbsp;|&nbsp; Datos al 31/12/2025</div>
  </div>

  <!-- KPI Cards -->
  <div class="cards-row">
    <div class="kpi">
      <span class="kpi-label">PIM Total</span>
      <span class="kpi-value">S/ 208.7M</span>
      <span class="kpi-sub">Presupuesto Modificado</span>
      <span class="kpi-trend up">▲ vs PIA: +2.7%</span>
    </div>
    <div class="kpi green-top">
      <span class="kpi-label">Devengado</span>
      <span class="kpi-value">S/ 207.2M</span>
      <span class="kpi-sub">Total ejecutado</span>
      <span class="kpi-trend up">▲ 99.3% avance</span>
    </div>
    <div class="kpi green-top">
      <span class="kpi-label">Avance Promedio</span>
      <span class="kpi-value">99.2%</span>
      <span class="kpi-sub">Todas las categorías</span>
      <span class="kpi-trend up">▲ Excelente</span>
    </div>
    <div class="kpi yellow-top">
      <span class="kpi-label">Prog. c/ Avance &lt;98%</span>
      <span class="kpi-value">2</span>
      <span class="kpi-sub">DIT (95.3%) · APNOP (97%)</span>
      <span class="kpi-trend down">▼ Requieren acción</span>
    </div>
    <div class="kpi">
      <span class="kpi-label">N° Programas</span>
      <span class="kpi-value">13</span>
      <span class="kpi-sub">Categorías presupuestales</span>
      <span class="kpi-trend up">▲ 100% c/ devengado</span>
    </div>
  </div>

  <!-- Insights -->
  <div class="insights">
    <div class="insight">
      <div class="insight-icon green">
        <svg viewBox="0 0 24 24" fill="#2e7d32"><path d="M9 16.17L4.83 12l-1.42 1.41L9 19 21 7l-1.41-1.41L9 16.17z"/></svg>
      </div>
      <div>
        <h4>🏆 100% de Avance</h4>
        <p>Los programas 0104 (Emergencias Médicas), 0129 (Discapacidad) y 1002 (Violencia) alcanzaron ejecución total.</p>
      </div>
    </div>
    <div class="insight">
      <div class="insight-icon yellow">
        <svg viewBox="0 0 24 24" fill="#f57f17"><path d="M1 21h22L12 2 1 21zm12-3h-2v-2h2v2zm0-4h-2v-4h2v4z"/></svg>
      </div>
      <div>
        <h4>⚠️ DIT por monitorear</h4>
        <p>Programa 1001 (DIT) registra 95.3% de avance — el más bajo del portafolio. S/ 156K sin devengar.</p>
      </div>
    </div>
    <div class="insight">
      <div class="insight-icon blue">
        <svg viewBox="0 0 24 24" fill="#1a47c4"><path d="M11 17h2v-6h-2v6zm1-15C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8zM11 9h2V7h-2v2z"/></svg>
      </div>
      <div>
        <h4>💡 Acciones Centrales dominan</h4>
        <p>El programa 9001 concentra el 66.5% del PIM total (S/ 138.8M), siendo el mayor componente presupuestario.</p>
      </div>
    </div>
  </div>

  <!-- Row 1: Barras horizontales + Dona -->
  <div class="chart-row r-2" style="grid-template-columns:2fr 1fr;">
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">📊 Gráfico de Barras — PIM vs Devengado por Programa</span>
        <span class="panel-hint">Gráfico de barras agrupadas · Power BI nativo</span>
      </div>
      <canvas id="barChart"></canvas>
    </div>
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">🥧 Distribución del PIM</span>
        <span class="panel-hint">Gráfico de anillo · Power BI nativo</span>
      </div>
      <canvas id="donutChart"></canvas>
    </div>
  </div>

  <!-- Row 2: Columnas + Línea avance -->
  <div class="chart-row r-2">
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">📈 Gráfico de Columnas — Devengado vs Comprometido</span>
        <span class="panel-hint">Gráfico de columnas agrupadas · Power BI nativo</span>
      </div>
      <canvas id="columnChart"></canvas>
    </div>
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">📉 Avance % por Programa (Línea)</span>
        <span class="panel-hint">Gráfico de líneas · Power BI nativo</span>
      </div>
      <canvas id="lineChart"></canvas>
    </div>
  </div>

  <!-- Row 3: Matriz completa -->
  <div class="chart-row r-full">
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">🗃 Matriz de Ejecución Presupuestal — Vista Detallada</span>
        <span class="panel-hint">Visual Matriz (Matrix) · Power BI nativo · Formato condicional activado</span>
      </div>
      <div class="matrix-wrap">
        <table class="matrix" id="matrixTable">
          <thead>
            <tr>
              <th>Categoría Presupuestal</th>
              <th>PIA (S/)</th>
              <th>PIM (S/)</th>
              <th>Certificación (S/)</th>
              <th>Compromiso (S/)</th>
              <th>Devengado (S/)</th>
              <th>Girado (S/)</th>
              <th>Avance %</th>
            </tr>
          </thead>
          <tbody id="matrixBody"></tbody>
        </table>
      </div>
    </div>
  </div>

</main>

<footer>
  <span>OEIT DIRESA Piura · Unidad Funcional de Estadística e Informática en Salud (UFESIS)</span>
  <span>Mockup fiel a gráficos nativos Power BI · Paleta corporativa #042FA5 · 2025</span>
</footer>

<script>
// ── DATA ──
const raw = [
  { cat: '0002: SALUD MATERNO NEONATAL',           pia: 20021896, pim: 24087524, cert: 23820493, comp: 23725933, dev: 23714243, gir: 23714113, pct: 98.5 },
  { cat: '0016: TBC-VIH/SIDA',                     pia: 1246862,  pim: 2796202,  cert: 2794607,  comp: 2794607,  dev: 2794607,  gir: 2794607,  pct: 99.9 },
  { cat: '0017: ENF. METAXÉNICAS Y ZOONOSIS',      pia: 4908123,  pim: 6838823,  cert: 6830066,  comp: 6785595,  dev: 6776207,  gir: 6776207,  pct: 99.1 },
  { cat: '0018: ENF. NO TRANSMISIBLES',             pia: 532785,   pim: 759834,   cert: 758223,   comp: 758223,   dev: 758223,   gir: 758193,   pct: 99.8 },
  { cat: '0024: PREV. CONTROL DEL CÁNCER',          pia: 1186162,  pim: 5529643,  cert: 5507273,  comp: 5506273,  dev: 5506273,  gir: 5506273,  pct: 99.6 },
  { cat: '0068: VULN. Y EMERGENCIAS DESASTRES',    pia: 223888,   pim: 231493,   cert: 231228,   comp: 231228,   dev: 231228,   gir: 231228,   pct: 99.9 },
  { cat: '0104: MORT. EMERGENCIAS URGENCIAS',      pia: 13850,    pim: 318961,   cert: 318960,   comp: 318960,   dev: 318960,   gir: 318960,   pct: 100.0 },
  { cat: '0129: DISCAPACIDAD',                     pia: 77494,    pim: 135135,   cert: 135133,   comp: 135133,   dev: 135133,   gir: 135133,   pct: 100.0 },
  { cat: '0131: CONTROL SALUD MENTAL',             pia: 9429302,  pim: 10805378, cert: 10791815, comp: 10788482, dev: 10786982, gir: 10786982, pct: 99.8 },
  { cat: '1001: DIT',                              pia: 2227178,  pim: 3300637,  cert: 3144615,  comp: 3144615,  dev: 3144615,  gir: 3144547,  pct: 95.3 },
  { cat: '1002: VIOLENCIA CONTRA LA MUJER',        pia: 752247,   pim: 617566,   cert: 617368,   comp: 617368,   dev: 617368,   gir: 617368,   pct: 100.0 },
  { cat: '9001: ACCIONES CENTRALES',               pia: 142999544,pim: 138766997,cert: 138306902,comp: 138303153,dev: 138283877,gir: 138261026,pct: 99.7 },
  { cat: '9002: APNOP',                            pia: 3346373,  pim: 14508082, cert: 14116002, comp: 14085286, dev: 14077510, gir: 14077510, pct: 97.0 },
];

// Short labels
const labels = raw.map(r => r.cat.split(':')[0]);
const shortCats = raw.map(r => {
  const parts = r.cat.split(':');
  const code = parts[0];
  const name = parts[1] ? parts[1].trim().substring(0, 22) : '';
  return code + ': ' + name + (parts[1] && parts[1].trim().length > 22 ? '…' : '');
});

const fmt = n => 'S/ ' + (n/1e6).toFixed(1) + 'M';
const fmtK = n => 'S/ ' + n.toLocaleString('es-PE');

// ── COLOR by PCT ──
function pctColor(pct) {
  // Scale: 0% → red, 50% → yellow, 100% → green
  const green = [99, 190, 123], yellow = [255, 235, 132], red = [248, 105, 107];
  const t = pct / 100;
  let r, g, b;
  if (t < 0.5) {
    const s = t / 0.5;
    r = Math.round(red[0] + s * (yellow[0] - red[0]));
    g = Math.round(red[1] + s * (yellow[1] - red[1]));
    b = Math.round(red[2] + s * (yellow[2] - red[2]));
  } else {
    const s = (t - 0.5) / 0.5;
    r = Math.round(yellow[0] + s * (green[0] - yellow[0]));
    g = Math.round(yellow[1] + s * (green[1] - yellow[1]));
    b = Math.round(yellow[2] + s * (green[2] - yellow[2]));
  }
  return `rgb(${r},${g},${b})`;
}

const PRIMARY = '#042FA5';
const PRIMARY_LIGHT = '#4a6dd9';

// ── MATRIX TABLE ──
function renderMatrix(data) {
  const tbody = document.getElementById('matrixBody');
  tbody.innerHTML = '';
  data.forEach(d => {
    const color = pctColor(d.pct);
    tbody.innerHTML += `
      <tr>
        <td style="font-weight:600;max-width:260px;">${d.cat}</td>
        <td>${fmtK(d.pia)}</td>
        <td style="font-weight:600">${fmtK(d.pim)}</td>
        <td>${fmtK(d.cert)}</td>
        <td>${fmtK(d.comp)}</td>
        <td style="font-weight:600">${fmtK(d.dev)}</td>
        <td>${fmtK(d.gir)}</td>
        <td>
          <div class="pct-bar-wrap">
            <div class="pct-bar-bg">
              <div class="pct-bar-fill" style="width:${d.pct}%;background:${color}"></div>
            </div>
            <span class="pct-num" style="color:${color}">${d.pct}%</span>
          </div>
        </td>
      </tr>`;
  });
}
renderMatrix(raw);

// ── CHART DEFAULTS ──
Chart.defaults.font.family = "'Segoe UI', system-ui, sans-serif";
Chart.defaults.font.size = 11;
Chart.defaults.color = '#5a6480';
const gridColor = '#E8EDF5';

// ── BAR CHART (horizontal) ──
new Chart(document.getElementById('barChart'), {
  type: 'bar',
  data: {
    labels: labels,
    datasets: [
      { label: 'PIM', data: raw.map(r => r.pim/1e6), backgroundColor: PRIMARY + 'cc', borderRadius: 3 },
      { label: 'Devengado', data: raw.map(r => r.dev/1e6), backgroundColor: '#63BE7B', borderRadius: 3 },
    ]
  },
  options: {
    indexAxis: 'y',
    responsive: true,
    maintainAspectRatio: true,
    aspectRatio: 2.2,
    plugins: { legend: { position: 'top' }, tooltip: { callbacks: { label: ctx => ` S/ ${ctx.raw.toFixed(1)}M` } } },
    scales: {
      x: { grid: { color: gridColor }, ticks: { callback: v => 'S/' + v + 'M' } },
      y: { grid: { display: false } }
    }
  }
});

// ── DONUT ──
const donutColors = [PRIMARY, '#1a47c4', '#4a6dd9', '#7a9ae8', '#2e7d32', '#63BE7B',
  '#f57f17', '#FFEB84', '#F8696B', '#c62828', '#6a1b9a', '#0277bd', '#00695c'];
new Chart(document.getElementById('donutChart'), {
  type: 'doughnut',
  data: {
    labels: labels,
    datasets: [{ data: raw.map(r => r.pim/1e6), backgroundColor: donutColors, borderWidth: 2, borderColor: '#fff', hoverOffset: 8 }]
  },
  options: {
    responsive: true,
    cutout: '62%',
    plugins: {
      legend: { display: false },
      tooltip: { callbacks: { label: ctx => ` ${ctx.label}: S/${ctx.raw.toFixed(1)}M (${(ctx.raw/208.77*100).toFixed(1)}%)` } }
    }
  }
});

// ── COLUMN CHART ──
new Chart(document.getElementById('columnChart'), {
  type: 'bar',
  data: {
    labels: labels,
    datasets: [
      { label: 'Devengado', data: raw.map(r => r.dev/1e6), backgroundColor: PRIMARY + 'dd', borderRadius: 4 },
      { label: 'Compromiso', data: raw.map(r => r.comp/1e6), backgroundColor: '#7a9ae8aa', borderRadius: 4 },
    ]
  },
  options: {
    responsive: true,
    maintainAspectRatio: true,
    aspectRatio: 2,
    plugins: { legend: { position: 'top' }, tooltip: { callbacks: { label: ctx => ` S/ ${ctx.raw.toFixed(1)}M` } } },
    scales: {
      y: { grid: { color: gridColor }, ticks: { callback: v => 'S/' + v + 'M' } },
      x: { grid: { display: false } }
    }
  }
});

// ── LINE CHART (Avance %) ──
const lineColors = raw.map(r => pctColor(r.pct));
new Chart(document.getElementById('lineChart'), {
  type: 'line',
  data: {
    labels: labels,
    datasets: [{
      label: 'Avance %',
      data: raw.map(r => r.pct),
      borderColor: PRIMARY,
      backgroundColor: PRIMARY + '22',
      fill: true,
      tension: 0.3,
      pointBackgroundColor: lineColors,
      pointBorderColor: lineColors,
      pointRadius: 6,
      pointHoverRadius: 8,
    }]
  },
  options: {
    responsive: true,
    maintainAspectRatio: true,
    aspectRatio: 2,
    plugins: {
      legend: { display: false },
      tooltip: { callbacks: { label: ctx => ` Avance: ${ctx.raw}%` } },
      annotation: { annotations: {
        line100: { type: 'line', yMin: 100, yMax: 100, borderColor: '#63BE7B', borderWidth: 1.5, borderDash: [5,4], label: { display: true, content: '100%', position: 'end', color: '#63BE7B', font: { size: 10 } } }
      }}
    },
    scales: {
      y: { min: 93, max: 101, grid: { color: gridColor }, ticks: { callback: v => v + '%' } },
      x: { grid: { display: false } }
    }
  }
});

// ── FILTERS ──
document.getElementById('range-avance').addEventListener('input', function() {
  const min = +this.value;
  document.getElementById('range-val').textContent = '≥ ' + min + '%';
  const filtered = raw.filter(r => r.pct >= min);
  renderMatrix(filtered);
});

document.querySelectorAll('.chip').forEach(chip => {
  chip.addEventListener('click', function() {
    document.querySelectorAll('#chips-cat .chip').forEach(c => c.classList.remove('active'));
    this.classList.add('active');
  });
});
</script>

<!-- ══════════════════════════════════════════════════════════════════
     INSTRUCCIONES PARA REPRODUCIR EN POWER BI
     ══════════════════════════════════════════════════════════════════ -->
<!--
═══════════════════════════════════════════════════════════════════
  GUÍA PASO A PASO: REPRODUCIR ESTE DASHBOARD EN POWER BI DESKTOP
═══════════════════════════════════════════════════════════════════

PASO 1 — IMPORTAR DATOS
  1. Abrir Power BI Desktop → "Obtener datos" → Excel
  2. Seleccionar: presupuesto_2025.xlsx → Hoja "PRESUPUESTO 2025"
  3. En Power Query: limpiar la columna "Avance %" (quitar espacios con Text.Trim)
     = Table.TransformColumns(Source, {{"Avance %", each Number.From(Text.Trim(_)), type number}})
  4. Cargar la tabla.

PASO 2 — MODELO DE DATOS
  Tabla única: PresupuestoData
  Columnas: Categoria, PIA, PIM, Certificacion, CompromisoAnual, Devengado, Girado, AvancePct

PASO 3 — MEDIDAS DAX
  TotalPIM       = SUM(PresupuestoData[PIM])
  TotalDevengado = SUM(PresupuestoData[Devengado])
  AvancePromedio = AVERAGE(PresupuestoData[AvancePct])
  SinDevengar    = [TotalPIM] - [TotalDevengado]

PASO 4 — TARJETAS KPI (Visual: Tarjeta)
  Card 1 → Valor: TotalPIM          | Título: "PIM Total"
  Card 2 → Valor: TotalDevengado    | Título: "Devengado"
  Card 3 → Valor: AvancePromedio    | Título: "Avance Promedio %"
  Card 4 → Fórmula: COUNTROWS(FILTER(PresupuestoData, AvancePct < 98))
           Título: "Programas < 98%"
  Card 5 → Valor: DISTINCTCOUNT(PresupuestoData[Categoria]) | Título: "N° Programas"

PASO 5 — GRÁFICO DE BARRAS HORIZONTALES (PIM vs Devengado)
  Visual: Gráfico de barras agrupadas
  Eje Y:  Categoria
  Valores: PIM, Devengado
  Color PIM:       #042FA5
  Color Devengado: #63BE7B
  Formato → Etiquetas de datos: ON

PASO 6 — GRÁFICO DE ANILLO (Distribución PIM)
  Visual: Gráfico de anillo
  Leyenda: Categoria
  Valores: PIM
  Etiquetas: mostrar porcentaje
  Color principal: gradiente de #042FA5 a #7a9ae8

PASO 7 — GRÁFICO DE COLUMNAS (Devengado vs Compromiso)
  Visual: Gráfico de columnas agrupadas
  Eje X: Categoria
  Valores: Devengado, CompromisoAnual
  Color Devengado:   #042FA5
  Color Compromiso:  #7a9ae8

PASO 8 — GRÁFICO DE LÍNEAS (Avance %)
  Visual: Gráfico de líneas
  Eje X: Categoria
  Valores: AvancePct
  Color línea: #042FA5
  Relleno área: ON con opacidad 15%
  Referencia: agregar línea constante en Y=100 color #63BE7B

PASO 9 — VISUAL MATRIZ (reemplaza tabla)
  Visual: Matriz
  Filas: Categoria
  Valores: PIA, PIM, Certificacion, CompromisoAnual, Devengado, Girado, AvancePct
  Formato condicional en AvancePct:
    → Clic en AvancePct → Formato condicional → Escala de colores
    → Mínimo (0%):  #F8696B  (rojo)
    → Medio (50%):  #FFEB84  (amarillo)
    → Máximo (100%): #63BE7B (verde)

PASO 10 — FILTROS INTERACTIVOS (Segmentación)
  Segmentación 1 → Campo: Categoria  (tipo: Lista)
  Segmentación 2 → Campo: AvancePct  (tipo: Rango numérico)
  Conectar a todos los visuales via "Editar interacciones"

PASO 11 — TEMA Y PALETA
  Ir a Vista → Temas → Personalizar tema actual
  Color primario: #042FA5
  Guardar tema como JSON y aplicar a todos los visuales

PASO 12 — FONDO Y LAYOUT
  Fondo de página: #F3F5FB
  Agregar rectángulo superior con color #042FA5 para el encabezado
  Agregar cuadro de texto con título e institución

PASO 13 — PUBLICAR
  Archivo → Publicar en Power BI Service
  Compartir enlace (si no hay licencia Pro: exportar a PDF)
-->
</body>
</html>
