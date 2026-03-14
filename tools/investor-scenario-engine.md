---
layout: tool
title: "Investor Engine"
tool_tagline: "Stress-test grants, utilization ramp, and acquisition timing against EBITDA, runway, and return profile."
permalink: /tools/investor-scenario-engine/
---

<style>
  @import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@300;400;500;600&family=Outfit:wght@400;600;700;800&display=swap');

  .inv-demo { --bg:#0a111c; --panel:#121b2c; --card:#17253a; --border:#2d3f61; --text:#dbe8fb; --dim:#90a6c8; --cyan:#38bdf8; --green:#34d399; --amber:#f59e0b; --red:#f87171; --violet:#a78bfa; }
  .inv-demo, .inv-demo * { box-sizing:border-box; margin:0; padding:0; }
  .inv-demo { font-family:'IBM Plex Mono', monospace; background:var(--bg); color:var(--text); min-height:100vh; }

  .head { display:flex; justify-content:space-between; align-items:center; padding:14px 18px; border-bottom:1px solid var(--border); background:linear-gradient(180deg, rgba(56,189,248,.1), transparent); }
  .head h2 { font-family:'Outfit',sans-serif; font-size:20px; background:linear-gradient(90deg,#a5edff,#93c5fd); -webkit-background-clip:text; -webkit-text-fill-color:transparent; }
  .head p { color:var(--dim); font-size:10px; margin-top:3px; text-transform:uppercase; letter-spacing:.08em; }

  .grid { display:grid; grid-template-columns: 320px 1fr 320px; min-height: calc(100vh - 62px); }
  .col { border-right:1px solid var(--border); background:var(--panel); padding:12px; overflow:auto; }
  .col:last-child { border-right:none; border-left:1px solid var(--border); }

  .blk { border:1px solid var(--border); border-radius:10px; background:var(--card); padding:10px; margin-bottom:10px; }
  .blk h3 { color:var(--dim); font-size:10px; text-transform:uppercase; letter-spacing:.09em; margin-bottom:8px; }

  .field { margin-bottom:8px; }
  .field label { display:block; font-size:9px; color:var(--dim); margin-bottom:4px; }
  .field input, .field select { width:100%; font-family:inherit; font-size:11px; color:var(--text); background:#0e1728; border:1px solid var(--border); border-radius:6px; padding:7px; }

  .metrics { display:grid; grid-template-columns:repeat(4,1fr); gap:8px; margin-bottom:10px; }
  .m { border:1px solid var(--border); border-radius:8px; background:var(--card); padding:10px; }
  .m .l { color:var(--dim); font-size:9px; text-transform:uppercase; margin-bottom:4px; }
  .m .v { font-family:'Outfit',sans-serif; font-size:23px; font-weight:800; line-height:1; }

  .bar-row { margin-bottom:8px; }
  .bar-meta { display:flex; justify-content:space-between; font-size:10px; margin-bottom:3px; }
  .bar { height:9px; border:1px solid var(--border); border-radius:999px; background:#0a1321; overflow:hidden; }
  .bar > span { display:block; height:100%; }

  .tbl { width:100%; border-collapse:collapse; font-size:10px; }
  .tbl th, .tbl td { padding:7px; text-align:left; border-bottom:1px solid rgba(45,63,97,.6); }
  .tbl th { font-size:9px; color:var(--dim); text-transform:uppercase; background:rgba(23,37,58,.95); }

  .evt { font-size:10px; color:var(--dim); padding:7px 0; border-bottom:1px solid rgba(45,63,97,.6); }
  .pill { font-size:8px; border:1px solid; border-radius:999px; padding:2px 6px; }
  .ok { color:var(--green); border-color:rgba(52,211,153,.5); background:rgba(52,211,153,.12); }
  .warn { color:var(--amber); border-color:rgba(245,158,11,.5); background:rgba(245,158,11,.12); }
  .bad { color:var(--red); border-color:rgba(248,113,113,.5); background:rgba(248,113,113,.12); }

  @media (max-width: 1100px) {
    .grid { grid-template-columns:1fr; }
    .col, .col:last-child { border-right:none; border-left:none; border-bottom:1px solid var(--border); }
    .metrics { grid-template-columns:repeat(2,1fr); }
  }
</style>

<div class="inv-demo">
  <div class="head">
    <div>
      <h2>Investor Scenario Engine</h2>
      <p>Capital efficiency and return sensitivity for NAC scale-up scenarios</p>
    </div>
  </div>

  <div class="grid">
    <aside class="col">
      <div class="blk">
        <h3>Capital Inputs</h3>
        <div class="field"><label>Initial Equity ($)</label><input id="equity" type="number" value="800000"></div>
        <div class="field"><label>Grant Capture ($)</label><input id="grant" type="number" value="180000"></div>
        <div class="field"><label>Monthly Burn at Launch ($)</label><input id="burn" type="number" value="62000"></div>
        <div class="field"><label>Ramp Duration (months)</label><input id="ramp" type="number" value="14"></div>
      </div>

      <div class="blk">
        <h3>Growth & M&A</h3>
        <div class="field"><label>Year-2 Revenue ($M)</label><input id="rev2" type="number" value="3.2" step="0.1"></div>
        <div class="field"><label>Year-3 Revenue ($M)</label><input id="rev3" type="number" value="5.7" step="0.1"></div>
        <div class="field"><label>EBITDA Margin Year-3 (%)</label><input id="marg3" type="number" value="18"></div>
        <div class="field"><label>Acquisition Timing</label><select id="ma"><option value="none">No Acquisition</option><option value="y2">Year 2 Bolt-On</option><option value="y3">Year 3 Bolt-On</option></select></div>
      </div>
    </aside>

    <main class="col">
      <div class="metrics" id="metrics"></div>

      <div class="blk">
        <h3>Scenario Waterfall</h3>
        <div id="waterfall"></div>
      </div>

      <div class="blk">
        <h3>3-Year Snapshot</h3>
        <table class="tbl">
          <thead><tr><th>Year</th><th>Revenue</th><th>EBITDA</th><th>Cash Position</th><th>Signal</th></tr></thead>
          <tbody id="rows"></tbody>
        </table>
      </div>
    </main>

    <aside class="col">
      <div class="blk">
        <h3>Investment Signals</h3>
        <div id="signals"></div>
      </div>
      <div class="blk">
        <h3>Return Envelope</h3>
        <div id="returns"></div>
      </div>
    </aside>
  </div>
</div>

<script>
(function () {
  const $ = id => document.getElementById(id);
  const ids = ['equity','grant','burn','ramp','rev2','rev3','marg3','ma'];
  const els = ids.map($);

  function calc() {
    const equity = Number($('equity').value || 0);
    const grant = Number($('grant').value || 0);
    const burn = Number($('burn').value || 0);
    const ramp = Number($('ramp').value || 0);
    const rev2 = Number($('rev2').value || 0) * 1_000_000;
    const rev3 = Number($('rev3').value || 0) * 1_000_000;
    const marg3 = Number($('marg3').value || 0) / 100;
    const ma = $('ma').value;

    const rev1 = rev2 * 0.42;
    const ebitda1 = rev1 * -0.16;
    const ebitda2 = rev2 * 0.08;
    const ebitda3 = rev3 * marg3;

    const maCost = ma === 'y2' ? 520_000 : ma === 'y3' ? 680_000 : 0;
    const maLift = ma === 'y2' ? 0.18 : ma === 'y3' ? 0.11 : 0;

    const adjRev3 = rev3 * (1 + maLift);
    const adjEbitda3 = ebitda3 * (1 + maLift * 0.9);

    const runwayMonths = Math.max(1, Math.round((equity + grant) / burn));

    let cash = equity + grant;
    cash += ebitda1 - maCost * (ma === 'y2' ? 0.6 : 0);
    const cashY1 = cash;
    cash += ebitda2 - maCost * (ma === 'y2' ? 0.4 : ma === 'y3' ? 0.6 : 0);
    const cashY2 = cash;
    cash += adjEbitda3 - maCost * (ma === 'y3' ? 0.4 : 0);
    const cashY3 = cash;

    const evLow = adjEbitda3 * 6.5;
    const evMid = adjEbitda3 * 8.0;
    const evHigh = adjEbitda3 * 9.5;
    const moicMid = evMid / Math.max(equity, 1);

    return {
      runwayMonths, rev1, rev2, rev3: adjRev3, e1: ebitda1, e2: ebitda2, e3: adjEbitda3,
      cashY1, cashY2, cashY3, evLow, evMid, evHigh, moicMid,
      burnCoverage: ((equity + grant) / (burn * Math.max(ramp,1))) * 100,
      ma
    };
  }

  function money(v) { return `$${(v/1_000_000).toFixed(2)}M`; }

  function render() {
    const c = calc();

    $('metrics').innerHTML = `
      <div class="m"><div class="l">Runway</div><div class="v" style="color:${c.runwayMonths>18?'var(--green)':c.runwayMonths>12?'var(--amber)':'var(--red)'}">${c.runwayMonths}m</div></div>
      <div class="m"><div class="l">Year-3 EBITDA</div><div class="v" style="color:var(--green)">${money(c.e3)}</div></div>
      <div class="m"><div class="l">Year-3 Cash</div><div class="v" style="color:${c.cashY3>0?'var(--cyan)':'var(--red)'}">${money(c.cashY3)}</div></div>
      <div class="m"><div class="l">MOIC (Mid Case)</div><div class="v" style="color:var(--violet)">${c.moicMid.toFixed(1)}x</div></div>
    `;

    const max = Math.max(c.evHigh, Math.abs(c.e1), Math.abs(c.e2), Math.abs(c.e3));
    $('waterfall').innerHTML = `
      <div class="bar-row"><div class="bar-meta"><span>Runway Coverage vs Ramp</span><span>${c.burnCoverage.toFixed(0)}%</span></div><div class="bar"><span style="width:${Math.min(100,c.burnCoverage)}%;background:${c.burnCoverage>120?'var(--green)':c.burnCoverage>90?'var(--amber)':'var(--red)'}"></span></div></div>
      <div class="bar-row"><div class="bar-meta"><span>Year-1 EBITDA</span><span>${money(c.e1)}</span></div><div class="bar"><span style="width:${Math.abs(c.e1)/max*100}%;background:var(--red)"></span></div></div>
      <div class="bar-row"><div class="bar-meta"><span>Year-2 EBITDA</span><span>${money(c.e2)}</span></div><div class="bar"><span style="width:${Math.abs(c.e2)/max*100}%;background:var(--amber)"></span></div></div>
      <div class="bar-row"><div class="bar-meta"><span>Year-3 EBITDA</span><span>${money(c.e3)}</span></div><div class="bar"><span style="width:${Math.abs(c.e3)/max*100}%;background:var(--green)"></span></div></div>
    `;

    $('rows').innerHTML = [
      ['Year 1', c.rev1, c.e1, c.cashY1],
      ['Year 2', c.rev2, c.e2, c.cashY2],
      ['Year 3', c.rev3, c.e3, c.cashY3]
    ].map(r => {
      const tone = r[3] > 0 ? 'ok' : 'bad';
      return `<tr><td>${r[0]}</td><td>${money(r[1])}</td><td>${money(r[2])}</td><td>${money(r[3])}</td><td><span class="pill ${tone}">${r[3] > 0 ? 'Sustainable' : 'Dilution Risk'}</span></td></tr>`;
    }).join('');

    $('signals').innerHTML = `
      <div class="evt"><strong>Acquisition Mode:</strong> ${c.ma === 'none' ? 'Organic only' : c.ma === 'y2' ? 'Year-2 bolt-on' : 'Year-3 bolt-on'}</div>
      <div class="evt"><strong>Cash Risk:</strong> ${c.cashY2 > 0 ? '<span class="pill ok">Controlled</span>' : '<span class="pill bad">Bridge likely required</span>'}</div>
      <div class="evt"><strong>Scale Signal:</strong> ${c.e3 > 900000 ? '<span class="pill ok">Institutional-ready trajectory</span>' : '<span class="pill warn">Needs stronger margin ramp</span>'}</div>
    `;

    $('returns').innerHTML = `
      <div class="evt"><strong>EV Low:</strong> ${money(c.evLow)}</div>
      <div class="evt"><strong>EV Mid:</strong> ${money(c.evMid)}</div>
      <div class="evt"><strong>EV High:</strong> ${money(c.evHigh)}</div>
      <div class="evt"><strong>MOIC Range:</strong> ${(c.evLow/Number($('equity').value||1)).toFixed(1)}x - ${(c.evHigh/Number($('equity').value||1)).toFixed(1)}x</div>
    `;
  }

  els.forEach(e => e.addEventListener('input', render));
  render();
})();
</script>
