---
layout: default
title: Home
---

<section class="hero">
  <div class="wrapper">
    <h1 class="hero__title">{{ site.title | escape }}</h1>
    <p class="hero__tagline">{{ site.description | escape }}</p>
    <p style="margin-top:1rem">
      <a class="btn" href="{{ '/blog/' | relative_url }}">Read the blog →</a>
    </p>
  </div>
  
</section>

<div class="wrapper" style="padding:1.25rem 0">
  <p>Welcome. This is a simple landing page. Use the button above or the header link to navigate to the blog.</p>
  
</div>
