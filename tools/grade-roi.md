---
layout: page
title: Grade ROI Calculator
permalink: /tools/grade-roi/
---

Estimate expected value (EV) if you grade vs sell raw.

<form id="roi-form" class="tool-form" onsubmit="return false;">
  <div class="row">
    <label>Raw Comp Price ($)
      <input type="number" id="rawPrice" placeholder="25" min="0" step="0.01">
    </label>
    <label>Grading Fee ($)
      <input type="number" id="gradeFee" placeholder="18" min="0" step="0.01" value="18">
    </label>
    <label>Ship/Prep ($)
      <input type="number" id="miscFee" placeholder="4" min="0" step="0.01" value="4">
    </label>
    <label>Marketplace Fee (%)
      <input type="number" id="feePct" placeholder="13" min="0" max="25" step="0.1" value="13">
    </label>
  </div>
  <div class="row">
    <label>PSA 10 Comp ($)
      <input type="number" id="p10" placeholder="150" min="0" step="0.01">
    </label>
    <label>PSA 9 Comp ($)
      <input type="number" id="p9" placeholder="60" min="0" step="0.01">
    </label>
    <label>PSA 8 Comp ($)
      <input type="number" id="p8" placeholder="25" min="0" step="0.01">
    </label>
  </div>
  <div class="row">
    <label>P(PSA 10) %
      <input type="number" id="w10" placeholder="30" min="0" max="100" step="1" value="30">
    </label>
    <label>P(PSA 9) %
      <input type="number" id="w9" placeholder="50" min="0" max="100" step="1" value="50">
    </label>
    <label>P(PSA 8) %
      <input type="number" id="w8" placeholder="20" min="0" max="100" step="1" value="20">
    </label>
  </div>
  <div class="row">
    <button class="btn" id="calc">Calculate</button>
  </div>
</form>

<div id="roi-out" class="roi-out" aria-live="polite"></div>

<script>
(function(){
  function fmt(n){ return isFinite(n) ? '$' + n.toFixed(2) : '$0.00'; }
  function netAfterFees(price, feePct){ return price * (1 - (feePct/100)); }
  function calc(){
    const raw = parseFloat(document.getElementById('rawPrice').value||'0');
    const fee = parseFloat(document.getElementById('gradeFee').value||'0');
    const misc = parseFloat(document.getElementById('miscFee').value||'0');
    const pct = parseFloat(document.getElementById('feePct').value||'0');
    const p10 = parseFloat(document.getElementById('p10').value||'0');
    const p9  = parseFloat(document.getElementById('p9').value||'0');
    const p8  = parseFloat(document.getElementById('p8').value||'0');
    const w10 = (parseFloat(document.getElementById('w10').value||'0')/100);
    const w9  = (parseFloat(document.getElementById('w9').value||'0')/100);
    const w8  = (parseFloat(document.getElementById('w8').value||'0')/100);
    const wsum = w10+w9+w8;
    const w10n = wsum? w10/wsum : 0, w9n = wsum? w9/wsum:0, w8n = wsum? w8/wsum:0;
    const net10 = netAfterFees(p10, pct);
    const net9  = netAfterFees(p9, pct);
    const net8  = netAfterFees(p8, pct);
    const evGraded = (w10n*net10 + w9n*net9 + w8n*net8) - (fee + misc);
    const evRaw    = netAfterFees(raw, pct);
    const delta = evGraded - evRaw;
    const msg = delta >= 0 ? 'Grade looks favorable' : 'Sell raw looks better';
    document.getElementById('roi-out').innerHTML = `
      <div class="cards">
        <div class="card"><h3>Expected (Graded)</h3><p><strong>${fmt(evGraded)}</strong></p></div>
        <div class="card"><h3>Expected (Raw)</h3><p><strong>${fmt(evRaw)}</strong></p></div>
        <div class="card"><h3>Difference</h3><p><strong>${fmt(delta)}</strong></p><p>${msg}</p></div>
      </div>`;
  }
  document.getElementById('calc').addEventListener('click', calc);
})();
</script>

<style>
.tool-form { margin-top: 1rem; }
.tool-form .row { display: flex; gap: 1rem; flex-wrap: wrap; margin-bottom: .75rem; }
.tool-form label { display: flex; flex-direction: column; font-weight: 600; color: var(--muted); }
.tool-form input[type="number"] { padding: .55rem .6rem; border-radius: 8px; border: 1px solid var(--border); min-width: 10rem; }
.roi-out { margin-top: 1rem; }
.cards { display: flex; gap: 1rem; flex-wrap: wrap; }
.card { border: 1px solid var(--border); border-radius: 10px; padding: 1rem; min-width: 12rem; }
.card h3 { margin: 0 0 .5rem; }
</style>

Notes
- Expected values subtract marketplace fees from sale proceeds.
- Adjust probabilities to match your pre‑grade assessment.
- Consider shipping/insurance/time cost as part of misc fee.

