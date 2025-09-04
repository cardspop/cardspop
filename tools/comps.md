---
layout: page
title: Comp Builder
permalink: /tools/comps/
---

Build a quick eBay search for sold or active listings. This does not track you or use any backend.

Related guides: [How to Comp Cards]({{ '/guides/comping/' | relative_url }}) · [How to Grade Cards]({{ '/guides/grading/' | relative_url }})

<div class="examples" aria-label="Examples">
  <label for="preset"><strong>Examples:</strong></label>
  <select id="preset">
    <option value="">Select an example…</option>
    <option value="ohtani-refractor">Ohtani · Chrome Refractor</option>
    <option value="elly-rainbow">Elly · Rainbow Foil</option>
    <option value="julio-rc">Julio · RC #150</option>
  </select>
</div>

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
    <label>Card #
      <input type="text" id="cardNo" placeholder="#150">
    </label>
  </div>
  <div class="row">
    <label>Sport / Category
      <select id="category">
        <option value="">All Cards</option>
        <option value="213">Baseball</option>
        <option value="214">Basketball</option>
        <option value="57988">Football</option>
        <option value="261328">Soccer</option>
        <option value="26320">Hockey</option>
      </select>
    </label>
    <label>Price Min
      <input type="number" id="minPrice" placeholder="10" min="0" step="1">
    </label>
    <label>Price Max
      <input type="number" id="maxPrice" placeholder="500" min="0" step="1">
    </label>
  </div>
  <details class="advanced" open>
    <summary>Advanced Filters</summary>
    <div class="row">
    <label>Marketplace
      <select id="domain">
        <option value="com">ebay.com (US)</option>
        <option value="co.uk">ebay.co.uk (UK)</option>
        <option value="ca">ebay.ca (CA)</option>
      </select>
    </label>
    <label>Sort
      <select id="sort">
        <option value="">Best Match</option>
        <option value="12">Newly Listed</option>
        <option value="1">Ending Soonest</option>
        <option value="15">Price+Ship: Lowest</option>
        <option value="16">Price+Ship: Highest</option>
        <option value="13">Recently Ended (Sold)</option>
      </select>
    </label>
    <label><input type="checkbox" id="freeShip"> Free Shipping</label>
    <label><input type="checkbox" id="returns"> Returns Accepted</label>
    </div>
  </details>
  <details class="advanced">
    <summary>Advanced Options (Power Users)</summary>
    <div class="row">
    <fieldset>
      <legend>Condition filters</legend>
      <label><input type="checkbox" id="condRaw" checked> Raw</label>
      <label><input type="checkbox" id="condPSA10"> PSA 10</label>
      <label><input type="checkbox" id="condPSA9"> PSA 9</label>
      <label><input type="checkbox" id="condBGS95"> BGS 9.5</label>
      <label><input type="checkbox" id="condSGC10"> SGC 10</label>
      <label><input type="checkbox" id="condCGC10"> CGC 10</label>
      <button type="button" class="chip" id="slabOnly">Slabbed only</button>
    </fieldset>
    <fieldset>
      <legend>Listing type</legend>
      <label><input type="checkbox" id="auction"> Auctions</label>
      <label><input type="checkbox" id="bin"> Buy It Now</label>
    </fieldset>
    <fieldset>
      <legend>Search options</legend>
      <label><input type="checkbox" id="exactPlayer"> Exact player</label>
      <label><input type="checkbox" id="exactParallel"> Exact parallel</label>
      <label title="Search listing descriptions (can add noise)"><input type="checkbox" id="titleDesc"> Search description</label>
      <label><input type="checkbox" id="excludeLots" checked> Exclude lots</label>
    </fieldset>
    </div>
    <div class="row">
    <fieldset>
      <legend>Synonym presets</legend>
      <label><input type="checkbox" id="synRC"> RC ↔ Rookie Card</label>
      <label><input type="checkbox" id="synRefractor"> Refractor ↔ Silver Prizm</label>
      <label><input type="checkbox" id="synNumbering"> /xx ↔ #/xx</label>
    </fieldset>
    </div>
  </details>
  <div class="row">
    <button class="btn" id="sold">Open Sold</button>
    <button class="btn btn--secondary" id="active" style="margin-left:.5rem">Open Active</button>
    <button class="btn" id="soldAll" style="margin-left:.5rem">Sold (All Variants)</button>
    <button class="btn btn--secondary" id="activeAll" style="margin-left:.5rem">Active (All Variants)</button>
    <button class="btn" id="copy" style="margin-left:.5rem">Copy Link</button>
  </div>
  <div class="row">
    <button class="btn" id="showSold">Show Sold Links</button>
    <button class="btn btn--secondary" id="showActive" style="margin-left:.5rem">Show Active Links</button>
    <button class="btn" id="showAll" style="margin-left:.5rem">Show All Variants</button>
  </div>
