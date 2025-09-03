---
layout: page
title: Centering Overlay
permalink: /tools/centering/
---

Estimate card centering by placing draggable guides over your photo. This runs entirely in your browser.

<div class="centering-tool">
  <div class="controls">
    <label class="file">Upload image<input type="file" id="imgInput" accept="image/*"></label>
    <button class="btn" id="reset">Reset Guides</button>
  </div>
  <div class="canvas-wrap">
    <canvas id="canvas" width="900" height="600" aria-label="Card image with draggable guides"></canvas>
    <div class="overlay" id="overlay">
      <div class="guide v" data-axis="v" data-key="left"   style="left: 120px"></div>
      <div class="guide v" data-axis="v" data-key="right"  style="left: 780px"></div>
      <div class="guide h" data-axis="h" data-key="top"    style="top:  80px"></div>
      <div class="guide h" data-axis="h" data-key="bottom" style="top: 520px"></div>
    </div>
  </div>
  <div class="readout" id="readout">
    <div class="card">
      <h3>Horizontal</h3>
      <p>Left: <span id="Lpx">–</span> px, Right: <span id="Rpx">–</span> px</p>
      <p>Centering: <strong><span id="Hcent">–</span></strong> (min/max borders)</p>
    </div>
    <div class="card">
      <h3>Vertical</h3>
      <p>Top: <span id="Tpx">–</span> px, Bottom: <span id="Bpx">–</span> px</p>
      <p>Centering: <strong><span id="Vcent">–</span></strong></p>
    </div>
    <div class="card">
      <h3>Tips</h3>
      <ul>
        <li>Drag the four guides to the inner border edges.</li>
        <li>Use pinch/zoom in your browser to be precise.</li>
        <li>PSA 10 targets ≈ 55/45 or better; PSA 9 ≈ 60/40.</li>
      </ul>
    </div>
  </div>
</div>

