---
layout: tool
title: "RFQ-to-Quote Engine"
tool_tagline: "Convert BOM, quantity, and service constraints into quote bands, lead-time promises, and margin envelopes."
permalink: /tools/rfq-quote-engine/
---

<style>
  @import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@300;400;500;600&family=Outfit:wght@400;600;700;800&display=swap');

  .rfq-demo {
    --bg: #0b1220;
    --panel: #121d31;
    --card: #18253b;
    --border: #2d3f60;
    --text: #d9e6f8;
    --dim: #8ca4c7;
    --cyan: #38bdf8;
    --green: #34d399;
    --amber: #f59e0b;
    --red: #f87171;
    --violet: #a78bfa;
  }
  .rfq-demo, .rfq-demo * { box-sizing: border-box; margin: 0; padding: 0; }
  .rfq-demo { font-family: 'IBM Plex Mono', monospace; background: var(--bg); color: var(--text); min-height: 100vh; }

  .rfq-head { display:flex; justify-content:space-between; align-items:center; padding:14px 18px; border-bottom:1px solid var(--border); background: linear-gradient(180deg, rgba(56,189,248,0.1), transparent); }
  .rfq-head h2 { font-family:'Outfit',sans-serif; font-size:20px; letter-spacing:-0.02em; background:linear-gradient(90deg,#a5edff,#93c5fd); -webkit-background-clip:text; -webkit-text-fill-color:transparent; }
  .rfq-head p { margin-top:3px; color: var(--dim); font-size:10px; letter-spacing:.08em; text-transform:uppercase; }

  .rfq-wrap { display:grid; grid-template-columns: 300px 1fr 320px; min-height: calc(100vh - 62px); }
  .col { border-right:1px solid var(--border); background: var(--panel); }
  .col:last-child { border-right:none; border-left:1px solid var(--border); }
  .col-main { padding:12px; overflow:auto; }
  .col-side { padding:12px; overflow:auto; }

  .blk { border:1px solid var(--border); border-radius:10px; background:var(--card); padding:10px; margin-bottom:10px; }
  .blk h3 { color:var(--dim); font-size:10px; letter-spacing:.09em; text-transform:uppercase; margin-bottom:8px; }

  .field { margin-bottom:8px; }
  .field label { display:block; font-size:9px; color:var(--dim); margin-bottom:4px; text-transform:uppercase; }
  .field input, .field select { width:100%; background:#0e1728; border:1px solid var(--border); color:var(--text); font-size:11px; border-radius:6px; padding:7px; font-family: inherit; }

  .kpi { display:grid; grid-template-columns: repeat(4,1fr); gap:9px; margin-bottom:10px; }
  .k { border:1px solid var(--border); border-radius:8px; background:var(--card); padding:10px; }
  .k .l { color:var(--dim); font-size:9px; text-transform:uppercase; margin-bottom:4px; }
  .k .v { font-family:'Outfit',sans-serif; font-size:24px; font-weight:800; line-height:1; }

  .band { margin-bottom:8px; }
  .band-row { display:flex; justify-content:space-between; font-size:10px; margin-bottom:3px; }
  .bar { height:9px; border:1px solid var(--border); border-radius:999px; overflow:hidden; background:#0b1322; }
  .bar > span { display:block; height:100%; }

  .tbl { width:100%; border-collapse: collapse; font-size:10px; }
  .tbl th, .tbl td { padding:7px; border-bottom:1px solid rgba(45,63,96,.6); text-align:left; }
  .tbl th { color:var(--dim); font-size:9px; text-transform:uppercase; background: rgba(24,37,59,.9); }
  .pill { font-size:8px; border:1px solid; border-radius:999px; padding:2px 6px; }
  .ok { color:var(--green); border-color: rgba(52,211,153,.5); background: rgba(52,211,153,.12); }
  .warn { color:var(--amber); border-color: rgba(245,158,11,.5); background: rgba(245,158,11,.12); }
  .bad { color:var(--red); border-color: rgba(248,113,113,.5); background: rgba(248,113,113,.12); }

  .evt { font-size:10px; color:var(--dim); padding:7px 0; border-bottom:1px solid rgba(45,63,96,.6); }
  .evt strong { color:var(--text); }

  @media (max-width: 1100px) {
    .rfq-wrap { grid-template-columns: 1fr; }
    .col, .col:last-child { border-right:none; border-left:none; border-bottom:1px solid var(--border); }
    .kpi { grid-template-columns: repeat(2,1fr); }
  }
</style>

<div class="rfq-demo">
  <div class="rfq-head">
    <div>
      <h2>RFQ-to-Quote Engine</h2>
      <p>Commercial decisioning over BOM volatility, service level, and supply risk</p>
    </div>
  </div>

  <div class="rfq-wrap">
    <aside class="col col-side">
      <div class="blk">
        <h3>RFQ Inputs</h3>
        <div class="field"><label>Customer</label><input id="in-customer" value="RailSense Systems"></div>
        <div class="field"><label>Build Quantity</label><input id="in-qty" type="number" value="650"></div>
        <div class="field"><label>BOM Line Items</label><input id="in-bom" type="number" value="192"></div>
        <div class="field"><label>Unique MPN Count</label><input id="in-mpn" type="number" value="141"></div>
        <div class="field"><label>Service Tier</label>
          <select id="in-tier">
            <option value="standard">Standard</option>
            <option value="priority">Priority</option>
            <option value="expedite">Expedite</option>
          </select>
        </div>
        <div class="field"><label>Cross-Border Delivery</label>
          <select id="in-border">
            <option value="no">No</option>
            <option value="yes">Yes</option>
          </select>
        </div>
      </div>

      <div class="blk">
        <h3>Risk Drivers</h3>
        <div class="field"><label>AVL Coverage (%)</label><input id="in-avl" type="number" value="74"></div>
        <div class="field"><label>Long-Lead Components</label><input id="in-long" type="number" value="9"></div>
        <div class="field"><label>Forecast Confidence (%)</label><input id="in-forecast" type="number" value="62"></div>
      </div>
    </aside>

    <main class="col-main">
      <div class="kpi" id="kpi"></div>

      <div class="blk">
        <h3>Quote Band & Margin Envelope</h3>
        <div id="bands"></div>
      </div>

      <div class="blk">
        <h3>Cost Stack Snapshot</h3>
        <table class="tbl">
          <thead><tr><th>Cost Bucket</th><th>$/Unit</th><th>% of Quote</th><th>Signal</th></tr></thead>
          <tbody id="cost-body"></tbody>
        </table>
      </div>
    </main>

    <aside class="col col-side">
      <div class="blk">
        <h3>Commitment Timeline</h3>
        <div id="timeline"></div>
      </div>
      <div class="blk">
        <h3>Approval Gates</h3>
        <div id="gates"></div>
      </div>
    </aside>
  </div>
</div>

<script>
(function () {
  const el = id => document.getElementById(id);
  const inputs = ['in-customer','in-qty','in-bom','in-mpn','in-tier','in-border','in-avl','in-long','in-forecast'].map(el);

  function clamp(v, lo, hi) { return Math.max(lo, Math.min(hi, v)); }

  function compute() {
    const qty = Number(el('in-qty').value || 0);
    const bom = Number(el('in-bom').value || 0);
    const mpn = Number(el('in-mpn').value || 0);
    const tier = el('in-tier').value;
    const border = el('in-border').value === 'yes';
    const avl = Number(el('in-avl').value || 0);
    const longLead = Number(el('in-long').value || 0);
    const fc = Number(el('in-forecast').value || 0);

    const baseMat = 24 + bom * 0.045 + mpn * 0.03;
    const labor = 7.5 + bom * 0.012;
    const test = 2.6 + (tier === 'priority' ? 0.6 : tier === 'expedite' ? 1.2 : 0.3);
    const logistics = border ? 3.4 : 1.5;

    const riskIdx = clamp(100 - avl + longLead * 2.8 + (100 - fc) * 0.35, 8, 92);
    const riskPremium = riskIdx * 0.06;
    const tierPremium = tier === 'standard' ? 0 : tier === 'priority' ? 3.5 : 7.5;
    const unitCost = baseMat + labor + test + logistics + riskPremium;

    const marginTarget = clamp(22 + (tier === 'expedite' ? 5 : tier === 'priority' ? 2 : 0) - riskIdx * 0.05, 14, 33);
    const quoteLow = unitCost / (1 - (marginTarget - 3) / 100);
    const quoteMid = unitCost / (1 - marginTarget / 100);
    const quoteHigh = unitCost / (1 - (marginTarget + 3.5) / 100);

    const leadDays = Math.round(10 + longLead * 1.4 + (tier === 'expedite' ? -3 : tier === 'priority' ? -1 : 0) + (border ? 2 : 0));
    const winProb = clamp(76 - quoteMid * 0.32 + (tier !== 'standard' ? 6 : 0), 18, 88);

    return {
      qty, riskIdx, marginTarget, unitCost, quoteLow, quoteMid, quoteHigh, leadDays, winProb,
      buckets: [
        ['Materials', baseMat],
        ['Assembly Labor', labor],
        ['Test & Validation', test],
        ['Logistics & Compliance', logistics],
        ['Risk Premium', riskPremium + tierPremium]
      ]
    };
  }

  function tone(v) { return v < 30 ? 'ok' : v < 55 ? 'warn' : 'bad'; }

  function render() {
    const c = compute();

    el('kpi').innerHTML = `
      <div class="k"><div class="l">Quote Mid ($/unit)</div><div class="v" style="color:var(--cyan)">$${c.quoteMid.toFixed(2)}</div></div>
      <div class="k"><div class="l">Margin Target</div><div class="v" style="color:var(--green)">${c.marginTarget.toFixed(1)}%</div></div>
      <div class="k"><div class="l">Promised Lead Time</div><div class="v" style="color:var(--violet)">${c.leadDays}d</div></div>
      <div class="k"><div class="l">Estimated Win Probability</div><div class="v" style="color:${c.winProb > 55 ? 'var(--green)' : 'var(--amber)'}">${c.winProb}%</div></div>
    `;

    const maxQ = c.quoteHigh * 1.15;
    el('bands').innerHTML = `
      <div class="band"><div class="band-row"><span>Low Confidence Quote</span><span>$${c.quoteLow.toFixed(2)}</span></div><div class="bar"><span style="width:${(c.quoteLow/maxQ)*100}%;background:var(--green)"></span></div></div>
      <div class="band"><div class="band-row"><span>Target Quote</span><span>$${c.quoteMid.toFixed(2)}</span></div><div class="bar"><span style="width:${(c.quoteMid/maxQ)*100}%;background:var(--cyan)"></span></div></div>
      <div class="band"><div class="band-row"><span>High Risk Quote</span><span>$${c.quoteHigh.toFixed(2)}</span></div><div class="bar"><span style="width:${(c.quoteHigh/maxQ)*100}%;background:var(--amber)"></span></div></div>
    `;

    const total = c.buckets.reduce((s, b) => s + b[1], 0);
    el('cost-body').innerHTML = c.buckets.map(([name, v]) => {
      const pct = (v / c.quoteMid) * 100;
      const signal = name === 'Risk Premium' ? (c.riskIdx > 45 ? 'warn' : 'ok') : 'ok';
      const txt = signal === 'warn' ? 'Monitor' : 'Stable';
      return `<tr><td>${name}</td><td>$${v.toFixed(2)}</td><td>${pct.toFixed(1)}%</td><td><span class="pill ${signal}">${txt}</span></td></tr>`;
    }).join('');

    el('timeline').innerHTML = `
      <div class="evt"><strong>D+0:</strong> RFQ intake + BOM sanity validation</div>
      <div class="evt"><strong>D+1:</strong> AVL and long-lead risk screening</div>
      <div class="evt"><strong>D+2:</strong> Supplier pulls + capacity reservation</div>
      <div class="evt"><strong>D+3:</strong> Quote release (${c.leadDays}d promise)</div>
      <div class="evt"><strong>D+5:</strong> PO lock and NPI kickoff</div>
    `;

    el('gates').innerHTML = `
      <div class="evt"><strong>Commercial Gate:</strong> ${c.winProb > 50 ? '<span class="pill ok">Recommended</span>' : '<span class="pill warn">Re-price</span>'}</div>
      <div class="evt"><strong>Supply Gate:</strong> <span class="pill ${tone(c.riskIdx)}">Risk ${c.riskIdx.toFixed(0)}</span></div>
      <div class="evt"><strong>Margin Gate:</strong> <span class="pill ${c.marginTarget > 20 ? 'ok' : 'warn'}">${c.marginTarget.toFixed(1)}% target</span></div>
      <div class="evt"><strong>Capacity Gate:</strong> <span class="pill ${c.leadDays <= 16 ? 'ok' : 'warn'}">${c.leadDays} day commit</span></div>
    `;
  }

  inputs.forEach(i => i.addEventListener('input', render));
  render();
})();
</script>
