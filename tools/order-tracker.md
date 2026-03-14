---
layout: tool
title: "Order Tracker"
tool_tagline: "Client-visible order intelligence with ETA confidence, exception routing, and live production state."
permalink: /tools/order-tracker/
---

<style>
  @import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@300;400;500;600&family=Outfit:wght@400;600;700;800&display=swap');

  .otr-demo { --bg:#0a111c; --panel:#121c2d; --card:#17253a; --border:#2c3f61; --text:#dae8fb; --dim:#8ea5c8; --cyan:#38bdf8; --green:#34d399; --amber:#f59e0b; --red:#f87171; --violet:#a78bfa; }
  .otr-demo, .otr-demo * { box-sizing:border-box; margin:0; padding:0; }
  .otr-demo { font-family:'IBM Plex Mono', monospace; background:var(--bg); color:var(--text); min-height:100vh; }

  .head { display:flex; justify-content:space-between; align-items:center; gap:12px; padding:14px 18px; border-bottom:1px solid var(--border); background:linear-gradient(180deg, rgba(56,189,248,.1), transparent); }
  .head h2 { font-family:'Outfit',sans-serif; font-size:20px; background:linear-gradient(90deg,#a5edff,#93c5fd); -webkit-background-clip:text; -webkit-text-fill-color:transparent; }
  .head p { color:var(--dim); font-size:10px; margin-top:3px; letter-spacing:.08em; text-transform:uppercase; }

  .layout { display:grid; grid-template-columns: 1fr 320px; gap:12px; padding:12px; }
  .panel { border:1px solid var(--border); border-radius:10px; background:var(--panel); }
  .in { padding:12px; }

  .metrics { display:grid; grid-template-columns: repeat(5,1fr); gap:8px; margin-bottom:10px; }
  .m { border:1px solid var(--border); border-radius:8px; background:var(--card); padding:10px; }
  .m .l { color:var(--dim); font-size:9px; text-transform:uppercase; margin-bottom:4px; }
  .m .v { font-family:'Outfit',sans-serif; font-size:22px; font-weight:800; line-height:1; }

  .toolbar { display:flex; gap:8px; flex-wrap:wrap; margin-bottom:10px; }
  .btn { border:1px solid var(--border); background:var(--card); color:var(--dim); border-radius:6px; padding:6px 10px; font-size:10px; cursor:pointer; }
  .btn.active, .btn:hover { border-color:var(--cyan); color:var(--cyan); background:rgba(56,189,248,.12); }

  .order { border:1px solid var(--border); border-radius:9px; background:var(--card); margin-bottom:9px; overflow:hidden; }
  .o-head { display:flex; justify-content:space-between; gap:10px; align-items:center; padding:10px; cursor:pointer; }
  .o-head:hover { background:rgba(56,189,248,.05); }
  .o-id { color:var(--cyan); font-size:10px; }
  .o-cust { color:var(--dim); font-size:10px; }
  .o-body { display:none; border-top:1px solid var(--border); padding:10px; }
  .order.open .o-body { display:block; }

  .step-row { display:grid; grid-template-columns: repeat(6,1fr); gap:6px; margin-bottom:8px; }
  .step { border:1px solid var(--border); border-radius:6px; padding:6px; text-align:center; font-size:9px; color:var(--dim); }
  .step.done { border-color:rgba(52,211,153,.4); color:var(--green); background:rgba(52,211,153,.08); }
  .step.now { border-color:rgba(56,189,248,.5); color:var(--cyan); background:rgba(56,189,248,.1); }

  .split { display:grid; grid-template-columns: 1fr 1fr; gap:8px; }
  .mini { border:1px solid var(--border); border-radius:6px; padding:7px; font-size:10px; background:#122036; }
  .mini strong { color:var(--text); }

  .pill { font-size:8px; border:1px solid; border-radius:999px; padding:2px 6px; }
  .ok { color:var(--green); border-color:rgba(52,211,153,.5); background:rgba(52,211,153,.12); }
  .warn { color:var(--amber); border-color:rgba(245,158,11,.5); background:rgba(245,158,11,.12); }
  .bad { color:var(--red); border-color:rgba(248,113,113,.5); background:rgba(248,113,113,.12); }

  .evt { font-size:10px; color:var(--dim); padding:7px 0; border-bottom:1px solid rgba(44,63,97,.6); }
  .evt:last-child { border-bottom:none; }

  @media (max-width: 1100px) {
    .layout { grid-template-columns:1fr; }
    .metrics { grid-template-columns:repeat(2,1fr); }
    .step-row { grid-template-columns:repeat(3,1fr); }
    .split { grid-template-columns:1fr; }
  }
</style>

<div class="otr-demo">
  <div class="head">
    <div>
      <h2>Order Tracker 2.0</h2>
      <p>Real-time order state with ETA confidence and exception intelligence</p>
    </div>
    <div class="toolbar" id="status-filters"></div>
  </div>

  <div class="layout">
    <section class="panel">
      <div class="in">
        <div class="metrics" id="metrics"></div>
        <div id="order-list"></div>
      </div>
    </section>

    <aside class="panel">
      <div class="in">
        <h3 style="color:var(--dim);font-size:10px;text-transform:uppercase;letter-spacing:.09em;margin-bottom:8px">Exception Stream</h3>
        <div id="events"></div>
      </div>
    </aside>
  </div>
</div>

<script>
(function () {
  const STATUS = ['All','On Track','At Risk','Delayed'];
  const STEPS = ['Kitting','Placement','Reflow','AOI','Test','Ship'];

  const ORDERS = [
    { id:'ORD-7821', customer:'RailSense', qty:420, status:'On Track', idx:3, eta:4, conf:91, issue:'None', line:'SMT-01' },
    { id:'ORD-7834', customer:'AquaGrid', qty:680, status:'At Risk', idx:2, eta:7, conf:68, issue:'Stencil cycle drift', line:'SMT-02' },
    { id:'ORD-7840', customer:'AgriNode', qty:1200, status:'Delayed', idx:1, eta:12, conf:43, issue:'Component shortage', line:'SMT-01' },
    { id:'ORD-7852', customer:'PortSecure', qty:300, status:'On Track', idx:4, eta:2, conf:94, issue:'None', line:'FT-01' },
    { id:'ORD-7865', customer:'HydroWatch', qty:760, status:'At Risk', idx:3, eta:6, conf:72, issue:'AOI false positive spike', line:'SMT-02' }
  ];

  let active = 'All';
  const filtersEl = document.getElementById('status-filters');
  const metricsEl = document.getElementById('metrics');
  const listEl = document.getElementById('order-list');
  const eventsEl = document.getElementById('events');

  function filtered() { return ORDERS.filter(o => active === 'All' || o.status === active); }

  function statusClass(s) { return s === 'On Track' ? 'ok' : s === 'At Risk' ? 'warn' : 'bad'; }

  function renderFilters() {
    filtersEl.innerHTML = STATUS.map(s => `<button class="btn ${s===active?'active':''}" data-s="${s}">${s}</button>`).join('');
    filtersEl.querySelectorAll('button').forEach(b => b.onclick = () => { active = b.dataset.s; render(); });
  }

  function renderMetrics(rows) {
    const onTrack = rows.filter(r=>r.status==='On Track').length;
    const atRisk = rows.filter(r=>r.status==='At Risk').length;
    const delayed = rows.filter(r=>r.status==='Delayed').length;
    const avgConf = rows.length ? Math.round(rows.reduce((s,r)=>s+r.conf,0)/rows.length) : 0;
    const units = rows.reduce((s,r)=>s+r.qty,0);

    metricsEl.innerHTML = `
      <div class="m"><div class="l">Visible Orders</div><div class="v" style="color:var(--cyan)">${rows.length}</div></div>
      <div class="m"><div class="l">On Track</div><div class="v" style="color:var(--green)">${onTrack}</div></div>
      <div class="m"><div class="l">At Risk</div><div class="v" style="color:var(--amber)">${atRisk}</div></div>
      <div class="m"><div class="l">Delayed</div><div class="v" style="color:var(--red)">${delayed}</div></div>
      <div class="m"><div class="l">ETA Confidence</div><div class="v" style="color:${avgConf>80?'var(--green)':avgConf>65?'var(--amber)':'var(--red)'}">${avgConf}%</div></div>
    `;

    eventsEl.innerHTML = `
      <div class="evt"><strong>Total Units In Flight:</strong> ${units}</div>
      ${rows.filter(r=>r.issue!=='None').map(r => `<div class="evt"><strong>${r.id}</strong> (${r.line}) -> ${r.issue}</div>`).join('') || '<div class="evt">No active exceptions in current filter.</div>'}
      <div class="evt"><strong>Client Feed Sync:</strong> Last update 2m ago</div>
    `;
  }

  function renderOrders(rows) {
    listEl.innerHTML = rows.map((o, i) => `
      <article class="order ${i===0?'open':''}">
        <div class="o-head">
          <div>
            <div class="o-id">${o.id}</div>
            <div class="o-cust">${o.customer} · ${o.qty} units · ETA ${o.eta}d</div>
          </div>
          <span class="pill ${statusClass(o.status)}">${o.status}</span>
        </div>
        <div class="o-body">
          <div class="step-row">
            ${STEPS.map((s, idx) => `<div class="step ${idx < o.idx ? 'done' : idx===o.idx ? 'now' : ''}">${s}</div>`).join('')}
          </div>
          <div class="split">
            <div class="mini"><strong>Current Line:</strong> ${o.line}<br><strong>ETA Confidence:</strong> ${o.conf}%</div>
            <div class="mini"><strong>Exception:</strong> ${o.issue}<br><strong>Client Visibility:</strong> Live</div>
          </div>
        </div>
      </article>
    `).join('');

    document.querySelectorAll('.order .o-head').forEach(h => {
      h.onclick = () => h.parentElement.classList.toggle('open');
    });
  }

  function render() {
    renderFilters();
    const rows = filtered();
    renderMetrics(rows);
    renderOrders(rows);
  }

  render();
})();
</script>
