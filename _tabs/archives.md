---
layout: page
title: Archives
icon: fas fa-archive
order: 4
---

<style>
  .archive-section {
    margin-bottom: 2.5rem;
  }
  .archive-section h3 {
    margin-bottom: 1rem;
    padding-bottom: 0.5rem;
    border-bottom: 1px solid var(--border-color);
  }
  .file-item {
    display: flex !important;
    align-items: center !important;
    justify-content: space-between !important;
    padding: 1rem 1.2rem !important;
    border: 1px solid var(--border-color) !important;
    border-radius: 8px !important;
    margin-bottom: 0.8rem !important;
    text-decoration: none !important;
    color: inherit !important;
    transition: background-color 0.2s !important;
    background: transparent !important;
    box-shadow: none !important;
  }
  /* Usunięcie domyślnych ikon linków z motywu Chirpy */
  .file-item::after, .file-item::before {
    display: none !important;
    content: none !important;
  }
  .file-item:hover {
    background-color: var(--button-bg) !important;
  }
  .file-info {
    display: flex !important;
    align-items: center !important;
    gap: 1.2rem !important;
  }
  .file-icon {
    font-size: 1.8rem !important;
  }
  .file-details {
    display: flex !important;
    flex-direction: column !important;
  }
  .file-name {
    font-weight: 600 !important;
    font-size: 1rem !important;
    line-height: 1.2 !important;
    margin-bottom: 0.2rem !important;
  }
  .file-meta {
    font-size: 0.85rem !important;
    opacity: 0.6 !important;
  }
  .file-action {
    font-size: 1.2rem !important;
    opacity: 0.8 !important;
  }
</style>

<div class="archive-section" markdown="0">
  <h3>📁 Pliki i Dokumenty</h3>

  <a href="{{ '/assets/files/instrukcja.pdf' | relative_url }}" class="file-item" target="_blank">
    <div class="file-info">
      <div class="file-icon">📄</div>
      <div class="file-details">
        <span class="file-name">Instrukcja obsługi.pdf</span>
        <span class="file-meta">Dokument PDF · 2.4 MB</span>
      </div>
    </div>
    <div class="file-action">⬇️</div>
  </a>

  <a href="{{ '/assets/files/skrypt.zip' | relative_url }}" class="file-item">
    <div class="file-info">
      <div class="file-icon">📦</div>
      <div class="file-details">
        <span class="file-name">Narzędzia_Downgrade.zip</span>
        <span class="file-meta">Archiwum ZIP · 15 MB</span>
      </div>
    </div>
    <div class="file-action">⬇️</div>
  </a>
</div>

<div class="archive-section" markdown="0">
  <h3>🔗 Zewnętrzne linki i repozytoria</h3>

  <a href="https://github.com/pwnerblu/surrealra1n" class="file-item" target="_blank">
    <div class="file-info">
      <div class="file-icon">🌐</div>
      <div class="file-details">
        <span class="file-name">Repozytorium Surrealra1n</span>
        <span class="file-meta">GitHub Link</span>
      </div>
    </div>
    <div class="file-action">↗️</div>
  </a>
</div>

<div class="archive-section" markdown="0">
  <h3>🖼️ Galeria schematów</h3>

  <a href="{{ '/assets/img/schemat.png' | relative_url }}" class="file-item" target="_blank">
    <div class="file-info">
      <div class="file-icon">🖼️</div>
      <div class="file-details">
        <span class="file-name">Schemat_plyty_glownej.png</span>
        <span class="file-meta">Obraz PNG · Wymaga powiększenia</span>
      </div>
    </div>
    <div class="file-action">👁️</div>
  </a>
</div>
