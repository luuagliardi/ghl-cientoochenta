# ghl-cientoochenta
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Lead Gen Funnel Dashboard</title>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; background: #f5f5f5; color: #1a1a1a; font-size: 13px; padding: 16px; min-height: 100vh; }
.header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 14px; flex-wrap: wrap; gap: 8px; }
.header h1 { font-size: 16px; font-weight: 600; color: #111; letter-spacing: -0.2px; }
.status-text { font-size: 11px; color: #666; }
.status-dot { width: 7px; height: 7px; border-radius: 50%; background: #22c55e; display: inline-block; margin-right: 5px; }
select, input[type="text"], input[type="date"] { border: 1px solid #ddd; border-radius: 6px; padding: 5px 10px; font-size: 12px; background: #fff; color: #111; outline: none; }
select:focus, input:focus { border-color: #6366f1; }
.btn { display: inline-flex; align-items: center; gap: 5px; padding: 5px 12px; border-radius: 6px; font-size: 12px; font-weight: 500; border: 1px solid #ddd; background: #fff; color: #333; cursor: pointer; transition: all 0.15s; }
.btn:hover { background: #f0f0f0; }
.btn-primary { background: #6366f1; color: #fff; border-color: #6366f1; }
.btn-primary:hover { background: #5558e8; }
.config-panel { background: #fff; border: 1px solid #e5e7eb; border-radius: 10px; padding: 14px 16px; margin-bottom: 12px; }
.config-panel h3 { font-size: 11px; font-weight: 600; color: #888; text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 10px; }
.api-row { display: flex; gap: 8px; align-items: center; margin-bottom: 8px; flex-wrap: wrap; }
.api-row label { font-size: 12px; color: #555; min-width: 80px; }
.api-row input { flex: 1; min-width: 200px; }
.help-text { font-size: 11px; color: #999; margin-top: 6px; line-height: 1.5; }
.period-bar { display: flex; align-items: center; gap: 6px; flex-wrap: wrap; margin-bottom: 12px; }
.period-btn { padding: 4px 12px; border-radius: 20px; font-size: 11px; font-weight: 500; border: 1px solid #ddd; background: #fff; color: #555; cursor: pointer; transition: all 0.15s; white-space: nowrap; }
.period-btn:hover { background: #f0f0f0; }
.period-btn.active { background: #ede9fe; border-color: #8b5cf6; color: #5b21b6; }
.period-custom { display: none; align-items: center; gap: 6px; }
.period-custom.visible { display: flex; }
.period-custom input[type="date"] { padding: 3px 8px; font-size: 11px; }
.period-label { font-size: 11px; color: #888; margin-left: 4px; }
.pipeline-selector { display: flex; flex-wrap: wrap; gap: 6px; margin-bottom: 12px; }
.pipeline-chip { display: inline-flex; align-items: center; gap: 5px; padding: 4px 10px; border: 1px solid #ddd; border-radius: 20px; font-size: 11px; cursor: pointer; background: #fff; color: #555; transition: all 0.15s; user-select: none; }
.pipeline-chip.active { background: #ede9fe; border-color: #8b5cf6; color: #5b21b6; font-weight: 500; }
.table-wrap { background: #fff; border: 1px solid #e5e7eb; border-radius: 10px; overflow: auto; margin-bottom: 10px; }
table { width: 100%; border-collapse: collapse; min-width: 700px; }
thead tr { background: #f8f8f8; }
thead th { padding: 10px 12px; text-align: left; font-size: 11px; font-weight: 600; color: #555; text-transform: uppercase; letter-spacing: 0.4px; border-bottom: 1px solid #e5e7eb; white-space: nowrap; }
thead th.num { text-align: right; }
tbody tr { border-bottom: 1px solid #f0f0f0; transition: background 0.1s; }
tbody tr:last-child { border-bottom: none; }
tbody tr:hover { background: #fafafa; }
tbody td { padding: 10px 12px; font-size: 13px; color: #222; white-space: nowrap; vertical-align: middle; }
tbody td.num { text-align: right; font-variant-numeric: tabular-nums; }
.pipeline-name { font-weight: 500; color: #111; }
.currency-input { width: 100px; text-align: right; padding: 4px 8px; font-size: 12px; border: 1px solid #e0e0e0; border-radius: 5px; background: #fafafa; color: #111; font-variant-numeric: tabular-nums; }
.currency-input:focus { border-color: #6366f1; background: #fff; outline: none; }
.inv-note { font-size: 10px; color: #bbb; display: block; text-align: right; margin-top: 2px; }
.calc-value { font-weight: 500; color: #059669; }
.calc-na { color: #ccc; font-size: 11px; }
.tag { display: inline-block; padding: 2px 7px; border-radius: 4px; font-size: 11px; font-weight: 500; }
.tag-won { background: #dcfce7; color: #15803d; }
.tag-lost { background: #fee2e2; color: #b91c1c; }
.tag-abandoned { background: #fef3c7; color: #b45309; }
.totals-row td { background: #f8f8f8; font-weight: 600; border-top: 2px solid #e5e7eb; color: #111; }
.error-banner { background: #fef2f2; border: 1px solid #fca5a5; border-radius: 8px; padding: 10px 14px; color: #b91c1c; font-size: 12px; margin-bottom: 12px; display: none; }
.loading-row td { text-align: center; padding: 40px; color: #888; }
.empty-state { text-align: center; padding: 48px 24px; color: #aaa; }
.footer-note { font-size: 11px; color: #999; text-align: center; margin-top: 8px; }
.demo-badge { font-size: 10px; background: #fef3c7; color: #92400e; padding: 2px 7px; border-radius: 4px; font-weight: 500; display: inline-block; margin-left: 8px; vertical-align: middle; }
@keyframes spin { from{transform:rotate(0deg)} to{transform:rotate(360deg)} }
</style>
</head>
<body>

<div class="header">
  <div>
    <h1>Lead Gen Funnel — Vista Ideal <span class="demo-badge" id="demoBadge">DEMO</span></h1>
    <div style="font-size:11px;color:#999;margin-top:2px;">Datos de pipelines de GHL · CPL y CAC calculados automáticamente</div>
  </div>
  <div style="display:flex;align-items:center;gap:8px;">
    <span id="statusArea" class="status-text">Modo demo activo</span>
    <button class="btn btn-primary" onclick="loadData()">↻ Actualizar</button>
  </div>
</div>

<div class="config-panel">
  <h3>Configuración de API</h3>
  <div class="api-row">
    <label>API Key</label>
    <input type="text" id="apiKey" placeholder="Pegá tu Private API Key de GHL" style="font-family:monospace;font-size:11px;flex:1;min-width:200px;" />
  </div>
  <div class="api-row">
    <label>Location ID</label>
    <input type="text" id="locationId" placeholder="ve9EPM428h8vShlRW1KT" style="flex:1;min-width:200px;" />
  </div>
  <div class="api-row" style="justify-content:space-between;margin-bottom:0;">
    <button class="btn btn-primary" onclick="connectAndLoad()">Conectar y cargar pipelines</button>
    <button class="btn" onclick="loadDemoData()" style="font-size:11px;color:#888;">Usar datos demo</button>
  </div>
  <div class="help-text">
    API Key: GHL → Settings → API Keys → <strong>Private</strong> API Key (no el JWT). Location ID: URL del subaccount → app.gohighlevel.com/location/<strong>ESTE_ID</strong>/dashboard
  </div>
</div>

<div id="errorBanner" class="error-banner"></div>

<!-- Filtro de período -->
<div class="period-bar">
  <span style="font-size:11px;font-weight:600;color:#555;margin-right:2px;">Período:</span>
  <button class="period-btn" onclick="setPeriod('week')" id="btn-week">Esta semana</button>
  <button class="period-btn active" onclick="setPeriod('month')" id="btn-month">Este mes</button>
  <button class="period-btn" onclick="setPeriod('lastweek')" id="btn-lastweek">Semana pasada</button>
  <button class="period-btn" onclick="setPeriod('lastmonth')" id="btn-lastmonth">Mes pasado</button>
  <button class="period-btn" onclick="setPeriod('custom')" id="btn-custom">Personalizado</button>
  <div class="period-custom" id="customDates">
    <input type="date" id="dateFrom" onchange="applyCustomDates()" />
    <span style="font-size:11px;color:#aaa;">→</span>
    <input type="date" id="dateTo" onchange="applyCustomDates()" />
    <button class="btn" onclick="applyCustomDates()" style="padding:3px 10px;font-size:11px;">Aplicar</button>
  </div>
  <span class="period-label" id="periodLabel"></span>
</div>

<!-- Selector de pipelines (aparece después de conectar) -->
<div id="pipelineSelector" style="display:none;margin-bottom:12px;">
  <div style="font-size:11px;font-weight:600;color:#888;text-transform:uppercase;letter-spacing:0.5px;margin-bottom:6px;">Pipelines incluidos</div>
  <div class="pipeline-selector" id="pipelineChips"></div>
</div>

<div class="table-wrap">
  <table>
    <thead>
      <tr>
        <th style="min-width:150px;">Nombre del funnel / pipeline</th>
        <th class="num">Inversión mes<br><span style="font-weight:400;color:#bbb;font-size:10px;text-transform:none;">editable · se prorratea</span></th>
        <th class="num">Entrada</th>
        <th class="num">Propuesta enviada</th>
        <th class="num" style="color:#15803d;">Cierre ✓</th>
        <th class="num" style="color:#b91c1c;">Perdido</th>
        <th class="num" style="color:#b45309;">Abandonado</th>
        <th class="num" style="color:#0369a1;">CPL</th>
        <th class="num" style="color:#7c3aed;">CAC</th>
      </tr>
    </thead>
    <tbody id="tableBody">
      <tr><td colspan="9" class="empty-state"><p>Conectá tu API o usá los datos demo para ver la tabla</p></td></tr>
    </tbody>
    <tfoot id="tableFoot" style="display:none;">
      <tr class="totals-row">
        <td><strong>TOTAL</strong></td>
        <td class="num" id="totInvCell">—</td>
        <td class="num" id="totEntrada">—</td>
        <td class="num" id="totPropuesta">—</td>
        <td class="num" id="totCierre">—</td>
        <td class="num" id="totPerdido">—</td>
        <td class="num" id="totAbandonado">—</td>
        <td class="num" id="totCPL">—</td>
        <td class="num" id="totCAC">—</td>
      </tr>
    </tfoot>
  </table>
</div>

<div class="footer-note">
  💡 CPL = Inversión período ÷ Entrada &nbsp;·&nbsp; CAC = Inversión período ÷ Cierre &nbsp;·&nbsp; Inversión mensual se prorratea automáticamente al período seleccionado
</div>

<script>
// ── Proxies CORS en cascada ──────────────────────────────
const PROXIES = [
  u => `https://api.allorigins.win/raw?url=${encodeURIComponent(u)}`,
  u => `https://corsproxy.io/?${encodeURIComponent(u)}`,
  u => `https://api.codetabs.com/v1/proxy?quest=${encodeURIComponent(u)}`,
];

async function ghlFetch(url, options) {
  let lastErr;
  for (const fn of PROXIES) {
    try {
      const res = await fetch(fn(url), {
        headers: options.headers,
        signal: AbortSignal.timeout(9000)
      });
      if (res.ok) return res;
      if (res.status === 401 || res.status === 403)
        throw new Error(`auth_${res.status}`);
    } catch(e) {
      if (e.message.startsWith('auth_')) throw e;
      lastErr = e;
    }
  }
  throw new Error('fetch_failed: ' + (lastErr?.message || ''));
}

// ── Estado global ────────────────────────────────────────
let pipelinesData = [];
let activePipelineIds = new Set();
let savedInversions = {};   // inversión MENSUAL por pipeline id
let currentPeriod = 'month';
let dateFrom = null;
let dateTo = null;

// ── Persistencia ─────────────────────────────────────────
function loadSaved() {
  try {
    const inv = localStorage.getItem('ghl_inv');
    if (inv) savedInversions = JSON.parse(inv);
    const key = localStorage.getItem('ghl_key');
    const loc = localStorage.getItem('ghl_loc');
    if (key) document.getElementById('apiKey').value = key;
    if (loc) document.getElementById('locationId').value = loc;
  } catch(e) {}
}

function persistInversions() {
  try { localStorage.setItem('ghl_inv', JSON.stringify(savedInversions)); } catch(e) {}
}

// ── Cálculo de período ───────────────────────────────────
function getPeriodDates(period) {
  const now = new Date();
  const y = now.getFullYear(), m = now.getMonth();
  const dayOfWeek = now.getDay(); // 0=dom
  const monday = new Date(now); monday.setDate(now.getDate() - ((dayOfWeek + 6) % 7)); monday.setHours(0,0,0,0);
  const sunday = new Date(monday); sunday.setDate(monday.getDate() + 6); sunday.setHours(23,59,59,999);

  if (period === 'week') {
    return { from: monday, to: sunday, days: 7, monthDays: daysInMonth(y, m) };
  }
  if (period === 'lastweek') {
    const lm = new Date(monday); lm.setDate(monday.getDate() - 7);
    const ls = new Date(lm); ls.setDate(lm.getDate() + 6); ls.setHours(23,59,59,999);
    return { from: lm, to: ls, days: 7, monthDays: daysInMonth(y, m) };
  }
  if (period === 'month') {
    const from = new Date(y, m, 1);
    const to = new Date(y, m+1, 0, 23, 59, 59, 999);
    const days = to.getDate();
    return { from, to, days, monthDays: days };
  }
  if (period === 'lastmonth') {
    const from = new Date(y, m-1, 1);
    const to = new Date(y, m, 0, 23, 59, 59, 999);
    const days = to.getDate();
    return { from, to, days, monthDays: days };
  }
  if (period === 'custom' && dateFrom && dateTo) {
    const from = new Date(dateFrom + 'T00:00:00');
    const to = new Date(dateTo + 'T23:59:59');
    const days = Math.max(1, Math.round((to - from) / 86400000) + 1);
    return { from, to, days, monthDays: daysInMonth(y, m) };
  }
  // fallback: mes actual
  const from = new Date(y, m, 1);
  const to = new Date(y, m+1, 0, 23, 59, 59, 999);
  return { from, to, days: to.getDate(), monthDays: to.getDate() };
}

function daysInMonth(y, m) {
  return new Date(y, m+1, 0).getDate();
}

function periodInversion(monthlyInv, periodDays, monthDays) {
  if (!monthlyInv) return 0;
  // si el período ES el mes completo, devolvés el total sin prorratear
  if (periodDays >= monthDays) return monthlyInv;
  return (monthlyInv / monthDays) * periodDays;
}

function formatPeriodLabel(period, dates) {
  const fmt = d => d.toLocaleDateString('es-AR', { day:'numeric', month:'short' });
  return `${fmt(dates.from)} – ${fmt(dates.to)}`;
}

// ── UI período ───────────────────────────────────────────
function setPeriod(p) {
  currentPeriod = p;
  document.querySelectorAll('.period-btn').forEach(b => b.classList.remove('active'));
  document.getElementById('btn-' + p)?.classList.add('active');
  const custom = document.getElementById('customDates');
  if (p === 'custom') {
    custom.classList.add('visible');
  } else {
    custom.classList.remove('visible');
    const dates = getPeriodDates(p);
    document.getElementById('periodLabel').textContent = formatPeriodLabel(p, dates);
    if (pipelinesData.length > 0) loadData();
    else if (document.querySelector('tbody tr[data-id]')) reRenderWithNewPeriod();
  }
}

function applyCustomDates() {
  dateFrom = document.getElementById('dateFrom').value;
  dateTo = document.getElementById('dateTo').value;
  if (!dateFrom || !dateTo) return;
  const dates = getPeriodDates('custom');
  document.getElementById('periodLabel').textContent = formatPeriodLabel('custom', dates);
  if (pipelinesData.length > 0) loadData();
  else reRenderWithNewPeriod();
}

function reRenderWithNewPeriod() {
  // Re-renderizar solo los cálculos CPL/CAC sin recargar datos
  document.querySelectorAll('tbody tr[data-id]').forEach(tr => {
    updateRowCalcs(tr.dataset.id);
  });
  recalcAll();
}

// ── Helpers de formato ───────────────────────────────────
function fmtNum(n) {
  return n.toLocaleString('es-AR', { minimumFractionDigits: 0, maximumFractionDigits: 0 });
}

function fmtCurrency(n) {
  return '$' + fmtNum(n);
}

function escHtml(s) {
  return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
}

// ── Conexión y carga ─────────────────────────────────────
async function connectAndLoad() {
  const apiKey = document.getElementById('apiKey').value.trim();
  const locationId = document.getElementById('locationId').value.trim();
  if (!apiKey || !locationId) { showError('Completá el API Key y el Location ID.'); return; }
  try { localStorage.setItem('ghl_key', apiKey); localStorage.setItem('ghl_loc', locationId); } catch(e) {}

  setStatus('Cargando pipelines...', false);
  document.getElementById('demoBadge').style.display = 'none';

  try {
    const res = await ghlFetch(
      `https://services.leadconnectorhq.com/opportunities/pipelines?locationId=${locationId}`,
      { headers: { 'Authorization': `Bearer ${apiKey}`, 'Version': '2021-07-28', 'Accept': 'application/json' } }
    );
    const data = await res.json();
    const pipelines = data.pipelines || [];
    if (!pipelines.length) { showError('No se encontraron pipelines.'); setStatus('Sin datos', true); return; }

    pipelinesData = pipelines.map(p => ({ id: p.id, name: p.name, stages: p.stages || [] }));
    activePipelineIds = new Set(pipelinesData.map(p => p.id));
    renderPipelineChips();
    await loadData();

  } catch(e) {
    const m = e.message || '';
    if (m.includes('auth_')) {
      showError('API Key o Location ID incorrecto. Usá la Private API Key (no el JWT de la URL).');
    } else {
      showError('No se pudo conectar: ' + m + '. Intentá de nuevo en unos segundos.');
    }
    setStatus('Error de conexión', true);
  }
}

async function loadData() {
  if (!pipelinesData.length) { loadDemoData(); return; }

  const apiKey = document.getElementById('apiKey').value.trim();
  const locationId = document.getElementById('locationId').value.trim();
  const { from, to } = getPeriodDates(currentPeriod);

  setStatus('Actualizando...', false);
  showTableLoading();

  const rows = [];
  const fromTs = Math.floor(from.getTime() / 1000);
  const toTs   = Math.floor(to.getTime()   / 1000);

  for (const pipeline of pipelinesData) {
    if (!activePipelineIds.has(pipeline.id)) continue;
    const counts = { entrada:0, propuesta:0, cierre:0, perdido:0, abandonado:0 };
    let page = 1, hasMore = true;

    while (hasMore) {
      try {
        const url = `https://services.leadconnectorhq.com/opportunities/search?location_id=${locationId}&pipeline_id=${pipeline.id}&limit=100&startAfter=${(page-1)*100}&date=${fromTs}&enddate=${toTs}`;
        const res = await ghlFetch(url, {
          headers: { 'Authorization': `Bearer ${apiKey}`, 'Version': '2021-07-28', 'Accept': 'application/json' }
        });
        const data = await res.json();
        const opps = data.opportunities || [];

        for (const opp of opps) {
          const status = (opp.status || '').toLowerCase();
          const stage  = (opp.pipelineStage?.name || '').toLowerCase();
          if (status === 'won')       { counts.cierre++;     continue; }
          if (status === 'lost')      { counts.perdido++;    continue; }
          if (status === 'abandoned') { counts.abandonado++; continue; }
          if (stage.includes('propuesta') || stage.includes('cotiz') || stage.includes('enviada') || stage.includes('oferta') || stage.includes('quote')) {
            counts.propuesta++;
          } else {
            counts.entrada++;
          }
        }

        hasMore = opps.length >= 100;
        page++;
      } catch(e) { hasMore = false; }
    }

    rows.push({ id: pipeline.id, name: pipeline.name, ...counts });
  }

  renderTable(rows);
  setStatus(`Actualizado ${new Date().toLocaleTimeString('es-AR')}`, true);
}

// ── Render tabla ─────────────────────────────────────────
function renderTable(rows) {
  const body = document.getElementById('tableBody');
  if (!rows.length) {
    body.innerHTML = `<tr><td colspan="9" class="empty-state"><p>No hay pipelines activos seleccionados</p></td></tr>`;
    document.getElementById('tableFoot').style.display = 'none';
    return;
  }

  body.innerHTML = '';
  for (const row of rows) {
    const inv = parseFloat(savedInversions[row.id]) || 0;
    const tr = document.createElement('tr');
    tr.dataset.id = row.id;
    tr.dataset.entrada = row.entrada;
    tr.dataset.propuesta = row.propuesta;
    tr.dataset.cierre = row.cierre;
    tr.dataset.perdido = row.perdido;
    tr.dataset.abandonado = row.abandonado;
    tr.innerHTML = `
      <td><span class="pipeline-name">${escHtml(row.name)}</span></td>
      <td class="num">
        <input class="currency-input" data-id="${row.id}" value="${inv ? fmtNum(inv) : ''}" placeholder="$ 0" onchange="handleInvChange(this)" />
        <span class="inv-note" id="invNote_${row.id}"></span>
      </td>
      <td class="num">${fmtNum(row.entrada)}</td>
      <td class="num">${fmtNum(row.propuesta)}</td>
      <td class="num"><span class="tag tag-won">${fmtNum(row.cierre)}</span></td>
      <td class="num"><span class="tag tag-lost">${fmtNum(row.perdido)}</span></td>
      <td class="num"><span class="tag tag-abandoned">${fmtNum(row.abandonado)}</span></td>
      <td class="num" id="cpl_${row.id}"></td>
      <td class="num" id="cac_${row.id}"></td>
    `;
    body.appendChild(tr);
    updateRowCalcs(row.id);
  }
  document.getElementById('tableFoot').style.display = '';
  recalcAll();
}

function handleInvChange(input) {
  const id = input.dataset.id;
  const raw = input.value.replace(/[^\d]/g, '');
  const num = parseFloat(raw) || 0;
  input.value = num > 0 ? fmtNum(num) : '';
  savedInversions[id] = num > 0 ? num : '';
  persistInversions();
  updateRowCalcs(id);
  recalcAll();
}

function updateRowCalcs(id) {
  const tr = document.querySelector(`tr[data-id="${id}"]`);
  if (!tr) return;

  const monthlyInv = parseFloat(savedInversions[id]) || 0;
  const { days, monthDays } = getPeriodDates(currentPeriod);
  const periodInv = periodInversion(monthlyInv, days, monthDays);

  const entrada  = parseInt(tr.dataset.entrada)  || 0;
  const cierre   = parseInt(tr.dataset.cierre)   || 0;

  // Nota de prorrateo
  const noteEl = document.getElementById('invNote_' + id);
  if (noteEl && monthlyInv > 0 && days < monthDays) {
    noteEl.textContent = `↳ período: ${fmtCurrency(Math.round(periodInv))}`;
  } else if (noteEl) {
    noteEl.textContent = '';
  }

  const cpl = (periodInv && entrada) ? periodInv / entrada : null;
  const cac = (periodInv && cierre)  ? periodInv / cierre  : null;

  document.getElementById('cpl_' + id).innerHTML = cpl
    ? `<span class="calc-value">${fmtCurrency(Math.round(cpl))}</span>`
    : `<span class="calc-na">sin inversión</span>`;
  document.getElementById('cac_' + id).innerHTML = cac
    ? `<span class="calc-value">${fmtCurrency(Math.round(cac))}</span>`
    : `<span class="calc-na">sin inversión</span>`;
}

function recalcAll() {
  let totInv=0, totEntrada=0, totProp=0, totCierre=0, totPerd=0, totAban=0;
  const { days, monthDays } = getPeriodDates(currentPeriod);

  document.querySelectorAll('tbody tr[data-id]').forEach(tr => {
    const id = tr.dataset.id;
    const monthlyInv = parseFloat(savedInversions[id]) || 0;
    totInv      += periodInversion(monthlyInv, days, monthDays);
    totEntrada  += parseInt(tr.dataset.entrada)   || 0;
    totProp     += parseInt(tr.dataset.propuesta) || 0;
    totCierre   += parseInt(tr.dataset.cierre)    || 0;
    totPerd     += parseInt(tr.dataset.perdido)   || 0;
    totAban     += parseInt(tr.dataset.abandonado)|| 0;
  });

  document.getElementById('totInvCell').innerHTML  = totInv ? `<strong>${fmtCurrency(Math.round(totInv))}</strong>` : '—';
  document.getElementById('totEntrada').textContent = fmtNum(totEntrada);
  document.getElementById('totPropuesta').textContent = fmtNum(totProp);
  document.getElementById('totCierre').innerHTML   = `<strong>${fmtNum(totCierre)}</strong>`;
  document.getElementById('totPerdido').textContent = fmtNum(totPerd);
  document.getElementById('totAbandonado').textContent = fmtNum(totAban);

  const totCPL = (totInv && totEntrada) ? totInv / totEntrada : null;
  const totCAC = (totInv && totCierre)  ? totInv / totCierre  : null;
  document.getElementById('totCPL').innerHTML = totCPL ? `<strong>${fmtCurrency(Math.round(totCPL))}</strong>` : '—';
  document.getElementById('totCAC').innerHTML = totCAC ? `<strong>${fmtCurrency(Math.round(totCAC))}</strong>` : '—';
}

// ── Chips de pipelines ───────────────────────────────────
function renderPipelineChips() {
  const wrap = document.getElementById('pipelineSelector');
  const cont = document.getElementById('pipelineChips');
  if (!pipelinesData.length) { wrap.style.display = 'none'; return; }
  wrap.style.display = 'block';
  cont.innerHTML = '';
  for (const p of pipelinesData) {
    const chip = document.createElement('div');
    chip.className = 'pipeline-chip' + (activePipelineIds.has(p.id) ? ' active' : '');
    chip.dataset.id = p.id;
    chip.textContent = p.name;
    chip.onclick = () => togglePipeline(p.id);
    cont.appendChild(chip);
  }
}

function togglePipeline(id) {
  if (activePipelineIds.has(id)) activePipelineIds.delete(id);
  else activePipelineIds.add(id);
  document.querySelectorAll('.pipeline-chip').forEach(c => c.classList.toggle('active', activePipelineIds.has(c.dataset.id)));
  loadData();
}

// ── Datos demo ───────────────────────────────────────────
function loadDemoData() {
  document.getElementById('demoBadge').style.display = 'inline-block';
  setStatus('Datos demo', true);
  document.getElementById('pipelineSelector').style.display = 'none';

  const demo = [
    { id: 'meta',   name: 'Meta',    entrada:142, propuesta:38, cierre:12, perdido:18, abandonado:8 },
    { id: 'otros',  name: 'Otros',   entrada:67,  propuesta:21, cierre:8,  perdido:9,  abandonado:4 },
    { id: 'zaask',  name: 'Zaask',   entrada:98,  propuesta:29, cierre:9,  perdido:14, abandonado:6 },
    { id: 'web180', name: 'Web 180', entrada:54,  propuesta:16, cierre:5,  perdido:7,  abandonado:4 },
    { id: 'doiser', name: 'Doiser',  entrada:31,  propuesta:9,  cierre:3,  perdido:4,  abandonado:2 },
  ];

  if (!savedInversions['meta']) {
    savedInversions = { meta:150000, otros:0, zaask:120000, web180:35000, doiser:80000 };
  }

  renderTable(demo);
}

// ── Misc UI ──────────────────────────────────────────────
function showTableLoading() {
  document.getElementById('tableBody').innerHTML = `<tr class="loading-row"><td colspan="9"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#6366f1" stroke-width="2" style="animation:spin 1s linear infinite;vertical-align:middle;margin-right:6px;"><path d="M12 2v4M12 18v4M4.93 4.93l2.83 2.83M16.24 16.24l2.83 2.83M2 12h4M18 12h4M4.93 19.07l2.83-2.83M16.24 7.76l2.83-2.83"/></svg>Cargando datos de GHL...</td></tr>`;
  document.getElementById('tableFoot').style.display = 'none';
}

function showError(msg) {
  const el = document.getElementById('errorBanner');
  el.textContent = '⚠ ' + msg;
  el.style.display = 'block';
  setTimeout(() => el.style.display = 'none', 10000);
}

function setStatus(msg, done) {
  document.getElementById('statusArea').innerHTML = done
    ? `<span class="status-dot"></span>${msg}`
    : `<svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="#999" stroke-width="2" style="animation:spin 1s linear infinite;margin-right:4px;vertical-align:middle;"><path d="M12 2v4M12 18v4M4.93 4.93l2.83 2.83M16.24 16.24l2.83 2.83M2 12h4M18 12h4M4.93 19.07l2.83-2.83M16.24 7.76l2.83-2.83"/></svg>${msg}`;
}

// ── Init ─────────────────────────────────────────────────
loadSaved();
setPeriod('month');
loadDemoData();
</script>
</body>
</html>
