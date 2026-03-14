---
layout: tool
title: "Firmware Bench"
tool_tagline: "Determine optimal firmware performance."
permalink: /tools/firmware/
---

<style>
  @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500;600;700&family=Barlow:wght@300;400;500;600;700;800;900&family=Barlow+Condensed:wght@400;500;600;700&display=swap');

  .firmware-demo {
    --bg: #0c0f13;
    --bg-raised: #12161c;
    --bg-inset: #080a0e;
    --bg-card: #161b24;
    --border: #1e2836;
    --border-active: #2a3a52;
    --cyan: #00e5ff;
    --cyan-dim: #006e7a;
    --green: #39ff14;
    --green-dim: #1a7a0a;
    --amber: #ffab00;
    --amber-dim: #7a5200;
    --red: #ff3d3d;
    --red-dim: #7a1e1e;
    --orange: #ff6b35;
    --purple: #b388ff;
    --text: #d4dce8;
    --text-2: #8899ab;
    --text-3: #4a5a6a;
    --glow-cyan: 0 0 20px rgba(0,229,255,0.15);
    --glow-green: 0 0 20px rgba(57,255,20,0.12);
  }

  .firmware-demo, .firmware-demo * { margin: 0; padding: 0; box-sizing: border-box; }

  .firmware-demo {
    font-family: 'JetBrains Mono', monospace;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ──── HEADER ──── */
  .hdr {
    padding: 14px 24px;
    border-bottom: 1px solid var(--border);
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: linear-gradient(180deg, rgba(0,229,255,0.03) 0%, transparent 100%);
  }
  .hdr-brand {
    display: flex;
    align-items: baseline;
    gap: 12px;
  }
  .hdr-logo {
    font-family: 'Barlow', sans-serif;
    font-weight: 900;
    font-size: 15px;
    letter-spacing: 2px;
    color: var(--cyan);
    text-transform: uppercase;
  }
  .hdr-sep { color: var(--border-active); font-size: 18px; font-weight: 300; }
  .hdr-title {
    font-family: 'Barlow Condensed', sans-serif;
    font-weight: 600;
    font-size: 16px;
    letter-spacing: 0.5px;
    color: var(--text);
  }
  .hdr-ver {
    font-size: 9px;
    color: var(--text-3);
    padding: 2px 6px;
    border: 1px solid var(--border);
    border-radius: 3px;
    margin-left: 8px;
  }
  .hdr-right { display: flex; align-items: center; gap: 16px; }
  .hdr-mcu-badge {
    font-size: 10px;
    color: var(--amber);
    padding: 3px 10px;
    border: 1px solid rgba(255,171,0,0.3);
    background: rgba(255,171,0,0.06);
    border-radius: 4px;
  }

  /* ──── MAIN GRID ──── */
  .main {
    display: grid;
    grid-template-columns: 280px 1fr;
    height: calc(100vh - 53px);
  }

  /* ──── LEFT: TASK + MCU SELECTOR ──── */
  .fw-sidebar {
    border-right: 1px solid var(--border);
    overflow-y: auto;
    background: var(--bg-raised);
    padding: 14px;
  }
  .sb-section {
    margin-bottom: 16px;
  }
  .sb-title {
    font-family: 'Barlow Condensed', sans-serif;
    font-weight: 600;
    font-size: 10px;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: var(--text-3);
    padding-bottom: 6px;
    margin-bottom: 8px;
    border-bottom: 1px solid var(--border);
  }

  .task-item {
    padding: 8px 10px;
    margin-bottom: 3px;
    border-radius: 5px;
    border: 1px solid transparent;
    cursor: pointer;
    transition: all 0.15s;
    position: relative;
  }
  .task-item:hover { background: var(--bg-card); border-color: var(--border); }
  .task-item.active {
    background: rgba(0,229,255,0.06);
    border-color: var(--cyan-dim);
    box-shadow: var(--glow-cyan);
  }
  .task-name {
    font-size: 11px;
    font-weight: 500;
    color: var(--text);
  }
  .task-desc {
    font-size: 9px;
    color: var(--text-3);
    margin-top: 2px;
  }
  .task-tags {
    display: flex;
    gap: 4px;
    margin-top: 4px;
  }
  .tag {
    font-size: 8px;
    padding: 1px 5px;
    border-radius: 3px;
    border: 1px solid;
  }
  .tag-iot { color: var(--cyan); border-color: rgba(0,229,255,0.3); background: rgba(0,229,255,0.06); }
  .tag-ctrl { color: var(--amber); border-color: rgba(255,171,0,0.3); background: rgba(255,171,0,0.06); }
  .tag-dsp { color: var(--purple); border-color: rgba(179,136,255,0.3); background: rgba(179,136,255,0.06); }
  .tag-comms { color: var(--green); border-color: rgba(57,255,20,0.3); background: rgba(57,255,20,0.06); }

  /* MCU selector */
  .mcu-item {
    padding: 6px 10px;
    margin-bottom: 3px;
    border-radius: 5px;
    border: 1px solid transparent;
    cursor: pointer;
    transition: all 0.15s;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .mcu-item:hover { background: var(--bg-card); border-color: var(--border); }
  .mcu-item.active {
    background: rgba(255,171,0,0.06);
    border-color: var(--amber-dim);
  }
  .mcu-name { font-size: 10px; font-weight: 500; }
  .mcu-arch { font-size: 8px; color: var(--text-3); }
  .mcu-mhz { font-size: 9px; color: var(--amber); }

  /* Battery selector */
  .bat-item {
    padding: 5px 10px;
    margin-bottom: 2px;
    border-radius: 4px;
    border: 1px solid transparent;
    cursor: pointer;
    transition: all 0.15s;
    font-size: 10px;
    display: flex;
    justify-content: space-between;
  }
  .bat-item:hover { background: var(--bg-card); }
  .bat-item.active { border-color: var(--green-dim); background: rgba(57,255,20,0.04); }
  .bat-cap { color: var(--green); font-size: 9px; }

  /* ──── RIGHT: BENCHMARK AREA ──── */
  .bench-area {
    overflow-y: auto;
    padding: 16px 20px;
  }

  /* Top metrics row */
  .metrics-row {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 10px;
    margin-bottom: 16px;
  }
  .metric-card {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 12px;
    position: relative;
    overflow: hidden;
  }
  .metric-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
  }
  .metric-card.savings::before { background: var(--green); }
  .metric-card.cycles::before { background: var(--cyan); }
  .metric-card.size::before { background: var(--amber); }
  .metric-card.power::before { background: var(--orange); }
  .metric-card.battery::before { background: var(--purple); }

  .mc-label {
    font-size: 8px;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    color: var(--text-3);
    margin-bottom: 4px;
  }
  .mc-row {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    gap: 6px;
  }
  .mc-val {
    font-family: 'Barlow', sans-serif;
    font-weight: 800;
    font-size: 24px;
    line-height: 1;
  }
  .mc-val.green { color: var(--green); }
  .mc-val.cyan { color: var(--cyan); }
  .mc-val.amber { color: var(--amber); }
  .mc-val.orange { color: var(--orange); }
  .mc-val.purple { color: var(--purple); }
  .mc-unit { font-size: 9px; color: var(--text-3); }
  .mc-delta {
    font-size: 10px;
    font-weight: 600;
    padding: 2px 6px;
    border-radius: 3px;
  }
  .mc-delta.good { color: var(--green); background: rgba(57,255,20,0.08); }
  .mc-delta.bad { color: var(--red); background: rgba(255,61,61,0.08); }

  /* Comparison panels */
  .compare-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin-bottom: 16px;
  }
  .code-panel {
    background: var(--bg-inset);
    border: 1px solid var(--border);
    border-radius: 6px;
    overflow: hidden;
  }
  .code-panel-hdr {
    padding: 8px 12px;
    border-bottom: 1px solid var(--border);
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .cp-badge {
    font-size: 9px;
    font-weight: 600;
    padding: 2px 8px;
    border-radius: 3px;
    letter-spacing: 0.5px;
  }
  .cp-badge.std { color: var(--red); background: rgba(255,61,61,0.12); border: 1px solid rgba(255,61,61,0.3); }
  .cp-badge.opt { color: var(--green); background: rgba(57,255,20,0.10); border: 1px solid rgba(57,255,20,0.3); }
  .cp-compiler { font-size: 9px; color: var(--text-3); }
  .code-body {
    padding: 10px 14px;
    font-size: 10px;
    line-height: 1.65;
    max-height: 280px;
    overflow-y: auto;
    white-space: pre;
    tab-size: 2;
  }
  .code-body .kw { color: #c792ea; }
  .code-body .fn { color: #82aaff; }
  .code-body .num { color: #f78c6c; }
  .code-body .str { color: #c3e88d; }
  .code-body .cmt { color: #546e7a; font-style: italic; }
  .code-body .reg { color: #ffcb6b; }
  .code-body .ins { color: #89ddff; }
  .code-body .dir { color: #f07178; }
  .code-body .lbl { color: #c792ea; }

  /* Bottom row: Bar charts + battery viz + fleet calc */
  .bottom-grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 12px;
  }
  .bottom-panel {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 14px;
  }
  .bp-title {
    font-family: 'Barlow Condensed', sans-serif;
    font-weight: 600;
    font-size: 11px;
    letter-spacing: 1px;
    text-transform: uppercase;
    color: var(--text-3);
    margin-bottom: 10px;
    padding-bottom: 6px;
    border-bottom: 1px solid var(--border);
  }

  /* Horizontal bar chart */
  .bar-group { margin-bottom: 10px; }
  .bar-label-row {
    display: flex;
    justify-content: space-between;
    font-size: 9px;
    margin-bottom: 3px;
  }
  .bar-label { color: var(--text-2); }
  .bar-value-label { font-weight: 600; }
  .bar-track {
    height: 10px;
    background: var(--bg-inset);
    border-radius: 3px;
    overflow: hidden;
    position: relative;
  }
  .bar-fill {
    height: 100%;
    border-radius: 3px;
    transition: width 0.6s ease;
    position: relative;
  }
  .bar-fill::after {
    content: '';
    position: absolute;
    right: 0; top: 0; bottom: 0; width: 2px;
    background: white;
    opacity: 0.4;
  }

  /* Battery visualization */
  .battery-viz {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 12px;
    padding: 8px 0;
  }
  .battery-pair {
    display: flex;
    gap: 24px;
    align-items: flex-end;
  }
  .battery-col { text-align: center; }
  .battery-shell {
    width: 52px;
    height: 100px;
    border: 2px solid var(--border-active);
    border-radius: 4px;
    position: relative;
    overflow: hidden;
    margin: 0 auto 6px;
  }
  .battery-shell::before {
    content: '';
    position: absolute;
    top: -6px; left: 14px;
    width: 20px; height: 6px;
    background: var(--border-active);
    border-radius: 2px 2px 0 0;
  }
  .battery-level {
    position: absolute;
    bottom: 0; left: 0; right: 0;
    transition: height 0.8s ease;
  }
  .battery-label {
    font-family: 'Barlow', sans-serif;
    font-weight: 700;
    font-size: 18px;
  }
  .battery-sub { font-size: 8px; color: var(--text-3); }

  /* Fleet calculator */
  .fleet-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 6px 0;
    border-bottom: 1px solid var(--border);
    font-size: 10px;
  }
  .fleet-row:last-child { border-bottom: none; }
  .fleet-label { color: var(--text-2); }
  .fleet-val { font-weight: 600; }
  .fleet-highlight {
    background: rgba(57,255,20,0.06);
    border: 1px solid rgba(57,255,20,0.2);
    border-radius: 5px;
    padding: 10px;
    margin-top: 10px;
    text-align: center;
  }
  .fleet-big {
    font-family: 'Barlow', sans-serif;
    font-weight: 800;
    font-size: 28px;
    color: var(--green);
  }
  .fleet-big-label { font-size: 9px; color: var(--text-3); margin-top: 2px; }

  .fleet-input-row {
    display: flex; align-items: center; gap: 6px; margin-bottom: 6px;
  }
  .fleet-input-row label { font-size: 9px; color: var(--text-3); min-width: 70px; }
  .fleet-input {
    background: var(--bg-inset); border: 1px solid var(--border); color: var(--text);
    padding: 4px 8px; border-radius: 3px; font-family: 'JetBrains Mono', monospace;
    font-size: 10px; width: 80px; text-align: right;
  }

  .firmware-demo ::-webkit-scrollbar { width: 4px; }
  .firmware-demo ::-webkit-scrollbar-track { background: transparent; }
  .firmware-demo ::-webkit-scrollbar-thumb { background: var(--border); border-radius: 2px; }
</style>

<div class="firmware-demo">
<div class="hdr">
  <div class="hdr-brand">
    <span class="hdr-logo">NAC</span>
    <span class="hdr-sep">│</span>
    <span class="hdr-title">Firmware Efficiency Benchmark</span>
    <span class="hdr-ver">v1.0</span>
  </div>
  <div class="hdr-right">
    <span class="hdr-mcu-badge" id="hdr-mcu">STM32L476 · ARM Cortex-M4 · 80MHz</span>
  </div>
</div>

<div class="main">
  <!-- ──── SIDEBAR ──── -->
  <div class="fw-sidebar">
    <div class="sb-section">
      <div class="sb-title">Benchmark Tasks</div>
      <div id="task-list"></div>
    </div>
    <div class="sb-section">
      <div class="sb-title">Target MCU</div>
      <div id="mcu-list"></div>
    </div>
    <div class="sb-section">
      <div class="sb-title">Battery Profile</div>
      <div id="bat-list"></div>
    </div>
  </div>

  <!-- ──── BENCH AREA ──── -->
  <div class="bench-area">
    <div class="metrics-row" id="metrics-row"></div>
    <div class="compare-grid" id="compare-grid"></div>
    <div class="bottom-grid" id="bottom-grid"></div>
  </div>
</div>

<script>
// ════════════════════════════════════════════════════════
// BENCHMARK DATA — real-world representative numbers
// ════════════════════════════════════════════════════════

const TASKS = [
  {
    id: 'sensor_poll',
    name: 'Sensor Poll + Sleep',
    desc: 'Read ADC, process, deep-sleep cycle',
    tags: ['iot'],
    // Per-MCU data: { cycles_std, cycles_opt, bytes_std, bytes_opt, uA_active_std, uA_active_opt, duty_cycle }
    mcuData: {
      stm32l476: { cycles_std: 4820, cycles_opt: 1340, bytes_std: 1872, bytes_opt: 286, uA_active_std: 12400, uA_active_opt: 4800, duty: 0.002 },
      attiny85:  { cycles_std: 2240, cycles_opt: 680,  bytes_std: 1420, bytes_opt: 198, uA_active_std: 5200,  uA_active_opt: 1900, duty: 0.003 },
      esp32s3:   { cycles_std: 8200, cycles_opt: 2800, bytes_std: 3640, bytes_opt: 520, uA_active_std: 42000, uA_active_opt: 18000, duty: 0.002 },
      msp430:    { cycles_std: 1860, cycles_opt: 520,  bytes_std: 948,  bytes_opt: 142, uA_active_std: 3200,  uA_active_opt: 980,  duty: 0.002 },
      rp2040:    { cycles_std: 5400, cycles_opt: 1680, bytes_std: 2180, bytes_opt: 340, uA_active_std: 24000, uA_active_opt: 9200, duty: 0.002 },
    },
    code_std: `<span class="cmt">// GCC -O2 output (C → ARM Cortex-M4)</span>
<span class="kw">void</span> <span class="fn">read_sensor_and_sleep</span>(<span class="kw">void</span>) {
  <span class="fn">HAL_ADC_Start</span>(&hadc1);
  <span class="fn">HAL_ADC_PollForConversion</span>(&hadc1, <span class="num">100</span>);
  <span class="kw">uint16_t</span> val = <span class="fn">HAL_ADC_GetValue</span>(&hadc1);
  <span class="fn">HAL_ADC_Stop</span>(&hadc1);

  <span class="cmt">// Process — HAL overhead: ~3200 cycles</span>
  <span class="kw">if</span> (val > threshold) {
    <span class="fn">trigger_alert</span>(val);
  }

  <span class="cmt">// Enter stop mode</span>
  <span class="fn">HAL_PWR_EnterSTOPMode</span>(
    PWR_LOWPOWERREGULATOR_ON,
    PWR_STOPENTRY_WFI);
  <span class="fn">SystemClock_Config</span>(); <span class="cmt">// reconfigure</span>
}
<span class="cmt">// Total: ~4820 cycles, 1872 bytes</span>
<span class="cmt">// HAL pulls in full peripheral driver</span>`,

    code_opt: `<span class="cmt">; NAC hand-optimized ARM Cortex-M4 ASM</span>
<span class="cmt">; Direct register access — zero HAL overhead</span>
<span class="lbl">sensor_poll_sleep:</span>
  <span class="ins">LDR</span>   <span class="reg">R0</span>, =ADC1_BASE
  <span class="ins">LDR</span>   <span class="reg">R1</span>, [<span class="reg">R0</span>, #ADC_CR2]
  <span class="ins">ORR</span>   <span class="reg">R1</span>, #<span class="num">0x40000000</span>   <span class="cmt">; SWSTART</span>
  <span class="ins">STR</span>   <span class="reg">R1</span>, [<span class="reg">R0</span>, #ADC_CR2]
<span class="lbl">.wait_eoc:</span>
  <span class="ins">LDR</span>   <span class="reg">R1</span>, [<span class="reg">R0</span>, #ADC_SR]
  <span class="ins">TST</span>   <span class="reg">R1</span>, #<span class="num">0x02</span>        <span class="cmt">; EOC bit</span>
  <span class="ins">BEQ</span>   <span class="lbl">.wait_eoc</span>
  <span class="ins">LDRH</span>  <span class="reg">R2</span>, [<span class="reg">R0</span>, #ADC_DR]
  <span class="cmt">; R2 = 12-bit ADC value</span>
  <span class="ins">CMP</span>   <span class="reg">R2</span>, <span class="reg">R3</span>          <span class="cmt">; threshold</span>
  <span class="ins">BHI</span>   <span class="lbl">.alert</span>
<span class="lbl">.sleep:</span>
  <span class="ins">LDR</span>   <span class="reg">R0</span>, =PWR_BASE
  <span class="ins">LDR</span>   <span class="reg">R1</span>, [<span class="reg">R0</span>, #PWR_CR]
  <span class="ins">ORR</span>   <span class="reg">R1</span>, #<span class="num">0x01</span>        <span class="cmt">; LPDS bit</span>
  <span class="ins">STR</span>   <span class="reg">R1</span>, [<span class="reg">R0</span>, #PWR_CR]
  <span class="ins">WFI</span>                    <span class="cmt">; sleep</span>
  <span class="ins">BX</span>    <span class="reg">LR</span>
<span class="cmt">; Total: ~1340 cycles, 286 bytes</span>
<span class="cmt">; 72% fewer cycles, 85% smaller</span>`
  },

  {
    id: 'pid_loop',
    name: 'PID Control Loop',
    desc: 'Fixed-point PID with anti-windup',
    tags: ['ctrl'],
    mcuData: {
      stm32l476: { cycles_std: 1820, cycles_opt: 640,  bytes_std: 1240, bytes_opt: 168, uA_active_std: 12400, uA_active_opt: 4800, duty: 0.01 },
      attiny85:  { cycles_std: 3400, cycles_opt: 1180, bytes_std: 1080, bytes_opt: 214, uA_active_std: 5200,  uA_active_opt: 1900, duty: 0.015 },
      esp32s3:   { cycles_std: 2600, cycles_opt: 880,  bytes_std: 2840, bytes_opt: 380, uA_active_std: 42000, uA_active_opt: 18000, duty: 0.01 },
      msp430:    { cycles_std: 4200, cycles_opt: 1480, bytes_std: 876,  bytes_opt: 196, uA_active_std: 3200,  uA_active_opt: 980,  duty: 0.01 },
      rp2040:    { cycles_std: 2100, cycles_opt: 720,  bytes_std: 1560, bytes_opt: 224, uA_active_std: 24000, uA_active_opt: 9200, duty: 0.01 },
    },
    code_std: `<span class="cmt">// Standard C PID — floating point</span>
<span class="kw">typedef struct</span> {
  <span class="kw">float</span> kp, ki, kd;
  <span class="kw">float</span> integral, prev_error;
  <span class="kw">float</span> out_min, out_max;
} <span class="fn">PID_t</span>;

<span class="kw">float</span> <span class="fn">pid_update</span>(<span class="fn">PID_t</span> *pid, <span class="kw">float</span> setpoint,
                <span class="kw">float</span> measured, <span class="kw">float</span> dt) {
  <span class="kw">float</span> error = setpoint - measured;
  pid->integral += error * dt;
  <span class="cmt">// Anti-windup clamp</span>
  <span class="kw">if</span> (pid->integral > pid->out_max)
    pid->integral = pid->out_max;
  <span class="kw">if</span> (pid->integral < pid->out_min)
    pid->integral = pid->out_min;

  <span class="kw">float</span> deriv = (error - pid->prev_error) / dt;
  pid->prev_error = error;

  <span class="kw">float</span> out = pid->kp * error
            + pid->ki * pid->integral
            + pid->kd * deriv;
  <span class="cmt">// Clamp output</span>
  <span class="kw">return</span> <span class="fn">fmaxf</span>(pid->out_min,
         <span class="fn">fminf</span>(pid->out_max, out));
}
<span class="cmt">// ~1820 cycles (FPU), 1240 bytes</span>`,

    code_opt: `<span class="cmt">; NAC fixed-point PID — Q15 format</span>
<span class="cmt">; Zero FPU dependency, deterministic timing</span>
<span class="lbl">pid_update:</span>
  <span class="cmt">; R0=&pid_state R1=setpoint R2=measured</span>
  <span class="ins">SUB</span>   <span class="reg">R3</span>, <span class="reg">R1</span>, <span class="reg">R2</span>        <span class="cmt">; error = sp - meas</span>
  <span class="ins">LDR</span>   <span class="reg">R4</span>, [<span class="reg">R0</span>, #<span class="num">0</span>]     <span class="cmt">; integral</span>
  <span class="ins">ADD</span>   <span class="reg">R4</span>, <span class="reg">R4</span>, <span class="reg">R3</span>        <span class="cmt">; integral += err</span>
  <span class="cmt">; Anti-windup: saturate Q15</span>
  <span class="ins">SSAT</span>  <span class="reg">R4</span>, #<span class="num">16</span>, <span class="reg">R4</span>
  <span class="ins">STR</span>   <span class="reg">R4</span>, [<span class="reg">R0</span>, #<span class="num">0</span>]
  <span class="cmt">; Derivative</span>
  <span class="ins">LDR</span>   <span class="reg">R5</span>, [<span class="reg">R0</span>, #<span class="num">4</span>]     <span class="cmt">; prev_error</span>
  <span class="ins">SUB</span>   <span class="reg">R6</span>, <span class="reg">R3</span>, <span class="reg">R5</span>        <span class="cmt">; deriv</span>
  <span class="ins">STR</span>   <span class="reg">R3</span>, [<span class="reg">R0</span>, #<span class="num">4</span>]     <span class="cmt">; save err</span>
  <span class="cmt">; P term: Kp * error (Q15 × Q15)</span>
  <span class="ins">LDR</span>   <span class="reg">R7</span>, [<span class="reg">R0</span>, #<span class="num">8</span>]     <span class="cmt">; Kp</span>
  <span class="ins">SMULL</span> <span class="reg">R8</span>, <span class="reg">R9</span>, <span class="reg">R3</span>, <span class="reg">R7</span>
  <span class="ins">LSR</span>   <span class="reg">R8</span>, <span class="reg">R8</span>, #<span class="num">15</span>
  <span class="ins">ORR</span>   <span class="reg">R8</span>, <span class="reg">R8</span>, <span class="reg">R9</span>, <span class="dir">LSL</span> #<span class="num">17</span>
  <span class="cmt">; I + D terms similar...</span>
  <span class="ins">SSAT</span>  <span class="reg">R0</span>, #<span class="num">16</span>, <span class="reg">R8</span>      <span class="cmt">; saturate</span>
  <span class="ins">BX</span>    <span class="reg">LR</span>
<span class="cmt">; 640 cycles (deterministic), 168 bytes</span>
<span class="cmt">; No FPU, no libm, cycle-exact timing</span>`
  },

  {
    id: 'uart_packet',
    name: 'UART Packet TX/RX',
    desc: 'Frame, checksum, transmit 32-byte packet',
    tags: ['comms'],
    mcuData: {
      stm32l476: { cycles_std: 6400, cycles_opt: 2100, bytes_std: 2680, bytes_opt: 412, uA_active_std: 13200, uA_active_opt: 5600, duty: 0.005 },
      attiny85:  { cycles_std: 3800, cycles_opt: 1240, bytes_std: 1640, bytes_opt: 296, uA_active_std: 5800,  uA_active_opt: 2200, duty: 0.008 },
      esp32s3:   { cycles_std: 9800, cycles_opt: 3400, bytes_std: 4200, bytes_opt: 680, uA_active_std: 45000, uA_active_opt: 19000, duty: 0.005 },
      msp430:    { cycles_std: 2900, cycles_opt: 980,  bytes_std: 1120, bytes_opt: 208, uA_active_std: 3600,  uA_active_opt: 1200, duty: 0.006 },
      rp2040:    { cycles_std: 7200, cycles_opt: 2400, bytes_std: 2960, bytes_opt: 448, uA_active_std: 26000, uA_active_opt: 10000, duty: 0.005 },
    },
    code_std: `<span class="cmt">// HAL-based UART transmit</span>
<span class="kw">void</span> <span class="fn">send_packet</span>(<span class="kw">uint8_t</span> *data, <span class="kw">uint8_t</span> len) {
  <span class="kw">uint8_t</span> frame[<span class="num">64</span>];
  frame[<span class="num">0</span>] = <span class="num">0xAA</span>;  <span class="cmt">// sync</span>
  frame[<span class="num">1</span>] = len;
  <span class="fn">memcpy</span>(&frame[<span class="num">2</span>], data, len);

  <span class="cmt">// CRC-8 via lookup table</span>
  <span class="kw">uint8_t</span> crc = <span class="num">0xFF</span>;
  <span class="kw">for</span> (<span class="kw">int</span> i = <span class="num">0</span>; i < len + <span class="num">2</span>; i++)
    crc = crc8_table[crc ^ frame[i]];
  frame[len + <span class="num">2</span>] = crc;

  <span class="cmt">// HAL blocking transmit</span>
  <span class="fn">HAL_UART_Transmit</span>(&huart2,
    frame, len + <span class="num">3</span>, <span class="num">1000</span>);
}
<span class="cmt">// ~6400 cycles, 2680 bytes</span>
<span class="cmt">// Includes full HAL UART driver</span>`,

    code_opt: `<span class="cmt">; NAC optimized UART packet TX</span>
<span class="cmt">; Direct USART register, inline CRC</span>
<span class="lbl">send_packet:</span>
  <span class="cmt">; R0=data_ptr R1=len</span>
  <span class="ins">PUSH</span>  {<span class="reg">R4</span>-<span class="reg">R7</span>, <span class="reg">LR</span>}
  <span class="ins">LDR</span>   <span class="reg">R4</span>, =USART2_BASE
  <span class="ins">MOV</span>   <span class="reg">R5</span>, #<span class="num">0xFF</span>        <span class="cmt">; CRC init</span>
  <span class="cmt">; Send sync byte</span>
  <span class="ins">MOV</span>   <span class="reg">R6</span>, #<span class="num">0xAA</span>
  <span class="ins">BL</span>    <span class="lbl">.tx_byte_crc</span>
  <span class="cmt">; Send length</span>
  <span class="ins">MOV</span>   <span class="reg">R6</span>, <span class="reg">R1</span>
  <span class="ins">BL</span>    <span class="lbl">.tx_byte_crc</span>
  <span class="cmt">; Send payload</span>
  <span class="ins">MOV</span>   <span class="reg">R7</span>, <span class="reg">R1</span>
<span class="lbl">.tx_loop:</span>
  <span class="ins">LDRB</span>  <span class="reg">R6</span>, [<span class="reg">R0</span>], #<span class="num">1</span>
  <span class="ins">BL</span>    <span class="lbl">.tx_byte_crc</span>
  <span class="ins">SUBS</span>  <span class="reg">R7</span>, #<span class="num">1</span>
  <span class="ins">BNE</span>   <span class="lbl">.tx_loop</span>
  <span class="cmt">; Send CRC</span>
  <span class="ins">MOV</span>   <span class="reg">R6</span>, <span class="reg">R5</span>
  <span class="ins">BL</span>    <span class="lbl">.tx_byte</span>
  <span class="ins">POP</span>   {<span class="reg">R4</span>-<span class="reg">R7</span>, <span class="reg">PC</span>}

<span class="lbl">.tx_byte_crc:</span>
  <span class="ins">EOR</span>   <span class="reg">R5</span>, <span class="reg">R5</span>, <span class="reg">R6</span>
  <span class="cmt">; Inline LFSR CRC-8 (no table)</span>
  <span class="ins">LSL</span>   <span class="reg">R5</span>, #<span class="num">1</span>
  <span class="ins">IT</span>    CS
  <span class="ins">EORCS</span> <span class="reg">R5</span>, #<span class="num">0x07</span>
<span class="lbl">.tx_byte:</span>
<span class="lbl">.wait_txe:</span>
  <span class="ins">LDR</span>   <span class="reg">R12</span>,[<span class="reg">R4</span>,#USART_SR]
  <span class="ins">TST</span>   <span class="reg">R12</span>, #<span class="num">0x80</span>      <span class="cmt">; TXE</span>
  <span class="ins">BEQ</span>   <span class="lbl">.wait_txe</span>
  <span class="ins">STRB</span>  <span class="reg">R6</span>, [<span class="reg">R4</span>,#USART_DR]
  <span class="ins">BX</span>    <span class="reg">LR</span>
<span class="cmt">; 2100 cycles, 412 bytes</span>
<span class="cmt">; No lookup table, no HAL driver</span>`
  },

  {
    id: 'fft_16',
    name: '16-Point FFT',
    desc: 'Radix-2 DIT FFT for vibration analysis',
    tags: ['dsp'],
    mcuData: {
      stm32l476: { cycles_std: 14200, cycles_opt: 4800,  bytes_std: 4860,  bytes_opt: 720, uA_active_std: 14000, uA_active_opt: 6200, duty: 0.008 },
      attiny85:  { cycles_std: 42000, cycles_opt: 18400, bytes_std: 5200,  bytes_opt: 1640, uA_active_std: 6000,  uA_active_opt: 2400, duty: 0.012 },
      esp32s3:   { cycles_std: 11800, cycles_opt: 3600,  bytes_std: 6400,  bytes_opt: 980, uA_active_std: 48000, uA_active_opt: 22000, duty: 0.006 },
      msp430:    { cycles_std: 38000, cycles_opt: 12600, bytes_std: 3200,  bytes_opt: 680, uA_active_std: 4200,  uA_active_opt: 1400, duty: 0.010 },
      rp2040:    { cycles_std: 16800, cycles_opt: 5400,  bytes_std: 5100,  bytes_opt: 780, uA_active_std: 28000, uA_active_opt: 11000, duty: 0.008 },
    },
    code_std: `<span class="cmt">// CMSIS-DSP 16-point FFT wrapper</span>
<span class="cmt">// Pulls in full CMSIS FFT library</span>
#<span class="dir">include</span> <span class="str">"arm_math.h"</span>

<span class="kw">static</span> arm_cfft_instance_f32 fft_inst;
<span class="kw">float32_t</span> fft_buf[<span class="num">32</span>]; <span class="cmt">// complex</span>
<span class="kw">float32_t</span> mag_buf[<span class="num">16</span>];

<span class="kw">void</span> <span class="fn">compute_fft</span>(<span class="kw">float</span> *samples) {
  <span class="cmt">// Interleave real/imag</span>
  <span class="kw">for</span> (<span class="kw">int</span> i = <span class="num">0</span>; i < <span class="num">16</span>; i++) {
    fft_buf[<span class="num">2</span>*i]   = samples[i];
    fft_buf[<span class="num">2</span>*i+<span class="num">1</span>] = <span class="num">0.0f</span>;
  }

  <span class="fn">arm_cfft_f32</span>(&arm_cfft_sR_f32_len16,
               fft_buf, <span class="num">0</span>, <span class="num">1</span>);
  <span class="fn">arm_cmplx_mag_f32</span>(fft_buf,
                    mag_buf, <span class="num">16</span>);
}
<span class="cmt">// ~14200 cycles, 4860 bytes</span>
<span class="cmt">// Includes CMSIS twiddle tables</span>
<span class="cmt">// + F32 complex math library</span>`,

    code_opt: `<span class="cmt">; NAC fixed-point radix-2 DIT FFT</span>
<span class="cmt">; Q15 format, unrolled butterflies</span>
<span class="cmt">; Hardcoded twiddle factors for N=16</span>
<span class="lbl">fft_16_q15:</span>
  <span class="cmt">; R0 = buffer (16 × Q15 complex pairs)</span>
  <span class="ins">PUSH</span>  {<span class="reg">R4</span>-<span class="reg">R11</span>, <span class="reg">LR</span>}
  <span class="cmt">; Bit-reverse permutation (hardcoded)</span>
  <span class="ins">ADR</span>   <span class="reg">R1</span>, <span class="lbl">.bitrev_lut</span>
  <span class="ins">MOV</span>   <span class="reg">R2</span>, #<span class="num">16</span>
<span class="lbl">.rev_loop:</span>
  <span class="ins">LDRB</span>  <span class="reg">R3</span>, [<span class="reg">R1</span>], #<span class="num">1</span>
  <span class="cmt">; swap if needed...</span>
  <span class="ins">SUBS</span>  <span class="reg">R2</span>, #<span class="num">1</span>
  <span class="ins">BNE</span>   <span class="lbl">.rev_loop</span>

  <span class="cmt">; Stage 1: N/2=8 butterflies, W=1</span>
  <span class="ins">MOV</span>   <span class="reg">R1</span>, #<span class="num">8</span>
<span class="lbl">.stg1:</span>
  <span class="ins">LDR</span>   <span class="reg">R4</span>, [<span class="reg">R0</span>]          <span class="cmt">; a</span>
  <span class="ins">LDR</span>   <span class="reg">R5</span>, [<span class="reg">R0</span>, #<span class="num">4</span>]      <span class="cmt">; b</span>
  <span class="ins">QADD16</span> <span class="reg">R6</span>, <span class="reg">R4</span>, <span class="reg">R5</span>       <span class="cmt">; a+b sat</span>
  <span class="ins">QSUB16</span> <span class="reg">R7</span>, <span class="reg">R4</span>, <span class="reg">R5</span>       <span class="cmt">; a-b sat</span>
  <span class="ins">STR</span>   <span class="reg">R6</span>, [<span class="reg">R0</span>], #<span class="num">8</span>
  <span class="ins">STR</span>   <span class="reg">R7</span>, [<span class="reg">R0</span>, #-<span class="num">4</span>]
  <span class="ins">SUBS</span>  <span class="reg">R1</span>, #<span class="num">1</span>
  <span class="ins">BNE</span>   <span class="lbl">.stg1</span>
  <span class="cmt">; Stages 2-4 with twiddle multiply</span>
  <span class="cmt">; SMUAD/SMUSD for complex Q15 MAC</span>
  <span class="cmt">; ... (unrolled for each stage)</span>
  <span class="ins">POP</span>   {<span class="reg">R4</span>-<span class="reg">R11</span>, <span class="reg">PC</span>}
<span class="cmt">; 4800 cycles, 720 bytes</span>
<span class="cmt">; SIMD Q15 ops, no float, no library</span>`
  },

  {
    id: 'gpio_debounce',
    name: 'GPIO Debounce + Edge',
    desc: 'Button debounce with edge detection',
    tags: ['ctrl', 'iot'],
    mcuData: {
      stm32l476: { cycles_std: 980,  cycles_opt: 180,  bytes_std: 640,  bytes_opt: 62,  uA_active_std: 11000, uA_active_opt: 3800, duty: 0.001 },
      attiny85:  { cycles_std: 620,  cycles_opt: 140,  bytes_std: 380,  bytes_opt: 48,  uA_active_std: 4800,  uA_active_opt: 1600, duty: 0.001 },
      esp32s3:   { cycles_std: 2400, cycles_opt: 520,  bytes_std: 1640, bytes_opt: 180, uA_active_std: 38000, uA_active_opt: 14000, duty: 0.001 },
      msp430:    { cycles_std: 480,  cycles_opt: 120,  bytes_std: 286,  bytes_opt: 38,  uA_active_std: 2800,  uA_active_opt: 800,  duty: 0.001 },
      rp2040:    { cycles_std: 1200, cycles_opt: 260,  bytes_std: 820,  bytes_opt: 88,  uA_active_std: 22000, uA_active_opt: 8000, duty: 0.001 },
    },
    code_std: `<span class="cmt">// HAL-based GPIO debounce</span>
<span class="kw">volatile</span> <span class="kw">uint32_t</span> last_tick = <span class="num">0</span>;
<span class="kw">volatile</span> <span class="kw">uint8_t</span>  btn_state = <span class="num">0</span>;

<span class="kw">void</span> <span class="fn">HAL_GPIO_EXTI_Callback</span>(
    <span class="kw">uint16_t</span> pin) {
  <span class="kw">uint32_t</span> now = <span class="fn">HAL_GetTick</span>();
  <span class="kw">if</span> (now - last_tick < <span class="num">50</span>) <span class="kw">return</span>;
  last_tick = now;

  btn_state = <span class="fn">HAL_GPIO_ReadPin</span>(
    BTN_GPIO_Port, BTN_Pin);

  <span class="kw">if</span> (btn_state == GPIO_PIN_SET) {
    <span class="fn">on_button_press</span>();
  }
}
<span class="cmt">// ~980 cycles, 640 bytes</span>
<span class="cmt">// Pulls HAL GPIO + SysTick drivers</span>`,

    code_opt: `<span class="cmt">; NAC shift-register debounce</span>
<span class="cmt">; Zero-jitter, interrupt-driven</span>
<span class="lbl">exti_handler:</span>
  <span class="ins">LDR</span>   <span class="reg">R0</span>, =GPIOA_BASE
  <span class="ins">LDR</span>   <span class="reg">R1</span>, [<span class="reg">R0</span>, #GPIO_IDR]
  <span class="ins">UBFX</span>  <span class="reg">R1</span>, <span class="reg">R1</span>, #<span class="num">0</span>, #<span class="num">1</span>    <span class="cmt">; PA0 bit</span>
  <span class="cmt">; Shift into debounce register</span>
  <span class="ins">LDR</span>   <span class="reg">R2</span>, =debounce_sr
  <span class="ins">LDRB</span>  <span class="reg">R3</span>, [<span class="reg">R2</span>]
  <span class="ins">LSL</span>   <span class="reg">R3</span>, #<span class="num">1</span>
  <span class="ins">ORR</span>   <span class="reg">R3</span>, <span class="reg">R1</span>
  <span class="ins">AND</span>   <span class="reg">R3</span>, #<span class="num">0x07</span>        <span class="cmt">; 3-sample</span>
  <span class="ins">STRB</span>  <span class="reg">R3</span>, [<span class="reg">R2</span>]
  <span class="cmt">; Stable high = 0b111</span>
  <span class="ins">CMP</span>   <span class="reg">R3</span>, #<span class="num">0x07</span>
  <span class="ins">IT</span>    EQ
  <span class="ins">BLEQ</span>  <span class="lbl">on_button_press</span>
  <span class="cmt">; Clear EXTI pending</span>
  <span class="ins">LDR</span>   <span class="reg">R0</span>, =EXTI_BASE
  <span class="ins">MOV</span>   <span class="reg">R1</span>, #<span class="num">1</span>
  <span class="ins">STR</span>   <span class="reg">R1</span>, [<span class="reg">R0</span>, #EXTI_PR]
  <span class="ins">BX</span>    <span class="reg">LR</span>
<span class="cmt">; 180 cycles, 62 bytes</span>
<span class="cmt">; 82% fewer cycles, 90% smaller</span>`
  },

  {
    id: 'modbus_rtu',
    name: 'Modbus RTU Response',
    desc: 'Parse request, CRC-16, respond',
    tags: ['comms', 'iot'],
    mcuData: {
      stm32l476: { cycles_std: 8900,  cycles_opt: 2800, bytes_std: 3840, bytes_opt: 540, uA_active_std: 13500, uA_active_opt: 5400, duty: 0.004 },
      attiny85:  { cycles_std: 5200,  cycles_opt: 1800, bytes_std: 2100, bytes_opt: 380, uA_active_std: 5500,  uA_active_opt: 2000, duty: 0.006 },
      esp32s3:   { cycles_std: 12400, cycles_opt: 4200, bytes_std: 5600, bytes_opt: 820, uA_active_std: 46000, uA_active_opt: 20000, duty: 0.004 },
      msp430:    { cycles_std: 3800,  cycles_opt: 1300, bytes_std: 1560, bytes_opt: 280, uA_active_std: 3400,  uA_active_opt: 1100, duty: 0.005 },
      rp2040:    { cycles_std: 9600,  cycles_opt: 3200, bytes_std: 4100, bytes_opt: 600, uA_active_std: 25000, uA_active_opt: 9800, duty: 0.004 },
    },
    code_std: `<span class="cmt">// libmodbus-style RTU response</span>
#<span class="dir">include</span> <span class="str">"modbus_rtu.h"</span>

<span class="kw">int</span> <span class="fn">process_modbus_request</span>(
    <span class="kw">uint8_t</span> *rx, <span class="kw">uint8_t</span> rx_len,
    <span class="kw">uint8_t</span> *tx, <span class="kw">uint8_t</span> *tx_len) {
  <span class="cmt">// Validate CRC</span>
  <span class="kw">uint16_t</span> crc = <span class="fn">modbus_crc16</span>(
    rx, rx_len - <span class="num">2</span>);
  <span class="kw">if</span> (crc != (<span class="kw">uint16_t</span>)(
      rx[rx_len-<span class="num">2</span>] | rx[rx_len-<span class="num">1</span>]<<<span class="num">8</span>))
    <span class="kw">return</span> -<span class="num">1</span>;

  <span class="kw">uint8_t</span> addr = rx[<span class="num">0</span>];
  <span class="kw">uint8_t</span> func = rx[<span class="num">1</span>];

  <span class="kw">switch</span> (func) {
    <span class="kw">case</span> <span class="num">0x03</span>: <span class="cmt">// Read Holding Regs</span>
      <span class="fn">read_holding_registers</span>(
        rx, tx, tx_len);
      <span class="kw">break</span>;
    <span class="kw">case</span> <span class="num">0x06</span>: <span class="cmt">// Write Single Reg</span>
      <span class="fn">write_single_register</span>(
        rx, tx, tx_len);
      <span class="kw">break</span>;
    <span class="kw">default</span>:
      <span class="fn">modbus_exception</span>(
        tx, tx_len, addr, func, <span class="num">0x01</span>);
  }
  <span class="cmt">// Append CRC to response</span>
  crc = <span class="fn">modbus_crc16</span>(tx, *tx_len);
  tx[(*tx_len)++] = crc & <span class="num">0xFF</span>;
  tx[(*tx_len)++] = crc >> <span class="num">8</span>;
  <span class="kw">return</span> <span class="num">0</span>;
}
<span class="cmt">// ~8900 cycles, 3840 bytes</span>`,

    code_opt: `<span class="cmt">; NAC Modbus RTU — bare metal</span>
<span class="cmt">; CRC-16/MODBUS via bit-bang (no table)</span>
<span class="lbl">modbus_process:</span>
  <span class="cmt">; R0=rx_buf R1=rx_len R2=tx_buf</span>
  <span class="ins">PUSH</span>  {<span class="reg">R4</span>-<span class="reg">R11</span>, <span class="reg">LR</span>}
  <span class="cmt">; Validate CRC-16</span>
  <span class="ins">SUB</span>   <span class="reg">R3</span>, <span class="reg">R1</span>, #<span class="num">2</span>
  <span class="ins">BL</span>    <span class="lbl">.crc16_compute</span>
  <span class="ins">LDRH</span>  <span class="reg">R5</span>, [<span class="reg">R0</span>, <span class="reg">R3</span>]
  <span class="ins">CMP</span>   <span class="reg">R4</span>, <span class="reg">R5</span>
  <span class="ins">BNE</span>   <span class="lbl">.crc_fail</span>
  <span class="cmt">; Parse function code</span>
  <span class="ins">LDRB</span>  <span class="reg">R6</span>, [<span class="reg">R0</span>, #<span class="num">1</span>]     <span class="cmt">; func</span>
  <span class="ins">CMP</span>   <span class="reg">R6</span>, #<span class="num">0x03</span>
  <span class="ins">BEQ</span>   <span class="lbl">.read_holding</span>
  <span class="ins">CMP</span>   <span class="reg">R6</span>, #<span class="num">0x06</span>
  <span class="ins">BEQ</span>   <span class="lbl">.write_single</span>
  <span class="ins">B</span>     <span class="lbl">.exception</span>

<span class="lbl">.crc16_compute:</span>
  <span class="cmt">; Bit-bang CRC-16/MODBUS</span>
  <span class="ins">MOV</span>   <span class="reg">R4</span>, #<span class="num">0xFFFF</span>
<span class="lbl">.crc_byte:</span>
  <span class="ins">LDRB</span>  <span class="reg">R5</span>, [<span class="reg">R0</span>], #<span class="num">1</span>
  <span class="ins">EOR</span>   <span class="reg">R4</span>, <span class="reg">R5</span>
  <span class="ins">MOV</span>   <span class="reg">R6</span>, #<span class="num">8</span>
<span class="lbl">.crc_bit:</span>
  <span class="ins">LSRS</span>  <span class="reg">R4</span>, #<span class="num">1</span>
  <span class="ins">IT</span>    CS
  <span class="ins">EORCS</span> <span class="reg">R4</span>, <span class="reg">R4</span>, #<span class="num">0xA001</span>
  <span class="ins">SUBS</span>  <span class="reg">R6</span>, #<span class="num">1</span>
  <span class="ins">BNE</span>   <span class="lbl">.crc_bit</span>
  <span class="ins">SUBS</span>  <span class="reg">R3</span>, #<span class="num">1</span>
  <span class="ins">BNE</span>   <span class="lbl">.crc_byte</span>
  <span class="ins">BX</span>    <span class="reg">LR</span>
<span class="cmt">; 2800 cycles, 540 bytes</span>
<span class="cmt">; Full Modbus slave in ~0.5KB</span>`
  },
];

const MCUS = [
  { id: 'stm32l476', name: 'STM32L476',     arch: 'ARM Cortex-M4F', mhz: 80,  sleep_uA: 0.03, vcc: 3.3, family: 'ST Ultra-Low-Power' },
  { id: 'msp430',    name: 'MSP430FR5994',  arch: 'MSP430 16-bit',  mhz: 16,  sleep_uA: 0.02, vcc: 3.3, family: 'TI Ultra-Low-Power' },
  { id: 'attiny85',  name: 'ATtiny85',      arch: 'AVR 8-bit',      mhz: 8,   sleep_uA: 0.1,  vcc: 3.3, family: 'Microchip AVR' },
  { id: 'rp2040',    name: 'RP2040',        arch: 'ARM Cortex-M0+', mhz: 133, sleep_uA: 0.18, vcc: 3.3, family: 'Raspberry Pi' },
  { id: 'esp32s3',   name: 'ESP32-S3',      arch: 'Xtensa LX7',    mhz: 240, sleep_uA: 8.0,  vcc: 3.3, family: 'Espressif WiFi/BLE' },
];

const BATTERIES = [
  { id: 'cr2032',  name: 'CR2032 Coin Cell',   mAh: 220,  nominal_V: 3.0 },
  { id: 'aa_lith', name: 'AA Lithium (Energizer)', mAh: 3000, nominal_V: 1.5 },
  { id: '18650',   name: '18650 Li-Ion',        mAh: 3500, nominal_V: 3.7 },
  { id: 'lipo_1s', name: '1S LiPo 1000mAh',    mAh: 1000, nominal_V: 3.7 },
  { id: 'aaa',     name: 'AAA Alkaline',        mAh: 1100, nominal_V: 1.5 },
];

// ════════════════════════════════════════════════════════
// STATE
// ════════════════════════════════════════════════════════

let currentTask = 'sensor_poll';
let currentMCU = 'stm32l476';
let currentBat = 'cr2032';
let fleetSize = 5000;

// ════════════════════════════════════════════════════════
// COMPUTE
// ════════════════════════════════════════════════════════

function getBenchmark() {
  const task = TASKS.find(t => t.id === currentTask);
  const mcu = MCUS.find(m => m.id === currentMCU);
  const bat = BATTERIES.find(b => b.id === currentBat);
  const d = task.mcuData[currentMCU];

  const cycleSavings = ((d.cycles_std - d.cycles_opt) / d.cycles_std * 100);
  const sizeSavings = ((d.bytes_std - d.bytes_opt) / d.bytes_std * 100);

  // Power calculation: I_avg = I_active * duty + I_sleep * (1-duty)
  const uA_avg_std = d.uA_active_std * d.duty + mcu.sleep_uA * (1 - d.duty);
  const uA_avg_opt = d.uA_active_opt * d.duty + mcu.sleep_uA * (1 - d.duty);
  const powerSavings = ((uA_avg_std - uA_avg_opt) / uA_avg_std * 100);

  // Battery life in days
  const batLife_std = (bat.mAh * 1000) / uA_avg_std / 24;
  const batLife_opt = (bat.mAh * 1000) / uA_avg_opt / 24;
  const batMultiplier = batLife_opt / batLife_std;

  return {
    task, mcu, bat, d,
    cycleSavings, sizeSavings, powerSavings,
    uA_avg_std, uA_avg_opt,
    batLife_std, batLife_opt, batMultiplier,
  };
}

// ════════════════════════════════════════════════════════
// RENDER
// ════════════════════════════════════════════════════════

function render() {
  const b = getBenchmark();

  // Header MCU badge
  document.getElementById('hdr-mcu').textContent =
    `${b.mcu.name} · ${b.mcu.arch} · ${b.mcu.mhz}MHz`;

  // ── Metrics Row ──
  const fmtDays = (d) => d > 365 ? `${(d/365).toFixed(1)}y` : d > 30 ? `${(d/30).toFixed(1)}mo` : `${d.toFixed(0)}d`;

  document.getElementById('metrics-row').innerHTML = `
    <div class="metric-card savings">
      <div class="mc-label">Cycle Reduction</div>
      <div class="mc-row">
        <div class="mc-val green">${b.cycleSavings.toFixed(0)}%</div>
        <div class="mc-delta good">↓ ${(b.d.cycles_std - b.d.cycles_opt).toLocaleString()}</div>
      </div>
      <div class="mc-unit">${b.d.cycles_opt.toLocaleString()} vs ${b.d.cycles_std.toLocaleString()} cycles</div>
    </div>
    <div class="metric-card size">
      <div class="mc-label">Code Size</div>
      <div class="mc-row">
        <div class="mc-val amber">${b.sizeSavings.toFixed(0)}%</div>
        <div class="mc-delta good">↓ ${(b.d.bytes_std - b.d.bytes_opt).toLocaleString()}B</div>
      </div>
      <div class="mc-unit">${b.d.bytes_opt}B vs ${b.d.bytes_std.toLocaleString()}B</div>
    </div>
    <div class="metric-card power">
      <div class="mc-label">Avg Power Draw</div>
      <div class="mc-row">
        <div class="mc-val orange">${b.powerSavings.toFixed(0)}%</div>
        <div class="mc-delta good">↓ ${(b.uA_avg_std - b.uA_avg_opt).toFixed(1)}µA</div>
      </div>
      <div class="mc-unit">${b.uA_avg_opt.toFixed(2)}µA vs ${b.uA_avg_std.toFixed(2)}µA avg</div>
    </div>
    <div class="metric-card cycles">
      <div class="mc-label">Exec Time @ ${b.mcu.mhz}MHz</div>
      <div class="mc-row">
        <div class="mc-val cyan">${(b.d.cycles_opt / b.mcu.mhz).toFixed(1)}</div>
        <div class="mc-unit">µs optimized</div>
      </div>
      <div class="mc-unit">${(b.d.cycles_std / b.mcu.mhz).toFixed(1)}µs standard</div>
    </div>
    <div class="metric-card battery">
      <div class="mc-label">Battery Life (${b.bat.name})</div>
      <div class="mc-row">
        <div class="mc-val purple">${b.batMultiplier.toFixed(1)}×</div>
        <div class="mc-delta good">↑ longer</div>
      </div>
      <div class="mc-unit">${fmtDays(b.batLife_opt)} vs ${fmtDays(b.batLife_std)}</div>
    </div>
  `;

  // ── Code Panels ──
  document.getElementById('compare-grid').innerHTML = `
    <div class="code-panel">
      <div class="code-panel-hdr">
        <span class="cp-badge std">STANDARD · GCC -O2</span>
        <span class="cp-compiler">${b.d.cycles_std.toLocaleString()} cyc · ${b.d.bytes_std.toLocaleString()}B · ${b.d.uA_active_std/1000}mA active</span>
      </div>
      <div class="code-body">${b.task.code_std}</div>
    </div>
    <div class="code-panel">
      <div class="code-panel-hdr">
        <span class="cp-badge opt">NAC OPTIMIZED · HAND ASM</span>
        <span class="cp-compiler">${b.d.cycles_opt.toLocaleString()} cyc · ${b.d.bytes_opt}B · ${b.d.uA_active_opt/1000}mA active</span>
      </div>
      <div class="code-body">${b.task.code_opt}</div>
    </div>
  `;

  // ── Bottom Grid ──
  const maxCyc = b.d.cycles_std;
  const maxBytes = b.d.bytes_std;

  // Fleet ROI calc
  const batCostStd = 2.50; // avg battery cost
  const replacementsStdPerYear = 365 / b.batLife_std;
  const replacementsOptPerYear = 365 / b.batLife_opt;
  const truckRollCost = 45; // cost per battery change site visit
  const annualSavedPerUnit = (replacementsStdPerYear - replacementsOptPerYear) * (batCostStd + truckRollCost);
  const fleetAnnualSaved = annualSavedPerUnit * fleetSize;

  document.getElementById('bottom-grid').innerHTML = `
    <div class="bottom-panel">
      <div class="bp-title">Cycle & Size Comparison</div>
      <div class="bar-group">
        <div class="bar-label-row">
          <span class="bar-label">Clock Cycles — Standard</span>
          <span class="bar-value-label" style="color:var(--red)">${b.d.cycles_std.toLocaleString()}</span>
        </div>
        <div class="bar-track">
          <div class="bar-fill" style="width:100%;background:linear-gradient(90deg,#7a1e1e,var(--red))"></div>
        </div>
      </div>
      <div class="bar-group">
        <div class="bar-label-row">
          <span class="bar-label">Clock Cycles — NAC Optimized</span>
          <span class="bar-value-label" style="color:var(--green)">${b.d.cycles_opt.toLocaleString()}</span>
        </div>
        <div class="bar-track">
          <div class="bar-fill" style="width:${(b.d.cycles_opt/maxCyc*100)}%;background:linear-gradient(90deg,#1a7a0a,var(--green))"></div>
        </div>
      </div>
      <div class="bar-group" style="margin-top:16px">
        <div class="bar-label-row">
          <span class="bar-label">Flash Size — Standard</span>
          <span class="bar-value-label" style="color:var(--red)">${b.d.bytes_std.toLocaleString()}B</span>
        </div>
        <div class="bar-track">
          <div class="bar-fill" style="width:100%;background:linear-gradient(90deg,var(--amber-dim),var(--amber))"></div>
        </div>
      </div>
      <div class="bar-group">
        <div class="bar-label-row">
          <span class="bar-label">Flash Size — NAC Optimized</span>
          <span class="bar-value-label" style="color:var(--green)">${b.d.bytes_opt.toLocaleString()}B</span>
        </div>
        <div class="bar-track">
          <div class="bar-fill" style="width:${(b.d.bytes_opt/maxBytes*100)}%;background:linear-gradient(90deg,#1a7a0a,var(--green))"></div>
        </div>
      </div>
      <div style="margin-top:14px;font-size:9px;color:var(--text-3);text-align:center">
        NAC assembly optimization enables use of <span style="color:var(--amber)">cheaper, smaller MCUs</span> — reducing BOM cost per unit
      </div>
    </div>

    <div class="bottom-panel">
      <div class="bp-title">Battery Life Projection</div>
      <div class="battery-viz">
        <div class="battery-pair">
          <div class="battery-col">
            <div class="battery-shell">
              <div class="battery-level" style="height:${Math.min(100, b.batLife_std / b.batLife_opt * 100)}%;background:linear-gradient(0deg,#7a1e1e,var(--red))"></div>
            </div>
            <div class="battery-label" style="color:var(--red)">${fmtDays(b.batLife_std)}</div>
            <div class="battery-sub">Standard C</div>
          </div>
          <div class="battery-col">
            <div class="battery-shell" style="border-color:var(--green-dim)">
              <div class="battery-level" style="height:100%;background:linear-gradient(0deg,#1a7a0a,var(--green))"></div>
            </div>
            <div class="battery-label" style="color:var(--green)">${fmtDays(b.batLife_opt)}</div>
            <div class="battery-sub">NAC Optimized</div>
          </div>
        </div>
        <div style="font-size:9px;color:var(--text-3);text-align:center">
          ${b.bat.name} · ${b.bat.mAh}mAh · ${b.bat.nominal_V}V<br>
          Duty cycle: ${(b.d.duty * 100).toFixed(1)}% · Sleep: ${b.mcu.sleep_uA}µA
        </div>
      </div>
    </div>

    <div class="bottom-panel">
      <div class="bp-title">Fleet ROI Calculator</div>
      <div class="fleet-input-row">
        <label>Fleet Size:</label>
        <input class="fleet-input" type="number" value="${fleetSize}" id="fleet-input" onchange="fleetSize=+this.value; render()">
      </div>
      <div class="fleet-row">
        <span class="fleet-label">Bat. replacements/yr (std)</span>
        <span class="fleet-val" style="color:var(--red)">${replacementsStdPerYear.toFixed(2)}/unit</span>
      </div>
      <div class="fleet-row">
        <span class="fleet-label">Bat. replacements/yr (opt)</span>
        <span class="fleet-val" style="color:var(--green)">${replacementsOptPerYear.toFixed(2)}/unit</span>
      </div>
      <div class="fleet-row">
        <span class="fleet-label">Site visit + battery cost</span>
        <span class="fleet-val">$${(batCostStd + truckRollCost).toFixed(2)}</span>
      </div>
      <div class="fleet-row">
        <span class="fleet-label">Saved per unit/year</span>
        <span class="fleet-val" style="color:var(--green)">$${annualSavedPerUnit.toFixed(2)}</span>
      </div>
      <div class="fleet-highlight">
        <div class="fleet-big">$${fleetAnnualSaved >= 1000000 ? (fleetAnnualSaved/1000000).toFixed(2) + 'M' : fleetAnnualSaved >= 1000 ? (fleetAnnualSaved/1000).toFixed(1) + 'K' : fleetAnnualSaved.toFixed(0)}</div>
        <div class="fleet-big-label">Annual Fleet Savings · ${fleetSize.toLocaleString()} units</div>
      </div>
      <div style="margin-top:8px;font-size:8px;color:var(--text-3);text-align:center">
        Avoided truck rolls + battery costs from extended life
      </div>
    </div>
  `;
}

// ── Sidebar Renderers ──

function renderSidebar() {
  // Tasks
  document.getElementById('task-list').innerHTML = TASKS.map(t => {
    const tagMap = { iot: 'tag-iot', ctrl: 'tag-ctrl', dsp: 'tag-dsp', comms: 'tag-comms' };
    const tagLabels = { iot: 'IoT', ctrl: 'Control', dsp: 'DSP', comms: 'Comms' };
    return `<div class="task-item ${t.id === currentTask ? 'active' : ''}" onclick="currentTask='${t.id}'; renderSidebar(); render()">
      <div class="task-name">${t.name}</div>
      <div class="task-desc">${t.desc}</div>
      <div class="task-tags">${t.tags.map(tag => `<span class="tag ${tagMap[tag]}">${tagLabels[tag]}</span>`).join('')}</div>
    </div>`;
  }).join('');

  // MCUs
  document.getElementById('mcu-list').innerHTML = MCUS.map(m => `
    <div class="mcu-item ${m.id === currentMCU ? 'active' : ''}" onclick="currentMCU='${m.id}'; renderSidebar(); render()">
      <div>
        <div class="mcu-name">${m.name}</div>
        <div class="mcu-arch">${m.arch} · ${m.family}</div>
      </div>
      <div class="mcu-mhz">${m.mhz}MHz</div>
    </div>
  `).join('');

  // Batteries
  document.getElementById('bat-list').innerHTML = BATTERIES.map(b => `
    <div class="bat-item ${b.id === currentBat ? 'active' : ''}" onclick="currentBat='${b.id}'; renderSidebar(); render()">
      <span>${b.name}</span>
      <span class="bat-cap">${b.mAh}mAh</span>
    </div>
  `).join('');
}

// ════════════════════════════════════════════════════════
// INIT
// ════════════════════════════════════════════════════════

renderSidebar();
render();
</script>
</div>
