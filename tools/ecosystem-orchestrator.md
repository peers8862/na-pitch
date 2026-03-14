---
layout: tool
title: "Ecosystem Orchestrator"
tool_tagline: "Coordinate partner capacity, reliability, and specialization across the Niagara-Buffalo production network."
permalink: /tools/ecosystem-orchestrator/
---

<style>
  @import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@300;400;500;600&family=Outfit:wght@400;600;700;800&display=swap');

  .eco-demo {
    --bg: #0a0f18;
    --bg-panel: #101827;
    --bg-card: #162235;
    --border: #2a3b56;
    --text: #dce7f8;
    --text-dim: #8ca3c5;
    --cyan: #3ec9ff;
    --green: #34d399;
    --amber: #f59e0b;
    --red: #f87171;
    --violet: #a78bfa;
    --blue: #60a5fa;
  }

  .eco-demo, .eco-demo * { box-sizing: border-box; margin: 0; padding: 0; }

  .eco-demo {
    font-family: 'IBM Plex Mono', monospace;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
  }

  .eco-header {
    display: flex;
    justify-content: space-between;
    gap: 12px;
    align-items: center;
    padding: 14px 18px;
    border-bottom: 1px solid var(--border);
    background: linear-gradient(180deg, rgba(62,201,255,0.08), transparent);
  }

  .eco-title h2 {
    font-family: 'Outfit', sans-serif;
    font-size: 20px;
    letter-spacing: -0.02em;
    background: linear-gradient(90deg, #9fe6ff, #60a5fa);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  .eco-title p {
    margin-top: 3px;
    color: var(--text-dim);
    font-size: 10px;
    text-transform: uppercase;
    letter-spacing: 0.09em;
  }

  .eco-controls { display: flex; gap: 8px; flex-wrap: wrap; }

  .eco-btn {
    border: 1px solid var(--border);
    background: var(--bg-card);
    color: var(--text-dim);
    border-radius: 6px;
    padding: 6px 11px;
    font-size: 10px;
    cursor: pointer;
  }
  .eco-btn.is-active,
  .eco-btn:hover {
    border-color: var(--cyan);
    color: var(--cyan);
    background: rgba(62,201,255,0.12);
  }

  .eco-grid {
    display: grid;
    grid-template-columns: 280px 1fr 300px;
    min-height: calc(100vh - 62px);
  }

  .eco-col {
    border-right: 1px solid var(--border);
    background: rgba(16,24,39,0.9);
    overflow: auto;
  }
  .eco-col:last-child { border-right: none; border-left: 1px solid var(--border); }

  .eco-section {
    padding: 12px;
    border-bottom: 1px solid var(--border);
  }

  .eco-section h3 {
    color: var(--text-dim);
    font-size: 10px;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    margin-bottom: 9px;
  }

  .partner-card {
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 9px;
    margin-bottom: 8px;
    background: #121d30;
  }

  .partner-top {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    gap: 8px;
    margin-bottom: 5px;
  }

  .partner-name { font-size: 11px; font-weight: 600; color: #f4f8ff; }
  .partner-tag { font-size: 8px; color: var(--blue); }

  .partner-metrics {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 6px;
    margin-bottom: 7px;
  }

  .mini {
    border: 1px solid var(--border);
    border-radius: 5px;
    padding: 4px;
    text-align: center;
    font-size: 9px;
    color: var(--text-dim);
  }
  .mini strong { display: block; color: var(--text); font-size: 11px; margin-bottom: 1px; }

  .load-track {
    height: 6px;
    border-radius: 20px;
    overflow: hidden;
    background: #0b1320;
    border: 1px solid var(--border);
  }
  .load-fill { height: 100%; background: linear-gradient(90deg, #34d399, #f59e0b, #f87171); }

  .eco-center {
    padding: 14px;
    overflow: auto;
  }

  .metric-strip {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
    margin-bottom: 12px;
  }

  .metric-box {
    border: 1px solid var(--border);
    border-radius: 8px;
    background: #111a2b;
    padding: 10px;
  }

  .metric-box .lbl {
    color: var(--text-dim);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    font-size: 9px;
    margin-bottom: 4px;
  }

  .metric-box .val {
    font-family: 'Outfit', sans-serif;
    font-size: 24px;
    line-height: 1;
    font-weight: 800;
  }

  .jobs {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
  }

  .job-card {
    border: 1px solid var(--border);
    border-radius: 8px;
    background: #111a2b;
    padding: 10px;
  }

  .job-head {
    display: flex;
    justify-content: space-between;
    margin-bottom: 7px;
    font-size: 10px;
  }
  .job-id { color: var(--cyan); }
  .job-customer { color: var(--text-dim); }

  .chip-row { display: flex; gap: 5px; margin-bottom: 8px; flex-wrap: wrap; }
  .chip {
    font-size: 8px;
    padding: 2px 6px;
    border-radius: 10px;
    border: 1px solid var(--border);
    color: var(--text-dim);
  }

  .assignment {
    border-top: 1px dashed var(--border);
    padding-top: 7px;
    font-size: 10px;
    line-height: 1.65;
  }
  .assignment strong { color: #f7fbff; }

  .score {
    margin-top: 6px;
    font-size: 9px;
    color: var(--text-dim);
  }

  .right-block {
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 9px;
    margin-bottom: 10px;
    background: #111a2b;
  }

  .risk-row {
    display: flex;
    justify-content: space-between;
    font-size: 10px;
    margin-bottom: 6px;
  }

  .bar {
    height: 7px;
    border: 1px solid var(--border);
    border-radius: 999px;
    background: #0b1320;
    overflow: hidden;
  }
  .bar > span { display: block; height: 100%; }

  .timeline-row {
    font-size: 10px;
    color: var(--text-dim);
    padding: 6px 0;
    border-bottom: 1px solid rgba(42,59,86,0.55);
  }
  .timeline-row:last-child { border-bottom: none; }

  @media (max-width: 1100px) {
    .eco-grid { grid-template-columns: 1fr; }
    .eco-col, .eco-col:last-child { border-right: none; border-left: none; border-bottom: 1px solid var(--border); }
    .jobs { grid-template-columns: 1fr; }
    .metric-strip { grid-template-columns: repeat(2, 1fr); }
  }
</style>

<div class="eco-demo">
  <div class="eco-header">
    <div class="eco-title">
      <h2>Ecosystem Partner Orchestrator</h2>
      <p>Capacity-aware assignment across hamilton, niagara, fort erie, and buffalo nodes</p>
    </div>
    <div class="eco-controls" id="mode-controls"></div>
  </div>

  <div class="eco-grid">
    <aside class="eco-col">
      <div class="eco-section">
        <h3>Partner Nodes</h3>
        <div id="partner-list"></div>
      </div>
    </aside>

    <main class="eco-center">
      <div class="metric-strip" id="metric-strip"></div>
      <div class="jobs" id="job-grid"></div>
    </main>

    <aside class="eco-col">
      <div class="eco-section">
        <h3>Risk Console</h3>
        <div id="risk-console"></div>
      </div>
      <div class="eco-section">
        <h3>Coordination Timeline</h3>
        <div id="timeline"></div>
      </div>
    </aside>
  </div>
</div>

<script>
(function () {
  const MODES = [
    { id: 'margin', label: 'Margin Priority', weight: { load: 0.2, reliability: 0.2, cost: 0.6 } },
    { id: 'speed', label: 'Lead-Time Priority', weight: { load: 0.6, reliability: 0.3, cost: 0.1 } },
    { id: 'resilience', label: 'Resilience Priority', weight: { load: 0.3, reliability: 0.6, cost: 0.1 } }
  ];

  const PARTNERS = [
    { id: 'HAM-A', name: 'Hamilton Anchor SMT', region: 'Hamilton', spec: ['SMT', 'AOI'], load: 68, reliability: 97, cost: 73, queueH: 18 },
    { id: 'NIA-CNC', name: 'Niagara Precision CNC', region: 'St. Catharines', spec: ['Enclosures', 'Fixtures'], load: 53, reliability: 92, cost: 61, queueH: 11 },
    { id: 'FER-LOG', name: 'Fort Erie Cross-Border', region: 'Fort Erie', spec: ['Brokerage', 'Kitting'], load: 41, reliability: 89, cost: 58, queueH: 7 },
    { id: 'BUF-TEST', name: 'Buffalo Validation Lab', region: 'Buffalo', spec: ['Functional Test', 'Burn-In'], load: 64, reliability: 95, cost: 66, queueH: 14 },
    { id: 'WEL-HAR', name: 'Welland Harness Cell', region: 'Welland', spec: ['Harness', 'Final Pack'], load: 36, reliability: 88, cost: 54, queueH: 8 }
  ];

  const JOBS = [
    { id: 'JOB-2814', customer: 'RailSense', profile: ['SMT', 'Functional Test', 'Brokerage'], qty: 420, promiseDays: 11 },
    { id: 'JOB-2862', customer: 'AquaGrid', profile: ['SMT', 'Enclosures', 'Harness'], qty: 680, promiseDays: 16 },
    { id: 'JOB-2913', customer: 'AgriNode', profile: ['SMT', 'Burn-In', 'Final Pack'], qty: 1200, promiseDays: 19 },
    { id: 'JOB-2944', customer: 'PortSecure', profile: ['Kitting', 'Functional Test'], qty: 300, promiseDays: 9 },
    { id: 'JOB-2990', customer: 'FieldMetric', profile: ['SMT', 'Harness', 'Brokerage'], qty: 540, promiseDays: 13 },
    { id: 'JOB-3041', customer: 'HydroWatch', profile: ['SMT', 'AOI', 'Burn-In'], qty: 760, promiseDays: 15 }
  ];

  let mode = MODES[0];

  const partnerListEl = document.getElementById('partner-list');
  const metricStripEl = document.getElementById('metric-strip');
  const jobGridEl = document.getElementById('job-grid');
  const riskConsoleEl = document.getElementById('risk-console');
  const timelineEl = document.getElementById('timeline');
  const modeControlsEl = document.getElementById('mode-controls');

  function scorePartner(p, job, w) {
    const capabilityMatch = job.profile.filter(tag => p.spec.includes(tag)).length;
    const matchScore = Math.min(100, capabilityMatch * 35);
    const loadScore = 100 - p.load;
    const relScore = p.reliability;
    const costScore = 100 - p.cost;
    return Math.round(
      matchScore * 0.35 +
      loadScore * w.load +
      relScore * w.reliability +
      costScore * w.cost
    );
  }

  function assign(job) {
    const ranked = PARTNERS
      .map(p => ({ partner: p, score: scorePartner(p, job, mode.weight) }))
      .sort((a, b) => b.score - a.score);
    const primary = ranked[0];
    const backup = ranked[1];
    const predictedRisk = Math.max(4, Math.round((100 - primary.partner.reliability + primary.partner.load * 0.4) / 2));
    const etaBuffer = Math.max(0, Math.round(primary.partner.queueH / 3 - (mode.id === 'speed' ? 2 : 0)));
    return { ranked, primary, backup, predictedRisk, etaBuffer };
  }

  function renderModes() {
    modeControlsEl.innerHTML = MODES.map(m =>
      `<button class="eco-btn ${m.id === mode.id ? 'is-active' : ''}" data-mode="${m.id}">${m.label}</button>`
    ).join('');
    modeControlsEl.querySelectorAll('button').forEach(btn => {
      btn.addEventListener('click', () => {
        mode = MODES.find(m => m.id === btn.dataset.mode) || MODES[0];
        renderAll();
      });
    });
  }

  function renderPartners() {
    partnerListEl.innerHTML = PARTNERS.map(p => `
      <div class="partner-card">
        <div class="partner-top">
          <div class="partner-name">${p.name}</div>
          <div class="partner-tag">${p.id}</div>
        </div>
        <div class="partner-metrics">
          <div class="mini"><strong>${p.load}%</strong>Load</div>
          <div class="mini"><strong>${p.reliability}%</strong>SLA</div>
          <div class="mini"><strong>${p.cost}</strong>Cost Ix</div>
        </div>
        <div class="load-track"><div class="load-fill" style="width:${p.load}%"></div></div>
      </div>
    `).join('');
  }

  function renderJobs() {
    const assignments = JOBS.map(assign);
    jobGridEl.innerHTML = JOBS.map((job, i) => {
      const a = assignments[i];
      return `
      <article class="job-card">
        <div class="job-head">
          <span class="job-id">${job.id}</span>
          <span class="job-customer">${job.customer}</span>
        </div>
        <div class="chip-row">
          ${job.profile.map(tag => `<span class="chip">${tag}</span>`).join('')}
        </div>
        <div class="assignment">
          <div><strong>Primary:</strong> ${a.primary.partner.name}</div>
          <div><strong>Backup:</strong> ${a.backup.partner.name}</div>
          <div><strong>Qty / Promise:</strong> ${job.qty} units / ${job.promiseDays} days</div>
          <div><strong>ETA Buffer:</strong> +${a.etaBuffer} days</div>
        </div>
        <div class="score">Score ${a.primary.score} | Risk ${a.predictedRisk}%</div>
      </article>`;
    }).join('');

    const meanRisk = Math.round(assignments.reduce((s, a) => s + a.predictedRisk, 0) / assignments.length);
    const meanBuffer = (assignments.reduce((s, a) => s + a.etaBuffer, 0) / assignments.length).toFixed(1);
    const overloaded = PARTNERS.filter(p => p.load > 65).length;

    metricStripEl.innerHTML = `
      <div class="metric-box"><div class="lbl">Open Jobs</div><div class="val" style="color:var(--cyan)">${JOBS.length}</div></div>
      <div class="metric-box"><div class="lbl">Average Risk</div><div class="val" style="color:${meanRisk > 28 ? 'var(--amber)' : 'var(--green)'}">${meanRisk}%</div></div>
      <div class="metric-box"><div class="lbl">Avg ETA Buffer</div><div class="val" style="color:var(--violet)">${meanBuffer}d</div></div>
      <div class="metric-box"><div class="lbl">Overloaded Nodes</div><div class="val" style="color:${overloaded > 1 ? 'var(--red)' : 'var(--green)'}">${overloaded}</div></div>
    `;

    renderRisk(assignments, meanRisk);
    renderTimeline(assignments);
  }

  function renderRisk(assignments, meanRisk) {
    const byRegion = {};
    assignments.forEach(a => {
      const r = a.primary.partner.region;
      byRegion[r] = byRegion[r] || { total: 0, count: 0 };
      byRegion[r].total += a.predictedRisk;
      byRegion[r].count += 1;
    });

    riskConsoleEl.innerHTML = Object.keys(byRegion).map(region => {
      const risk = Math.round(byRegion[region].total / byRegion[region].count);
      const color = risk > 30 ? 'var(--red)' : risk > 20 ? 'var(--amber)' : 'var(--green)';
      return `
      <div class="right-block">
        <div class="risk-row"><span>${region}</span><span style="color:${color}">${risk}%</span></div>
        <div class="bar"><span style="width:${risk}%;background:${color}"></span></div>
      </div>`;
    }).join('') + `
      <div class="right-block">
        <div class="risk-row"><span>Network Composite</span><span style="color:${meanRisk > 28 ? 'var(--amber)' : 'var(--green)'}">${meanRisk}%</span></div>
        <div class="bar"><span style="width:${meanRisk}%;background:linear-gradient(90deg,var(--green),var(--amber),var(--red))"></span></div>
      </div>`;
  }

  function renderTimeline(assignments) {
    const lines = assignments
      .map((a, i) => ({
        text: `${JOBS[i].id} -> ${a.primary.partner.id} (backup ${a.backup.partner.id})`,
        risk: a.predictedRisk
      }))
      .sort((a, b) => b.risk - a.risk);

    timelineEl.innerHTML = lines.map(line => `
      <div class="timeline-row">
        <div>${line.text}</div>
        <div style="color:${line.risk > 30 ? 'var(--red)' : line.risk > 20 ? 'var(--amber)' : 'var(--green)'}">Risk ${line.risk}%</div>
      </div>
    `).join('');
  }

  function renderAll() {
    renderModes();
    renderPartners();
    renderJobs();
  }

  renderAll();
})();
</script>