</form>

<div id="comp-results" class="comp-results" hidden>
  <h3>Search Link Preview</h3>
  <div id="results-summary" class="results-summary"></div>
  <ul id="results-list" class="results-list"></ul>
  <div class="row">
    <button class="btn" id="openShown">Open Shown In Tabs</button>
  </div>
</div>

<script>
(function(){
  const enc = s => encodeURIComponent(s.trim()).replace(/%20/g, '+');
  // remember marketplace/sort
  try {
    const savedDomain = localStorage.getItem('cards-domain');
    const savedSort = localStorage.getItem('cards-sort');
    if (savedDomain) document.getElementById('domain').value = savedDomain;
    if (savedSort) document.getElementById('sort').value = savedSort;
    document.getElementById('domain').addEventListener('change', e=> localStorage.setItem('cards-domain', e.target.value));
    document.getElementById('sort').addEventListener('change', e=> localStorage.setItem('cards-sort', e.target.value));
  } catch(e) {}
  function baseParts(){
    const year = document.getElementById('year').value.trim();
    const brand = document.getElementById('brand').value.trim();
    const player = document.getElementById('player').value.trim();
    const parallel = document.getElementById('parallel').value.trim();
    const cardNo = document.getElementById('cardNo').value.trim();
    const exactPlayer = document.getElementById('exactPlayer')?.checked;
    const exactParallel = document.getElementById('exactParallel')?.checked;
    const excludeLots = document.getElementById('excludeLots')?.checked;
    const parts = [];
    if (year) parts.push(year);
    if (brand) parts.push(brand);
    if (player) parts.push(exactPlayer ? `"${player}"` : player);
    if (parallel) parts.push(exactParallel ? `"${parallel}"` : parallel);
    if (cardNo) parts.push(cardNo.startsWith('#') ? cardNo : `#${cardNo}`);
    return { parts, excludeLots };
  }
  function toQuery(parts, modifiers){
    let q = parts.join(' ').replace(/\s+/g,' ').trim();
    if (modifiers?.length) q += ' ' + modifiers.join(' ');
    return enc(q);
  }
  function buildParams(sold){
    const cat = document.getElementById('category').value;
    const auction = document.getElementById('auction').checked;
    const bin = document.getElementById('bin').checked;
    const minP = document.getElementById('minPrice').value;
    const maxP = document.getElementById('maxPrice').value;
    const titleDesc = document.getElementById('titleDesc')?.checked;
    let params = '';
    if (sold) params += '&LH_Sold=1&LH_Complete=1';
    const sort = document.getElementById('sort').value;
    if (sort) params += '&_sop=' + encodeURIComponent(sort);
    else params += sold ? '&_sop=13' : '&_sop=12';
    if (cat) params += '&_sacat=' + encodeURIComponent(cat);
    if (auction) params += '&LH_Auction=1';
    if (bin) params += '&LH_BIN=1';
    if (minP) params += '&_udlo=' + encodeURIComponent(minP);
    if (maxP) params += '&_udhi=' + encodeURIComponent(maxP);
    if (titleDesc) params += '&LH_TitleDesc=1';
    if (document.getElementById('freeShip').checked) params += '&LH_FS=1';
    if (document.getElementById('returns').checked) params += '&LH_Returns=1';
    return params;
  }
  function condMods(){
    const mods = [];
    if (document.getElementById('condRaw')?.checked) mods.push('-PSA -BGS -SGC');
    if (document.getElementById('condPSA10')?.checked) mods.push('PSA 10 -BGS -SGC');
    if (document.getElementById('condPSA9')?.checked)  mods.push('PSA 9 -BGS -SGC');
    if (document.getElementById('condBGS95')?.checked) mods.push('BGS 9.5 -PSA -SGC');
    if (document.getElementById('condSGC10')?.checked) mods.push('SGC 10 -PSA -BGS');
    if (document.getElementById('condCGC10')?.checked) mods.push('CGC 10 -PSA -BGS -SGC');
    return mods.length ? mods : [''];
  }
  function synonymParts(parts){
    const out = [parts];
    if (document.getElementById('synRC')?.checked){
      out.push(parts.map(x=> x.replace(/\bRC\b/gi,'"Rookie Card"')));
    }
    if (document.getElementById('synRefractor')?.checked){
      out.push(parts.map(x=> x.replace(/\bRefractor\b/gi,'"Silver Prizm"')));
    }
    if (document.getElementById('synNumbering')?.checked){
      out.push(parts.map(x=> x.replace(/\/(\d{1,3})\b/g,'#/$1')));
    }
    const seen = new Set();
    const uniq = [];
    for (const arr of out){ const key = arr.join(' '); if(!seen.has(key)){ seen.add(key); uniq.push(arr); } }
    return uniq;
  }
  function buildUrls(sold, allVariants){
    const { parts, excludeLots } = baseParts();
    const partSets = allVariants ? synonymParts(parts) : [parts];
    const cmods = condMods();
    const urls = [];
    for (const p of partSets){
      for (const m of cmods){
        const mods = [];
        if (m) mods.push(m);
        if (excludeLots) mods.push('-lot -bundle');
        const q = toQuery(p, mods);
        const dom = document.getElementById('domain').value || 'com';
        urls.push('https://www.ebay.' + dom + '/sch/i.html?_nkw=' + q + buildParams(sold));
      }
    }
    return urls;
  }
  function openUrl(sold){
    buildUrls(sold, false).forEach(u=> window.open(u, '_blank'));
  }
  function openUrlAll(sold){
    buildUrls(sold, true).forEach(u=> window.open(u, '_blank'));
  }
  function renderResults(sold, allVariants){
    const urls = buildUrls(sold, !!allVariants);
    const box = document.getElementById('comp-results');
    const list = document.getElementById('results-list');
    const summary = document.getElementById('results-summary');
    list.innerHTML = '';
    // label links with condition/variant if encoded in query
    urls.forEach((u)=>{
      const li = document.createElement('li');
      li.innerHTML = `<a href="${u}" target="_blank">${u.includes('PSA+10')?'PSA 10':u.includes('PSA+9')?'PSA 9':u.includes('BGS+9.5')?'BGS 9.5':u.includes('SGC+10')?'SGC 10':u.includes('CGC+10')?'CGC 10':'Raw'} · ${u.includes('Rookie+Card')?'RC→Rookie Card':u.includes('Silver+Prizm')?'Refractor→Silver Prizm':u.includes('%23%2F')?'/xx→#/xx':'Base'} · ebay.${(document.getElementById('domain').value||'com')}</a>`;
      list.appendChild(li);
    });
    summary.textContent = `${sold ? 'Sold' : 'Active'} · ${urls.length} link${urls.length!==1?'s':''}`;
    box.hidden = false;
  }
  function copyLink(){
    const urls = buildUrls(true, false);
    const url = urls[0];
    if (navigator.clipboard?.writeText) navigator.clipboard.writeText(url).then(()=>alert('Link copied to clipboard')).catch(()=>alert(url));
    else alert(url);
  }
  document.getElementById('sold').addEventListener('click', () => openUrl(true));
  document.getElementById('active').addEventListener('click', () => openUrl(false));
  document.getElementById('soldAll').addEventListener('click', () => openUrlAll(true));
  document.getElementById('activeAll').addEventListener('click', () => openUrlAll(false));
  document.getElementById('showSold').addEventListener('click', () => renderResults(true, false));
  document.getElementById('showActive').addEventListener('click', () => renderResults(false, false));
  document.getElementById('showAll').addEventListener('click', () => renderResults(true, true));
  document.getElementById('openShown').addEventListener('click', () => {
    document.querySelectorAll('#results-list a').forEach(a=> window.open(a.href, '_blank'));
  });
  document.getElementById('copy').addEventListener('click', copyLink);

  // Slabbed only toggle
  document.getElementById('slabOnly').addEventListener('click', ()=>{
    const ids = ['condRaw','condPSA10','condPSA9','condBGS95','condSGC10','condCGC10'];
    const el = id=> document.getElementById(id);
    el('condRaw').checked = false;
    el('condPSA10').checked = true;
    el('condPSA9').checked = true;
    el('condBGS95').checked = true;
    el('condSGC10').checked = true;
    el('condCGC10').checked = true;
  });

  // Example chips
  function applyPreset(name){
    const set = (id,val)=>{ const el=document.getElementById(id); el.value = val; };
    const check = (id,val)=>{ const el=document.getElementById(id); el.checked = val; };
    // reset basics
    set('year',''); set('brand',''); set('player',''); set('parallel',''); set('cardNo','');
    check('exactPlayer', true); check('exactParallel', false);
    if (name==='ohtani-refractor'){
      set('year','2024'); set('brand','Topps Chrome'); set('player','Shohei Ohtani'); set('parallel','Refractor');
    } else if (name==='elly-rainbow'){
      set('year','2024'); set('brand','Topps'); set('player','Elly De La Cruz'); set('parallel','Rainbow Foil');
    } else if (name==='julio-rc'){
      set('year','2022'); set('brand','Topps'); set('player','Julio Rodriguez'); set('cardNo','#150');
    }
    renderResults(true,false);
  }
  const presetSel = document.getElementById('preset');
  presetSel.addEventListener('change', ()=>{
    if (presetSel.value) applyPreset(presetSel.value);
  });
})();
</script>

