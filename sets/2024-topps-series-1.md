---
layout: page
title: 2024 Topps Series 1
permalink: /sets/2024-topps-series-1/
---

{% assign s = site.data.sets["2024_topps_series_1"] %}

<p>{{ s.year }} {{ s.brand }} — {{ s.sport }}. {{ s.notes }}</p>

<div class="filter">
  <label>Filter by player/team
    <input type="text" id="flt" placeholder="e.g., Ohtani or LAD">
  </label>
</div>

<table class="table" id="cl">
  <thead>
    <tr><th>#</th><th>Player</th><th>Team</th><th>Comps</th></tr>
  </thead>
  <tbody>
  {% for c in s.checklist %}
    <tr>
      <td>{{ c.no }}</td>
      <td>{% if c.rc %}<strong>{{ c.player }}</strong> <span class="rc">RC</span>{% else %}{{ c.player }}{% endif %}</td>
      <td>{{ c.team }}</td>
      <td>
        <a class="btn" target="_blank" href="https://www.ebay.com/sch/i.html?_nkw={{ s.year }}+{{ s.brand | uri_escape }}+{{ c.player | uri_escape }}+%23{{ c.no }}&LH_Sold=1&LH_Complete=1&_sop=13">Sold</a>
        <a class="btn btn--secondary" target="_blank" href="https://www.ebay.com/sch/i.html?_nkw={{ s.year }}+{{ s.brand | uri_escape }}+{{ c.player | uri_escape }}+%23{{ c.no }}&_sop=12">Active</a>
      </td>
    </tr>
  {% endfor %}
  </tbody>
</table>

<h3>Parallels</h3>
<ul>
{% for p in s.parallels %}
  <li>{{ p.name }}</li>
{% endfor %}
</ul>

<style>
.filter { margin: .75rem 0; }
.filter input { padding:.5rem .6rem; border:1px solid var(--border); border-radius:8px; min-width:16rem; }
.table { width:100%; border-collapse: collapse; }
.table th, .table td { padding: .5rem .5rem; border-bottom: 1px solid var(--border); }
.table tr:hover { background: rgba(0,0,0,.03); }
.rc { display:inline-block; background:#f59e0b; color:#111; font-size:.75rem; padding:.05rem .35rem; border-radius:6px; margin-left:.35rem; }
</style>

<script>
(function(){
  const q = document.getElementById('flt');
  const tbody = document.querySelector('#cl tbody');
  q.addEventListener('input', ()=>{
    const term = q.value.trim().toLowerCase();
    for (const tr of tbody.querySelectorAll('tr')){
      const text = tr.innerText.toLowerCase();
      tr.style.display = text.includes(term) ? '' : 'none';
    }
  });
})();
</script>

