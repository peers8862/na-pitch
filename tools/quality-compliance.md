---
layout: tool
title: "Quality & Compliance Cockpit"
tool_tagline: "Track first-pass yield, defect classes, CAPA actions, and certification readiness across every production lot."
permalink: /tools/quality-compliance/
---

<style>
  @import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@300;400;500;600&family=Outfit:wght@400;600;700;800&display=swap');

  .qms-demo {
    --bg: #0b111c;
    --bg-panel: #121b2a;
    --bg-card: #182336;
    --border: #2c3d5b;
    --text: #dce6f7;
    --text-dim: #8fa4c7;
    --green: #34d399;
    --amber: #f59e0b;
    --red: #f87171;
    --cyan: #38bdf8;
    --violet: #a78bfa;
  }

  .qms-demo, .qms-demo * { box-sizing: border-box; margin: 0; padding: 0; }

  .qms-demo {
    font-family: 'IBM Plex Mono', monospace;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
  }

  .qms-head {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 12px;
    padding: 14px 18px;
    border-bottom: 1px solid var(--border);
    background: linear-gradient(180deg, rgba(56,189,248,0.09), transparent);
  }

  .qms-head h2 {
    font-family: 'Outfit', sans-serif;
    font-size: 20px;
    letter-spacing: -0.02em;
    background: linear-gradient(90deg, #9be7ff, #93c5fd);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  .qms-head p {
    font-size: 10px;
    color: var(--text-dim);
    margin-top: 3px;
    letter-spacing: 0.09em;
    text-transform: uppercase;
  }

  .qms-layout {
    display: grid;
    grid-template-columns: 1fr 320px;
    gap: 12px;
    padding: 12px;
  }

  .panel {
    border: 1px solid var(--border);
    border-radius: 10px;
    background: var(--bg-panel);
  }

  .panel-in { padding: 12px; }

  .metric-grid {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 10px;
    margin-bottom: 12px;
  }

  .metric {
    border: 1px solid var(--border);
    border-radius: 8px;
    background: var(--bg-card);
    padding: 10px;
  }

  .metric .k { color: var(--text-dim); font-size: 9px; letter-spacing: 0.08em; text-transform: uppercase; margin-bottom: 4px; }
  .metric .v { font-family: 'Outfit', sans-serif; font-size: 24px; line-height: 1; font-weight: 800; }

  .toolbar {
    display: flex;
    gap: 8px;
    margin-bottom: 10px;
    flex-wrap: wrap;
  }

  .btn {
    border: 1px solid var(--border);
    background: var(--bg-card);
    color: var(--text-dim);
    border-radius: 6px;
    padding: 6px 10px;
    font-size: 10px;
    cursor: pointer;
  }
  .btn.active, .btn:hover { border-color: var(--cyan); color: var(--cyan); background: rgba(56,189,248,0.12); }

  .table {
    border: 1px solid var(--border);
    border-radius: 8px;
    overflow: hidden;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 10px;
  }

  th, td {
    padding: 8px;
    text-align: left;
    border-bottom: 1px solid rgba(44,61,91,0.75);
  }

  th {
    background: rgba(24,35,54,0.95);
    color: var(--text-dim);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    font-size: 9px;
  }

  tr:last-child td { border-bottom: none; }
  tbody tr:hover td { background: rgba(56,189,248,0.05); }

  .status {
    font-size: 8px;
    padding: 2px 6px;
    border-radius: 999px;
    border: 1px solid;
    display: inline-block;
  }

  .ok { color: var(--green); border-color: rgba(52,211,153,0.5); background: rgba(52,211,153,0.12); }
  .warn { color: var(--amber); border-color: rgba(245,158,11,0.45); background: rgba(245,158,11,0.12); }
  .bad { color: var(--red); border-color: rgba(248,113,113,0.5); background: rgba(248,113,113,0.12); }

  .split {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
    margin-top: 10px;
  }

  .subcard {
    border: 1px solid var(--border);
    border-radius: 8px;
    background: var(--bg-card);
    padding: 10px;
  }

  .subcard h4 {
    color: var(--text-dim);
    text-transform: uppercase;
    font-size: 9px;
    letter-spacing: 0.08em;
    margin-bottom: 8px;
  }

  .bar-row { margin-bottom: 9px; }
  .bar-row:last-child { margin-bottom: 0; }
  .bar-meta {
    display: flex;
    justify-content: space-between;
    font-size: 10px;
    margin-bottom: 3px;
  }

  .bar {
    height: 8px;
    border-radius: 999px;
    overflow: hidden;
    border: 1px solid var(--border);
    background: #0a1321;
  }
  .bar > span { display: block; height: 100%; }

  .side-section {
    border-bottom: 1px solid var(--border);
    padding: 12px;
  }
  .side-section:last-child { border-bottom: none; }

  .side-section h3 {
    color: var(--text-dim);
    text-transform: uppercase;
    font-size: 10px;
    letter-spacing: 0.1em;
    margin-bottom: 8px;
  }

  .check {
    display: flex;
    justify-content: space-between;
    gap: 8px;
    padding: 7px 0;
    border-bottom: 1px solid rgba(44,61,91,0.7);
    font-size: 10px;
  }
  .check:last-child { border-bottom: none; }

  .capa {
    border: 1px solid var(--border);
    border-radius: 8px;
    background: var(--bg-card);
    padding: 8px;
    margin-bottom: 8px;
  }

  .capa-top {
    display: flex;
    justify-content: space-between;
    margin-bottom: 4px;
    font-size: 10px;
  }

  .capa p { color: var(--text-dim); font-size: 9px; line-height: 1.5; }

  @media (max-width: 1100px) {
    .qms-layout { grid-template-columns: 1fr; }
    .metric-grid { grid-template-columns: repeat(2, 1fr); }
    .split { grid-template-columns: 1fr; }
  }
</style>

<div class="qms-demo">
  <div class="qms-head">
    <div>
      <h2>Quality & Compliance Cockpit</h2>
      <p>Lot genealogy, defect intelligence, and certification readiness in one pane</p>
    </div>
    <div class="toolbar" id="line-filters"></div>
  </div>

  <div class="qms-layout">
    <section class="panel">
      <div class="panel-in">
        <div class="metric-grid" id="metric-grid"></div>

        <div class="toolbar" id="severity-filters"></div>

        <div class="table">
          <table>
            <thead>
              <tr>
                <th>Lot</th>
                <th>Line</th>
                <th>First-Pass Yield</th>
                <th>Defect PPM</th>
                <th>Top Defect</th>
                <th>Traceability</th>
                <th>Status</th>
              </tr>
            </thead>
            <tbody id="lot-body"></tbody>
          </table>
        </div>

        <div class="split">
          <div class="subcard">
            <h4>Defect Pareto</h4>
            <div id="pareto"></div>
          </div>
          <div class="subcard">
            <h4>Audit Readiness by Standard</h4>
            <div id="audit-bars"></div>
          </div>
        </div>
      </div>
    </section>

    <aside class="panel">
      <div class="side-section">
        <h3>Certification Checklist</h3>
        <div id="checklist"></div>
      </div>
      <div class="side-section">
        <h3>Open CAPA Queue</h3>
        <div id="capa-list"></div>
      </div>
    </aside>
  </div>
</div>

<script>
(function () {
  const LINES = ['All Lines', 'SMT-01', 'SMT-02', 'Final-Test'];
  const SEVERITIES = ['All', 'Healthy', 'Watch', 'Critical'];

  const LOTS = [
    { lot: 'L-2408-A', line: 'SMT-01', fpy: 98.9, ppm: 420, top: 'Insufficient solder', trace: 99.7, severity: 'Healthy' },
    { lot: 'L-2408-B', line: 'SMT-02', fpy: 96.4, ppm: 1150, top: 'Misalignment', trace: 98.8, severity: 'Watch' },
    { lot: 'L-2408-C', line: 'Final-Test', fpy: 94.8, ppm: 1880, top: 'ICT failure', trace: 97.4, severity: 'Critical' },
    { lot: 'L-2408-D', line: 'SMT-01', fpy: 97.5, ppm: 760, top: 'Bridge', trace: 99.1, severity: 'Watch' },
    { lot: 'L-2408-E', line: 'Final-Test', fpy: 99.1, ppm: 260, top: 'Label mismatch', trace: 99.9, severity: 'Healthy' },
    { lot: 'L-2408-F', line: 'SMT-02', fpy: 93.9, ppm: 2260, top: 'Polarity error', trace: 96.8, severity: 'Critical' }
  ];

  const CAPAS = [
    { id: 'CAPA-117', owner: 'Process Eng', due: '3d', level: 'Critical', note: 'Stencil aperture reduction on QFN footprint for bridge reduction.' },
    { id: 'CAPA-114', owner: 'QA Lead', due: '6d', level: 'Watch', note: 'Retrain visual inspection rubric for line-end operators.' },
    { id: 'CAPA-109', owner: 'Supplier Dev', due: '9d', level: 'Watch', note: 'Tighten MSL handling protocol with external kitting partner.' }
  ];

  const CHECKS = [
    { item: 'IPC-A-610 operator recertification', done: true },
    { item: 'ISO 9001 document control closure', done: true },
    { item: 'Traceability drill (24h recall)', done: true },
    { item: 'CAPA closure within SLA', done: false },
    { item: 'Supplier scorecard monthly review', done: false },
    { item: 'Internal audit nonconformance closure', done: true }
  ];

  let activeLine = 'All Lines';
  let activeSeverity = 'All';

  const lineFilters = document.getElementById('line-filters');
  const severityFilters = document.getElementById('severity-filters');
  const metricGrid = document.getElementById('metric-grid');
  const lotBody = document.getElementById('lot-body');
  const pareto = document.getElementById('pareto');
  const auditBars = document.getElementById('audit-bars');
  const checklist = document.getElementById('checklist');
  const capaList = document.getElementById('capa-list');

  function toneClass(severity) {
    if (severity === 'Healthy') return 'ok';
    if (severity === 'Watch') return 'warn';
    return 'bad';
  }

  function filteredLots() {
    return LOTS.filter(l => (activeLine === 'All Lines' || l.line === activeLine) && (activeSeverity === 'All' || l.severity === activeSeverity));
  }

  function renderFilters() {
    lineFilters.innerHTML = LINES.map(l => `<button class="btn ${l === activeLine ? 'active' : ''}" data-line="${l}">${l}</button>`).join('');
    severityFilters.innerHTML = SEVERITIES.map(s => `<button class="btn ${s === activeSeverity ? 'active' : ''}" data-sev="${s}">${s}</button>`).join('');

    lineFilters.querySelectorAll('button').forEach(btn => btn.onclick = () => { activeLine = btn.dataset.line; renderAll(); });
    severityFilters.querySelectorAll('button').forEach(btn => btn.onclick = () => { activeSeverity = btn.dataset.sev; renderAll(); });
  }

  function renderMetrics(lots) {
    const avgFpy = lots.length ? (lots.reduce((s, l) => s + l.fpy, 0) / lots.length) : 0;
    const avgPpm = lots.length ? Math.round(lots.reduce((s, l) => s + l.ppm, 0) / lots.length) : 0;
    const critical = lots.filter(l => l.severity === 'Critical').length;
    const trace = lots.length ? (lots.reduce((s, l) => s + l.trace, 0) / lots.length) : 0;
    const readiness = Math.round((CHECKS.filter(c => c.done).length / CHECKS.length) * 100);

    metricGrid.innerHTML = `
      <div class="metric"><div class="k">First-Pass Yield</div><div class="v" style="color:${avgFpy >= 98 ? 'var(--green)' : avgFpy >= 96 ? 'var(--amber)' : 'var(--red)'}">${avgFpy.toFixed(1)}%</div></div>
      <div class="metric"><div class="k">Defect PPM</div><div class="v" style="color:${avgPpm < 800 ? 'var(--green)' : avgPpm < 1400 ? 'var(--amber)' : 'var(--red)'}">${avgPpm}</div></div>
      <div class="metric"><div class="k">Critical Lots</div><div class="v" style="color:${critical ? 'var(--red)' : 'var(--green)'}">${critical}</div></div>
      <div class="metric"><div class="k">Traceability Coverage</div><div class="v" style="color:var(--cyan)">${trace.toFixed(1)}%</div></div>
      <div class="metric"><div class="k">Audit Readiness</div><div class="v" style="color:${readiness > 82 ? 'var(--green)' : 'var(--amber)'}">${readiness}%</div></div>
    `;
  }

  function renderLots(lots) {
    lotBody.innerHTML = lots.map(l => `
      <tr>
        <td>${l.lot}</td>
        <td>${l.line}</td>
        <td>${l.fpy.toFixed(1)}%</td>
        <td>${l.ppm}</td>
        <td>${l.top}</td>
        <td>${l.trace.toFixed(1)}%</td>
        <td><span class="status ${toneClass(l.severity)}">${l.severity}</span></td>
      </tr>
    `).join('');
  }

  function renderPareto(lots) {
    const defectCount = {};
    lots.forEach(l => { defectCount[l.top] = (defectCount[l.top] || 0) + l.ppm; });
    const max = Math.max(...Object.values(defectCount), 1);

    pareto.innerHTML = Object.entries(defectCount)
      .sort((a, b) => b[1] - a[1])
      .map(([name, val]) => `
        <div class="bar-row">
          <div class="bar-meta"><span>${name}</span><span>${val}</span></div>
          <div class="bar"><span style="width:${Math.round((val / max) * 100)}%;background:linear-gradient(90deg,var(--amber),var(--red))"></span></div>
        </div>
      `).join('') || '<div style="font-size:10px;color:var(--text-dim)">No rows in current filter.</div>';
  }

  function renderAudit() {
    const readiness = [
      { label: 'IPC-A-610', value: 92, color: 'var(--green)' },
      { label: 'ISO 9001', value: 88, color: 'var(--cyan)' },
      { label: 'Traceability SOP', value: 81, color: 'var(--amber)' },
      { label: 'CAPA SLA', value: 74, color: 'var(--red)' }
    ];

    auditBars.innerHTML = readiness.map(r => `
      <div class="bar-row">
        <div class="bar-meta"><span>${r.label}</span><span>${r.value}%</span></div>
        <div class="bar"><span style="width:${r.value}%;background:${r.color}"></span></div>
      </div>
    `).join('');
  }

  function renderChecklist() {
    checklist.innerHTML = CHECKS.map(c => `
      <div class="check">
        <span>${c.item}</span>
        <span class="status ${c.done ? 'ok' : 'warn'}">${c.done ? 'Closed' : 'Open'}</span>
      </div>
    `).join('');
  }

  function renderCAPA() {
    capaList.innerHTML = CAPAS.map(c => `
      <div class="capa">
        <div class="capa-top">
          <strong style="color:var(--violet)">${c.id}</strong>
          <span class="status ${c.level === 'Critical' ? 'bad' : 'warn'}">${c.level}</span>
        </div>
        <p>${c.note}</p>
        <p style="margin-top:6px">Owner: ${c.owner} | Due: ${c.due}</p>
      </div>
    `).join('');
  }

  function renderAll() {
    const lots = filteredLots();
    renderFilters();
    renderMetrics(lots);
    renderLots(lots);
    renderPareto(lots);
    renderAudit();
    renderChecklist();
    renderCAPA();
  }

  renderAll();
})();
</script>