<style>
.comp-form { margin-top: 1rem; }
.comp-form .row { display: flex; gap: 1rem; flex-wrap: wrap; margin-bottom: .75rem; }
.comp-form label { display: flex; flex-direction: column; font-weight: 600; color: var(--muted); }
.comp-form input[type="text"], .comp-form input[type="number"], .comp-form select { padding: .55rem .6rem; border-radius: 8px; border: 1px solid var(--border); min-width: 15rem; }
fieldset { border: 1px solid var(--border); border-radius: 8px; padding: .5rem .75rem; }
legend { padding: 0 .25rem; color: var(--muted); }
.examples { margin:.5rem 0 1rem; display:flex; gap:.5rem; align-items:center; flex-wrap:wrap; }
.chip { border:1px solid var(--border); background:transparent; padding:.35rem .6rem; border-radius:999px; cursor:pointer; }
.advanced { border:1px solid var(--border); border-radius:10px; padding:.5rem .75rem; margin:.5rem 0; }
.advanced > summary { cursor:pointer; font-weight:600; color: var(--muted); }
.comp-results { margin-top: 1rem; }
.comp-results .results-list { list-style: none; padding: 0; }
.comp-results .results-list li { padding: .35rem 0; border-bottom: 1px solid var(--border); }
.comp-results .results-summary { color: var(--muted); margin-bottom: .35rem; }
</style>

Tips
- Use Sold first, then Active to see ask vs market.
- Add specific card numbers if needed (e.g., `#150`).
- Exclude lots with `-lot -bundle` in Parallel/Insert field.
- For auctions only, add `&_sacat=0&LH_Auction=1` to the eBay URL.
- Categories: Baseball (213), Basketball (214), Football (57988), Soccer (261328), Hockey (26320).