<style>
.centering-tool { margin-top: 1rem; }
.controls { display:flex; gap:.5rem; align-items:center; margin-bottom:.5rem; }
.controls .file input{ display:block; }
.canvas-wrap { position: relative; border:1px solid var(--border); border-radius: 8px; overflow:hidden; max-width: 100%; }
canvas { display:block; width:100%; height:auto; background:#111; }
.overlay { position:absolute; inset:0; pointer-events:none; }
.guide { position:absolute; background: rgba(37,99,235,.85); pointer-events:auto; }
.guide.v { top:0; bottom:0; width:2px; cursor: ew-resize; }
.guide.h { left:0; right:0; height:2px; cursor: ns-resize; }
.guide[data-key="right"], .guide[data-key="bottom"] { background: rgba(200,16,46,.85); }
.readout { display:flex; gap:1rem; flex-wrap:wrap; margin-top:.75rem; }
.readout .card { border:1px solid var(--border); border-radius:10px; padding: .75rem 1rem; min-width: 14rem; }
</style>

<script>
(function(){
  const canvas = document.getElementById('canvas');
  const ctx = canvas.getContext('2d');
  const overlay = document.getElementById('overlay');
  const imgInput = document.getElementById('imgInput');
  const resetBtn = document.getElementById('reset');
  const Lpx = document.getElementById('Lpx');
  const Rpx = document.getElementById('Rpx');
  const Tpx = document.getElementById('Tpx');
  const Bpx = document.getElementById('Bpx');
  const Hcent = document.getElementById('Hcent');
  const Vcent = document.getElementById('Vcent');

  let img = new Image();
  let natural = { w: 900, h: 600 };

  function draw(imgObj){
    const maxW = 1200, maxH = 800;
    let w = imgObj ? imgObj.naturalWidth : natural.w;
    let h = imgObj ? imgObj.naturalHeight: natural.h;
    const scale = Math.min(maxW / w, maxH / h);
    w = Math.round(w * scale); h = Math.round(h * scale);
    canvas.width = w; canvas.height = h;
    ctx.fillStyle = '#000'; ctx.fillRect(0,0,w,h);
    if (imgObj) ctx.drawImage(imgObj, 0, 0, w, h);
    overlay.style.width = w + 'px';
    overlay.style.height = h + 'px';
    updateReadout();
  }

  imgInput.addEventListener('change', (e)=>{
    const f = e.target.files && e.target.files[0];
    if (!f) return;
    const url = URL.createObjectURL(f);
    const tmp = new Image();
    tmp.onload = () => { img = tmp; draw(img); URL.revokeObjectURL(url); };
    tmp.src = url;
  });

  resetBtn.addEventListener('click', ()=>{
    // place guides with 10%/90% margins by default
    const w = canvas.width, h = canvas.height;
    setGuidePos('left',  Math.round(w*0.12), 'v');
    setGuidePos('right', Math.round(w*0.88), 'v');
    setGuidePos('top',   Math.round(h*0.15), 'h');
    setGuidePos('bottom',Math.round(h*0.85), 'h');
    updateReadout();
  });

  function setGuidePos(key, val, axis){
    const el = overlay.querySelector(`.guide[data-key="${key}"]`);
    if (!el) return;
    if (axis === 'v') el.style.left = val + 'px';
    else el.style.top = val + 'px';
  }

  function getGuidePos(key){
    const el = overlay.querySelector(`.guide[data-key="${key}"]`);
    const rect = el.getBoundingClientRect();
    const orect = overlay.getBoundingClientRect();
    if (el.dataset.axis === 'v') return Math.round(rect.left - orect.left);
    return Math.round(rect.top - orect.top);
  }

  function clamp(n, min, max){ return Math.max(min, Math.min(max, n)); }

  overlay.addEventListener('mousedown', startDrag);
  overlay.addEventListener('touchstart', startDrag, {passive:false});

  function startDrag(ev){
    const target = ev.target.closest('.guide');
    if (!target) return;
    ev.preventDefault();
    const axis = target.dataset.axis;
    const start = (ev.touches? ev.touches[0] : ev);
    const oRect = overlay.getBoundingClientRect();
    const startPos = axis==='v' ? target.offsetLeft : target.offsetTop;
    const startXY = axis==='v' ? start.clientX : start.clientY;

    function move(e){
      const pt = (e.touches? e.touches[0] : e);
      const cur = axis==='v' ? pt.clientX : pt.clientY;
      let delta = cur - startXY;
      let next = startPos + delta;
      if (axis==='v') next = clamp(next, 0, overlay.clientWidth);
      else next = clamp(next, 0, overlay.clientHeight);
      if (axis==='v') target.style.left = next + 'px'; else target.style.top = next + 'px';
      updateReadout();
    }
    function end(){ window.removeEventListener('mousemove', move); window.removeEventListener('mouseup', end); overlay.removeEventListener('touchmove', move); overlay.removeEventListener('touchend', end); }
    window.addEventListener('mousemove', move);
    window.addEventListener('mouseup', end);
    overlay.addEventListener('touchmove', move, {passive:false});
    overlay.addEventListener('touchend', end);
  }

  function pct(a,b){ const total=a+b; if(!total) return '—'; const small=Math.min(a,b), large=Math.max(a,b); const ratio = small/large; const leftPct = Math.round(100*a/total); const rightPct = Math.round(100*b/total); return `${leftPct}/${rightPct} (${(ratio*100).toFixed(1)}%)`; }

  function updateReadout(){
    const w = canvas.width, h = canvas.height;
    const left = getGuidePos('left');
    const right = w - getGuidePos('right');
    const top = getGuidePos('top');
    const bottom = h - getGuidePos('bottom');
    Lpx.textContent = left; Rpx.textContent = right; Tpx.textContent = top; Bpx.textContent = bottom;
    Hcent.textContent = pct(left, right);
    Vcent.textContent = pct(top, bottom);
  }

  // initial draw
  draw(null);
})();
</script>

