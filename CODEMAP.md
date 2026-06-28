# CODEMAP — leri-ok.ru portfolio

> Last updated: 2026-06-28  
> Total size: ~11 MB (cases/ 3.9MB + photos/ 6.6MB + 7 HTML ~300KB)

---

## File tree

```
personal/
├── about-me.html          60 KB  — main SPA (tabs: Кейсы / Обо мне / CV / С чем могу помочь)
├── coderabbit.html        36 KB  — case study
├── dna-match-tools.html   38 KB  — case study
├── dna-match.html         36 KB  — case study
├── taara.html             37 KB  — case study  ⚠️ NO footer-next (last in chain?)
├── ydna.html              39 KB  — case study
├── yourroots.html         38 KB  — case study
├── favicon.svg
├── cases/                 3.9 MB — all WebP + 1 MP4
└── photos/                6.6 MB — photo_00-35.webp, video_00-02.mp4, yourroots_00-03.png
```

---

## Case navigation chain

Cards order in about-me.html (top→bottom):

```
dna-match-tools → yourroots → dna-match → ydna → coderabbit → taara
```

Footer next-case chain:

```
dna-match-tools → yourroots → dna-match → ydna → coderabbit → taara → (no next, last)
```

Footer back on all pages → `about-me.html#cases` + `localStorage.setItem('activeTab','cases')`

---

## about-me.html — key structure

### Nav tabs
```html
<button class="tab-btn" data-tab="cases">   <!-- active by default -->
<button class="tab-btn" data-tab="about">
<button class="tab-btn" data-tab="cv">
<button class="tab-btn" data-tab="help">
<button class="tab-btn" data-tab="works" style="display:none;"> <!-- hidden -->
```
Tab panels: `#tab-cases`, `#tab-about`, `#tab-cv`, `#tab-help`, `#tab-works`

### Mobile nav
```html
<button class="burger-btn">  <!-- shown at ≤699px -->
<div class="tab-nav-current"> <!-- current tab name, shown at ≤699px -->
<div class="mobile-menu">    <!-- overlay with .mobile-menu-item buttons -->
```

### Lang toggle (topbar, right side)
```html
<div class="lang-toggle">   <!-- margin-left:auto pushes to right -->
  <button class="lang-btn active" id="btnRU" onclick="setLang('ru')">RU</button>
  <span class="lang-sep">/</span>
  <button class="lang-btn" id="btnEN" onclick="setLang('en')">EN</button>
</div>
<a href="https://t.me/leri_ok" class="discuss-btn">Обсудить</a>
```

### Photos collage
- 36 WebP: `photos/photo_00.webp` … `photos/photo_35.webp`
- 3 MP4: `photos/video_00.mp4`, `video_01.mp4`, `video_02.mp4`
- JS `const items = [{type:'img'|'vid', src:'...'}]`
- Grid class: `.works-masonry`

### i18n system
- `data-i18n="key"` attributes on elements
- `setLang(lang)` reads `translations[lang][key]`, sets `el.textContent`
- Special: `<span data-i18n="back">` inside `<a>` (not on `<a>`) to preserve SVG arrow
- Persisted: `localStorage.setItem('siteLang', lang)`
- Init IIFE reads `localStorage.getItem('siteLang')` on load

### Key i18n keys in about-me
`nav_cases, nav_about, nav_cv, nav_help, discuss, hero_title, hero_sub, hero_cta,
 case_N_title, case_N_sub, case_N_body` (N = 0–5), `btn_zdorovo`

---

## Case pages — key structure (identical pattern)

### Topbar
```html
<nav class="topbar">
  <a href="about-me.html#cases" class="back-link" onclick="localStorage.setItem('activeTab','cases')">
    <svg>…arrow…</svg>
    <span data-i18n="back">Кейсы</span>
  </a>
  <span class="topbar-sep">/</span>
  <span class="topbar-current">PAGE TITLE</span>
  <div class="lang-toggle">  <!-- margin-left:auto -->
    <button class="lang-btn active" id="btnRU" onclick="setLang('ru')">RU</button>
    <span class="lang-sep">/</span>
    <button class="lang-btn" id="btnEN" onclick="setLang('en')">EN</button>
  </div>
  <a href="https://t.me/leri_ok" class="discuss-btn" data-i18n="discuss">Обсудить</a>
</nav>
```

