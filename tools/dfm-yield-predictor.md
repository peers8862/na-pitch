---
layout: tool
title: "DFM + Yield Predictor"
tool_tagline: "Model first-pass yield, defect PPM, and rework load from board complexity and process controls."
permalink: /tools/dfm-yield-predictor/
---

<style>
  @import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@300;400;500;600&family=Outfit:wght@400;600;700;800&display=swap');

  .dfm-demo { --bg:#0b111b; --panel:#121c2c; --card:#172438; --border:#2c3f60; --text:#dbe8fb; --dim:#90a6c8; --cyan:#38bdf8; --green:#34d399; --amber:#f59e0b; --red:#f87171; --violet:#a78bfa; }
  .dfm-demo, .dfm-demo * { box-sizing:border-box; margin:0; padding:0; }
  .dfm-demo { font-family:'IBM Plex Mono', monospace; background:var(--bg); color:var(--text); min-height:100vh; }

  .head { display:flex; justify-content:space-between; align-items:center; padding:14px 18px; border-bottom:1px solid var(--border); background:linear-gradient(180deg, rgba(56,189,248,.1), transparent); }
  .head h2 { font-family:'Outfit',sans-serif; font-size:20px; background:linear-gradient(90deg,#a5edff,#93c5fd); -webkit-background-clip:text; -webkit-text-fill-color:transparent; }
  .head p { margin-top:3px; color:var(--dim); font-size:10px; text-transform:uppercase; letter-spacing:.08em; }

  .grid { display:grid; grid-template-columns: 310px 1fr 310px; min-height: calc(100vh - 62px); }
  .col { border-right:1px solid var(--border); background:var(--panel); padding:12px; overflow:auto; }
  .col:last-child { border-right:none; border-left:1px solid var(--border); }

  .blk { border:1px solid var(--border); border-radius:10px; background:var(--card); padding:10px; margin-bottom:10px; }
  .blk h3 { color:var(--dim); font-size:10px; text-transform:uppercase; letter-spacing:.09em; margin-bottom:8px; }

  .field { margin-bottom:8px; }
  .field label { display:block; font-size:9px; color:var(--dim); margin-bottom:4px; }
  .field input, .field select { width:100%; font-family:inherit; font-size:11px; color:var(--text); background:#0e1726; border:1px solid var(--border); border-radius:6px; padding:7px; }

  .metrics { display:grid; grid-template-columns: repeat(4,1fr); gap:8px; margin-bottom:10px; }
  .m { border:1px solid var(--border); border-radius:8px; background:var(--card); padding:10px; }
  .m .l { color:var(--dim); font-size:9px; margin-bottom:4px; text-transform:uppercase; }
  .m .v { font-family:'Outfit',sans-serif; font-size:23px; font-weight:800; line-height:1; }

  .table { width:100%; border-collapse:collapse; font-size:10px; }
  .table th, .table td { padding:7px; border-bottom:1px solid rgba(44,63,96,.6); text-align:left; }
  .table th { font-size:9px; color:var(--dim); text-transform:uppercase; background:rgba(23,36,56,.95); }

  .bar-row { margin-bottom:9px; }
  .bar-meta { display:flex; justify-content:space-between; font-size:10px; margin-bottom:3px; }
  .bar { height:9px; border:1px solid var(--border); border-radius:999px; overflow:hidden; background:#0b1321; }
  .bar > span { display:block; height:100%; }

  .chip { font-size:8px; border:1px solid; border-radius:999px; padding:2px 6px; }
  .ok { color:var(--green); border-color:rgba(52,211,153,.5); background:rgba(52,211,153,.12); }
  .warn { color:var(--amber); border-color:rgba(245,158,11,.5); background:rgba(245,158,11,.12); }
  .bad { color:var(--red); border-color:rgba(248,113,113,.5); background:rgba(248,113,113,.12); }

  .evt { font-size:10px; color:var(--dim); padding:7px 0; border-bottom:1px solid rgba(44,63,96,.6); }

  @media (max-width: 1100px) {
    .grid { grid-template-columns:1fr; }
    .col, .col:last-child { border-right:none; border-left:none; border-bottom:1px solid var(--border); }
    .metrics { grid-template-columns: repeat(2,1fr); }
  }
</style>

<div class="dfm-demo">
  <div class="head">
    <div>
      <h2>DFM + Yield Predictor</h2>
      <p>Pre-NPI risk screening across component density, fine-pitch exposure, and process controls</p>
    </div>
  </div>

  <div class="grid">
    <aside class="col">
      <div class="blk">
        <h3>Board Complexity</h3>
        <div class="field"><label>Component Count</label><input id="comp" type="number" value="286"></div>
        <div class="field"><label>Fine-Pitch Devices</label><input id="fine" type="number" value="24"></div>
        <div class="field"><label>BGA Count</label><input id="bga" type="number" value="8"></div>
        <div class="field"><label>Layers</label><input id="layers" type="number" value="6"></div>
        <div class="field"><label>Board Area (cm²)</label><input id="area" type="number" value="92"></div>
      </div>

      <div class="blk">
        <h3>Process Baseline</h3>
        <div class="field"><label>Stencil Quality</label><select id="stencil"><option value="good">High</option><option value="avg">Medium</option><option value="poor">Low</option></select></div>
        <div class="field"><label>AOI Coverage (%)</label><input id="aoi" type="number" value="88"></div>
        <div class="field"><label>X-Ray Gate</label><select id="xray"><option value="yes">Enabled</option><option value="no">Disabled</option></select></div>
        <div class="field"><label>Operator Experience</label><select id="exp"><option value="senior">Senior</option><option value="mixed">Mixed</option><option value="new">New</option></select></div>
      </div>
    </aside>

    <main class="col">
      <div class="metrics" id="metrics"></div>

      <div class="blk">
        <h3>Top Defect Drivers</h3>
        <div id="drivers"></div>
      </div>

      <div class="blk">
        <h3>Before vs Controlled Process</h3>
        <table class="table">
          <thead><tr><th>Metric</th><th>Baseline</th><th>NAC Controlled</th><th>Delta</th></tr></thead>
          <tbody id="compare"></tbody>
        </table>
      </div>
    </main>

    <aside class="col">
      <div class="blk">
        <h3>Release Recommendation</h3>
        <div id="rec"></div>
      </div>
      <div class="blk">
        <h3>Action Queue</h3>
        <div id="actions"></div>
      </div>
    </aside>
  </div>
</div>

<script>
(function () {
  const $ = id => document.getElementById(id);
  const ids = ['comp','fine','bga','layers','area','stencil','aoi','xray','exp'];
  const els = ids.map($);

  function calc() {
    const comp = Number($('comp').value || 0);
    const fine = Number($('fine').value || 0);
    const bga = Number($('bga').value || 0);
    const layers = Number($('layers').value || 0);
    const area = Number($('area').value || 0);
    const stencil = $('stencil').value;
    const aoi = Number($('aoi').value || 0);
    const xray = $('xray').value === 'yes';
    const exp = $('exp').value;

    const complexity = comp * 0.04 + fine * 1.2 + bga * 1.8 + layers * 1.5 + area * 0.08;
    const processPenalty = (stencil === 'good' ? 4 : stencil === 'avg' ? 9 : 16) + (100 - aoi) * 0.22 + (xray ? -5 : 7) + (exp === 'senior' ? -5 : exp === 'mixed' ? 0 : 7);

    const defectPPM = Math.max(180, Math.round(220 + complexity * 22 + processPenalty * 55));
    const fpy = Math.max(89, Math.min(99.6, 99.5 - (defectPPM / 3000) - fine * 0.012 - bga * 0.02));
    const reworkH = Math.max(2.5, (defectPPM / 650) + comp * 0.007);

    const controlledPPM = Math.round(defectPPM * 0.54);
    const controlledFPY = Math.min(99.8, fpy + 2.1);
    const controlledRework = reworkH * 0.58;

    return {
      defectPPM, fpy, reworkH, controlledPPM, controlledFPY, controlledRework,
      risk: Math.min(95, Math.round((defectPPM / 35) + (100 - aoi) * 0.35)),
      drivers: [
        ['Fine-pitch placement sensitivity', Math.min(95, 20 + fine * 2.2)],
        ['BGA void/open probability', Math.min(95, 15 + bga * 4.5)],
        ['Stencil deposition variance', stencil === 'good' ? 28 : stencil === 'avg' ? 52 : 78],
        ['Inspection escape exposure', Math.max(8, 100 - aoi)]
      ]
    };
  }

  function clsRisk(v) { return v < 28 ? 'ok' : v < 52 ? 'warn' : 'bad'; }

  function render() {
    const c = calc();

    $('metrics').innerHTML = `
      <div class="m"><div class="l">Predicted FPY</div><div class="v" style="color:${c.fpy > 97 ? 'var(--green)' : c.fpy > 95 ? 'var(--amber)' : 'var(--red)'}">${c.fpy.toFixed(1)}%</div></div>
      <div class="m"><div class="l">Defect PPM</div><div class="v" style="color:${c.defectPPM < 800 ? 'var(--green)' : c.defectPPM < 1500 ? 'var(--amber)' : 'var(--red)'}">${c.defectPPM}</div></div>
      <div class="m"><div class="l">Rework Load</div><div class="v" style="color:var(--violet)">${c.reworkH.toFixed(1)}h</div></div>
      <div class="m"><div class="l">NPI Risk Index</div><div class="v" style="color:${c.risk < 35 ? 'var(--green)' : c.risk < 55 ? 'var(--amber)' : 'var(--red)'}">${c.risk}</div></div>
    `;

    $('drivers').innerHTML = c.drivers.map(d => `
      <div class="bar-row"><div class="bar-meta"><span>${d[0]}</span><span>${d[1]}%</span></div><div class="bar"><span style="width:${d[1]}%;background:linear-gradient(90deg,var(--green),var(--amber),var(--red))"></span></div></div>
    `).join('');

    const rows = [
      ['First-Pass Yield', `${c.fpy.toFixed(1)}%`, `${c.controlledFPY.toFixed(1)}%`, `${(c.controlledFPY-c.fpy).toFixed(1)} pts`],
      ['Defect PPM', `${c.defectPPM}`, `${c.controlledPPM}`, `${Math.round(((c.defectPPM-c.controlledPPM)/c.defectPPM)*100)}% lower`],
      ['Rework Hours / Lot', `${c.reworkH.toFixed(1)}h`, `${c.controlledRework.toFixed(1)}h`, `${Math.round((1-c.controlledRework/c.reworkH)*100)}% lower`]
    ];
    $('compare').innerHTML = rows.map(r => `<tr><td>${r[0]}</td><td>${r[1]}</td><td>${r[2]}</td><td>${r[3]}</td></tr>`).join('');

    $('rec').innerHTML = `
      <div class="evt">Release Status: <span class="chip ${clsRisk(c.risk)}">${c.risk < 35 ? 'GO' : c.risk < 55 ? 'GO WITH CONTROLS' : 'HOLD / REWORK DFM'}</span></div>
      <div class="evt">Suggested pilot lot size: <strong style="color:var(--text)">${c.risk < 35 ? '600' : c.risk < 55 ? '350' : '180'} units</strong></div>
      <div class="evt">Expected warranty exposure: <strong style="color:var(--text)">${(c.defectPPM/1000*1.2).toFixed(2)}%</strong></div>
    `;

    const actions = [
      ['Stencil aperture optimization + paste study', c.risk > 48],
      ['Add BGA X-ray gate at FAI + lot 1', c.drivers[1][1] > 42],
      ['Tighten AOI library threshold by package class', c.drivers[3][1] > 16],
      ['Operator recertification on fine-pitch handling', c.drivers[0][1] > 44]
    ];

    $('actions').innerHTML = actions.map(a => `<div class="evt">${a[0]} ${a[1] ? '<span class="chip warn">Required</span>' : '<span class="chip ok">Optional</span>'}</div>`).join('');
  }

  els.forEach(e => e.addEventListener('input', render));
  render();
})();
</script>
