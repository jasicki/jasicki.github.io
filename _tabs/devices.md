---
layout: page
title: Devices
icon: "fas fa-tv"
order: 2
---

<style>
.device-grid {
  display: grid !important;
  grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
  gap: 1.2rem;
  margin-top: 1.5rem;
}
.device-card {
  display: flex !important;
  flex-direction: column !important;
  align-items: center !important;
  text-align: center;
  border: 1px solid var(--border-color);
  border-radius: 8px;
  padding: 1.2rem;
  text-decoration: none !important;
  color: inherit !important;
}
.device-card img {
  height: 64px;
  width: auto;
  object-fit: contain;
  margin-bottom: .6rem;
  display: block;
}
.device-card strong { display: block; }
.device-card span {
  font-size: .82rem;
  opacity: .65;
  margin-top: .25rem;
  display: block;
}
</style>

{::nomarkdown}
<div class="device-grid">
  <a href="/devices/iphone-5c" class="device-card">
    <img src="/assets/img/iphone.png" alt="iPhone 5c">
    <strong>iPhone 5c</strong>
    <span>iOS · 2013</span>
  </a>
  <a href="/devices/xbox-360" class="device-card">
    <img src="/assets/img/xbox.png" alt="Xbox 360">
    <strong>Xbox 360</strong>
    <span>Console · 2005</span>
  </a>
</div>
{:/nomarkdown}
