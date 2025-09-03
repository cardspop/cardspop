---
layout: page
title: Comp Builder
permalink: /tools/comps/
---

Build a quick eBay search for sold or active listings. This does not track you or use any backend.

<form id="comp-form" class="comp-form" onsubmit="return false;">
  <div class="row">
    <label>Year
      <input type="number" id="year" placeholder="2024" min="1900" max="2100">
    </label>
    <label>Brand / Set
      <input type="text" id="brand" placeholder="Topps Chrome">
    </label>
  </div>
  <div class="row">
    <label>Player
      <input type="text" id="player" placeholder="Shohei Ohtani">
    </label>
    <label>Parallel / Insert
      <input type="text" id="parallel" placeholder="Rainbow Foil, Gold /50, Refractor">
    </label>
  </div>
  <div class="row">
    <fieldset>
      <legend>Condition filter</legend>
      <label><input type="radio" name="cond" value="raw" checked> Raw</label>
      <label><input type="radio" name="cond" value="psa10"> PSA 10</label>
      <label><input type="radio" name="cond" value="psa9"> PSA 9</label>
    </fieldset>
  </div>
  <div class="row">
    <button class="btn" id="sold">Open Sold</button>
    <button class="btn btn--secondary" id="active" style="margin-left:.5rem">Open Active</button>
  </div>
</form>

<script>
(function(){
  const enc = s => encodeURIComponent(s.trim()).replace(/%20/g, '+');
  function buildQuery() {
    const year = document.getElementById('year').value.trim();
    const brand = document.getElementById('brand').value.trim();
    const player = document.getElementById('player').value.trim();
    const parallel = document.getElementById('parallel').value.trim();
    const cond = (document.querySelector('input[name="cond"]:checked')||{}).value;
    let parts = [];
    if (year) parts.push(year);
    if (brand) parts.push(brand);
    if (player) parts.push(player);
    if (parallel) parts.push(parallel);
    let q = parts.join(' ').replace(/\s+/g,' ').trim();
    if (cond === 'raw') q += ' -PSA -BGS -SGC';
    if (cond === 'psa10') q += ' PSA 10 -BGS -SGC';
    if (cond === 'psa9') q += ' PSA 9 -BGS -SGC';
    return enc(q);
  }
  function openUrl(sold){
    const q = buildQuery();
    const base = 'https://www.ebay.com/sch/i.html?_nkw=' + q;
    const params = sold ? '&LH_Sold=1&LH_Complete=1&_sop=13' : '&_sop=12';
    window.open(base + params, '_blank');
  }
  document.getElementById('sold').addEventListener('click', () => openUrl(true));
  document.getElementById('active').addEventListener('click', () => openUrl(false));
})();
</script>

<style>
.comp-form { margin-top: 1rem; }
.comp-form .row { display: flex; gap: 1rem; flex-wrap: wrap; margin-bottom: .75rem; }
.comp-form label { display: flex; flex-direction: column; font-weight: 600; color: var(--muted); }
.comp-form input[type="text"], .comp-form input[type="number"] { padding: .55rem .6rem; border-radius: 8px; border: 1px solid var(--border); min-width: 15rem; }
fieldset { border: 1px solid var(--border); border-radius: 8px; padding: .5rem .75rem; }
legend { padding: 0 .25rem; color: var(--muted); }
</style>

Tips
- Use Sold first, then Active to see ask vs market.
- Add specific card numbers if needed (e.g., `#150`).
- Exclude lots with `-lot -bundle` in Parallel/Insert field.
- For auctions only, add `&_sacat=0&LH_Auction=1` to the eBay URL.

