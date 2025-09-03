---
layout: default
title: Home
---

<section class="hero">
  <div class="wrapper">
    <h1 class="hero__title">{{ site.title | escape }}</h1>
    <p class="hero__tagline">{{ site.description | escape }}</p>
    <p style="margin-top:1rem; display:flex; gap:.5rem; flex-wrap:wrap">
      <a class="btn btn--cta" href="{{ '/blog/' | relative_url }}">Read the Blog →</a>
      <a class="btn" href="{{ '/tools/comps/' | relative_url }}">Comp a Card</a>
      <a class="btn" href="{{ '/tools/grade-roi/' | relative_url }}">Grade ROI</a>
      <a class="btn" href="{{ '/tools/centering/' | relative_url }}">Centering Overlay</a>
      <a class="btn btn--secondary" href="{{ '/sets/' | relative_url }}">Sets</a>
      <a class="btn btn--secondary" href="{{ '/guides/' | relative_url }}">Guides</a>
    </p>
  </div>
  
</section>

<div class="wrapper" style="padding:1.25rem 0">
  <p>Welcome. This is a simple landing page. Use the button above or the header link to navigate to the blog.</p>
  
</div>