### Footer
```html
<footer class="case-footer">
  <a href="about-me.html#cases" class="footer-back" onclick="…" aria-label="Все кейсы">← Все кейсы</a>
  <a href="NEXT.html" class="footer-next" aria-label="…">Следующий кейс →</a>
  <!-- taara.html has no footer-next -->
</footer>
```

### Meta block pattern
```html
<div class="meta-grid">
  <div class="meta-item"><span class="meta-key" data-i18n="meta_client">Клиент</span><span class="meta-val">…</span></div>
  <div class="meta-item"><span class="meta-key" data-i18n="meta_period">Период</span><span class="meta-val" data-i18n="meta_period_val">…</span></div>
  <div class="meta-item"><span class="meta-key" data-i18n="meta_role">Роль</span><span class="meta-val">…</span></div>
  <div class="meta-item"><span class="meta-key" data-i18n="meta_type">Тип</span><span class="meta-val">…</span></div>
</div>
```

### i18n keys in case pages
`back, discuss, footer_back, footer_next_label, hero_desc,
 dot_0…3, eyebrow_0…3, title_0…2, body_0…N, phase_0…N, result_0…N,
 meta_client, meta_period, meta_role, meta_type, meta_period_val`

---

## CSS conventions

| Element | Font | Size | Letter-spacing | Padding | Border-radius |
|---|---|---|---|---|---|
| `.discuss-btn` | Lebowski | 0.8rem | 0.12em | 8px 20px | 20px |
| Large CTA | Lebowski | 1.1rem | 0.18em | 32px 100px | 300px |
| `.lang-btn` | Lebowski | 0.75rem | — | — | — |

Mobile breakpoint: `@media (max-width: 699px)`  
Mobile overrides: `.tab-btn { display:none!important }`, `.burger-btn { display:flex!important }`,  
`.works-masonry { grid-template-columns:1fr }`, large CTA `width:100%`

---

## assets/cases inventory

| File | Used in |
|---|---|
| `Coderabbit bus.webp` | coderabbit.html |
| `Coderabbit diagram.webp` | coderabbit.html |
| `Coderabbit hero.webp` | coderabbit.html |
| `Coderabbit infographic.webp` | coderabbit.html |
| `DNA Macth Tools cover.webp` | about-me.html (card) |
| `DNA Match Part of the script…` | dna-match.html |
| `DNA Match Request to connect.webp` | dna-match.html |
| `DNA Match Tools Chromosome Canvas.webp` | dna-match-tools.html |
| `DNA Match Tools Cluster Report.webp` | dna-match-tools.html |
| `DNA Match Tools Network Graph.webp` | dna-match-tools.html |
| `DNA Match case cover.webp` | about-me.html (card) |
| `DNA Match form states.webp` | dna-match.html |
| `DNA Match image_8965.webp` | dna-match-tools.html |
| `DNA match MVP.webp` | dna-match.html |
| `DNA match hero.webp` | dna-match.html |
| `Taara AI.webp` | taara.html |
| `Taara Install Guide.webp` | taara.html |
| `Taara cover 2.webp` | taara.html, about-me.html (card) |
| `Taara hero.webp` | taara.html |
| `Taara marketing assets.webp` | taara.html |
| `Taara stand.webp` | taara.html |
| `Y-DNA LP video.mp4` | ydna.html |
| `Y-DNA SEO-page 2.webp` | ydna.html |
| `Y-DNA cover.webp` | about-me.html (card) |
| `Y-DNA timeline.webp` | ydna.html |
| `YourRoots case cover.webp` | about-me.html (card), yourroots.html |
| `comparison slider - AI prototype - Ancient Map.webp` | yourroots.html |
| `comparison slider - New UI - Ancient Map.webp` | yourroots.html |

## assets/photos inventory

- `photo_00.webp` … `photo_35.webp` — collage grid (about-me.html)
- `video_00.mp4`, `video_01.mp4`, `video_02.mp4` — collage videos (about-me.html)
- `yourroots_00.png` … `yourroots_03.png` — yourroots.html inline images

---

## OG / meta

- Domain: `https://leri-ok.ru`
- `og:locale`: `ru_RU`
- favicon: `favicon.svg` (linked in all 7 HTML files)
- All pages have `<title>`, `<meta name="description">`, full OG set, Twitter card

---

## localStorage keys

| Key | Values | Purpose |
|---|---|---|
| `activeTab` | `'cases'` | Restore tab on back-navigation |
| `siteLang` | `'ru'` \| `'en'` | Persist language across pages |
