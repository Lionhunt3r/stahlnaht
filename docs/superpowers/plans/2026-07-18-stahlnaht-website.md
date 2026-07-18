# Stahlnaht Metallbau Website — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a fast, mobile-first, static one-page website for Stahlnaht Metallbau (Timo Betz) that presents the business, shows real project photos, and drives contact via direct channels (WhatsApp, Telegram, phone, email, LinkedIn).

**Architecture:** A single `index.html` styled with Tailwind CSS (CDN), Google Fonts, and minimal vanilla JS for scroll effects, a lightbox, and nav behavior. No build step, no backend. Images live in `assets/images/`. Deployable free on GitHub Pages / Netlify.

**Tech Stack:** HTML5, Tailwind CSS (CDN with inline config), Google Fonts (Oswald for headlines, Inter for body), vanilla JavaScript (IntersectionObserver + lightbox).

## Global Constraints

- **Single file:** All markup, styles config, and scripts live in `index.html`. No npm, no build tooling.
- **Tailwind via CDN:** `<script src="https://cdn.tailwindcss.com"></script>` with an inline `tailwind.config` for custom colors/fonts.
- **Language:** German only.
- **No contact form. No cookie banner. No tracking.**
- **Color palette:** background navy `#14142a`–`#1a1a2e`; silver-grey text `#c0c6d8`; white headlines; oak accent `#c9a56a`.
- **Fonts:** Headlines = Oswald (uppercase, bold); body = Inter.
- **Mobile-first:** every section must look clean at 375px width and scale up. Large touch targets.
- **Real contact data:** WhatsApp/phone `+4917682137608`, Telegram `@Timobetz`, email `info@stahlnaht.de`, LinkedIn `https://www.linkedin.com/in/timo-betz-59357b321`. Instagram = placeholder.
- **Assets present:** `assets/images/logo.jpeg`, `treppe-detail.jpeg`, `treppe-frontal.jpeg`, `treppe-galerie.jpeg`.
- **Verification method:** Each task is verified by opening `index.html` in a browser (or via the `run`/`verify` skill) and checking the described visual/functional result at mobile (375px) and desktop widths. There is no unit-test harness for static HTML — browser observation is the test.

---

### Task 1: Document skeleton, Tailwind config, fonts, and fixed navigation

**Files:**
- Create: `index.html`

**Interfaces:**
- Consumes: nothing.
- Produces: the base HTML document with `<head>` (meta, Tailwind CDN + inline config, Google Fonts, custom `<style>` for smooth-scroll and font families), a fixed top `<nav>` with anchor links (`#leistungen`, `#ueber`, `#referenzen`, `#kontakt`), and empty `<section>` placeholders with those IDs. Later tasks fill the sections. Nav is transparent at top, gains a navy background on scroll (JS toggles a class).

- [ ] **Step 1: Create `index.html` with head, Tailwind config, fonts, and nav**

```html
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Stahlnaht Metallbau — Timo Betz | Speyer & Umgebung</title>
  <meta name="description" content="Stahlnaht Metallbau — Individuelle Stahlkonstruktionen, Geländer, Treppen und Schweißarbeiten mit Präzision. Timo Betz, Speyer und Umgebung.">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Oswald:wght@500;600;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            navy: { DEFAULT: '#1a1a2e', deep: '#14142a', light: '#252546' },
            silver: '#c0c6d8',
            oak: '#c9a56a',
          },
          fontFamily: {
            head: ['Oswald', 'sans-serif'],
            body: ['Inter', 'sans-serif'],
          },
        },
      },
    };
  </script>
  <style>
    html { scroll-behavior: smooth; }
    body { font-family: 'Inter', sans-serif; }
    .font-head { font-family: 'Oswald', sans-serif; }
    /* fade-in on scroll */
    .reveal { opacity: 0; transform: translateY(24px); transition: opacity .6s ease, transform .6s ease; }
    .reveal.visible { opacity: 1; transform: translateY(0); }
  </style>
</head>
<body class="bg-navy-deep text-silver font-body antialiased">

  <!-- NAV -->
  <nav id="nav" class="fixed top-0 inset-x-0 z-50 transition-colors duration-300">
    <div class="max-w-6xl mx-auto px-5 h-16 flex items-center justify-between">
      <a href="#top" class="font-head font-bold tracking-widest text-white text-lg">STAHLNAHT</a>
      <div class="hidden md:flex items-center gap-8 text-sm tracking-wide">
        <a href="#leistungen" class="hover:text-white transition">Leistungen</a>
        <a href="#ueber" class="hover:text-white transition">Über Timo</a>
        <a href="#referenzen" class="hover:text-white transition">Referenzen</a>
        <a href="#kontakt" class="hover:text-white transition">Kontakt</a>
      </div>
      <a href="https://wa.me/4917682137608" target="_blank" rel="noopener"
         class="md:hidden text-xs font-semibold bg-silver text-navy-deep px-3 py-2 rounded">WhatsApp</a>
    </div>
  </nav>

  <span id="top"></span>

  <!-- Sections filled by later tasks -->
  <header id="hero"></header>
  <section id="leistungen"></section>
  <section id="ueber"></section>
  <section id="referenzen"></section>
  <section id="kontakt"></section>
  <footer id="footer"></footer>

  <!-- Scripts filled by later tasks -->
  <script>
    // nav background on scroll
    const nav = document.getElementById('nav');
    const onScroll = () => {
      if (window.scrollY > 40) {
        nav.classList.add('bg-navy-deep/95', 'backdrop-blur', 'shadow-lg', 'shadow-black/20');
      } else {
        nav.classList.remove('bg-navy-deep/95', 'backdrop-blur', 'shadow-lg', 'shadow-black/20');
      }
    };
    window.addEventListener('scroll', onScroll);
    onScroll();
  </script>

</body>
</html>
```

- [ ] **Step 2: Verify in browser**

Open `index.html`. Expected: dark navy page, fixed nav bar at top with "STAHLNAHT" left and links right (desktop) / WhatsApp button (mobile ≤768px). On scroll the nav gains a navy background + blur. Links do nothing visible yet (empty sections) but don't error. Check at 375px and 1280px widths.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: base HTML, Tailwind config, fonts, fixed nav"
```

---

### Task 2: Hero section (fullscreen photo)

**Files:**
- Modify: `index.html` (fill `<header id="hero">`)

**Interfaces:**
- Consumes: Task 1 document, `assets/images/treppe-frontal.jpeg`.
- Produces: a full-viewport-height hero with the treppe-frontal photo as background, a dark navy gradient overlay for legibility, headline "PRÄZISION IN STAHL", subtitle, and two buttons: WhatsApp (primary, `https://wa.me/4917682137608`) and "Projekte ansehen" (secondary, anchor `#referenzen`).

- [ ] **Step 1: Fill the hero header**

Replace `<header id="hero"></header>` with:

```html
<header id="hero" class="relative min-h-screen flex items-center">
  <div class="absolute inset-0">
    <img src="assets/images/treppe-frontal.jpeg" alt="Stahltreppe mit Eichenstufen von Stahlnaht Metallbau"
         class="w-full h-full object-cover">
    <div class="absolute inset-0 bg-gradient-to-b from-navy-deep/60 via-navy-deep/70 to-navy-deep"></div>
  </div>
  <div class="relative max-w-6xl mx-auto px-5 w-full">
    <p class="font-head tracking-[0.3em] text-silver text-sm mb-4">STAHLNAHT · METALLBAU</p>
    <h1 class="font-head font-bold text-white text-5xl sm:text-6xl md:text-7xl leading-none">
      PRÄZISION<br>IN STAHL
    </h1>
    <p class="mt-6 text-lg text-silver max-w-md">
      Individuelle Stahlkonstruktionen, Geländer und Treppen —
      sauberes Handwerk mit Präzision aus der Luftfahrt. Speyer & Umgebung.
    </p>
    <div class="mt-8 flex flex-wrap gap-4">
      <a href="https://wa.me/4917682137608" target="_blank" rel="noopener"
         class="bg-silver text-navy-deep font-semibold px-6 py-3 rounded hover:bg-white transition">
        Per WhatsApp anfragen
      </a>
      <a href="#referenzen"
         class="border border-silver/50 text-silver font-semibold px-6 py-3 rounded hover:border-silver hover:text-white transition">
        Projekte ansehen
      </a>
    </div>
  </div>
</header>
```

- [ ] **Step 2: Verify in browser**

Reload. Expected: full-screen hero, treppe photo dimmed under a navy gradient, large "PRÄZISION IN STAHL" headline, subtitle, two buttons. WhatsApp button opens `wa.me` link; "Projekte ansehen" scrolls down. Text is readable over the photo at 375px and desktop. Bottom of hero fades into the navy background.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: fullscreen hero with real project photo"
```

---

### Task 3: Leistungen (services) section

**Files:**
- Modify: `index.html` (fill `<section id="leistungen">`)

**Interfaces:**
- Consumes: Task 1 document.
- Produces: a services section with a heading and 4 cards (Geländer & Treppen, Individuelle Stahlkonstruktionen, Schweißarbeiten, Metallbau & Reparaturen), each with an inline SVG icon, title, and short text. Cards use the `reveal` class for scroll fade-in (wired in Task 7). Responsive grid: 1 col mobile, 2 cols each on `sm`+.

- [ ] **Step 1: Fill the leistungen section**

Replace `<section id="leistungen"></section>` with:

```html
<section id="leistungen" class="py-24 px-5 bg-navy-deep">
  <div class="max-w-6xl mx-auto">
    <p class="font-head tracking-[0.3em] text-oak text-xs mb-3">LEISTUNGEN</p>
    <h2 class="font-head font-bold text-white text-3xl sm:text-4xl mb-12">Was wir bauen</h2>
    <div class="grid sm:grid-cols-2 gap-6">

      <div class="reveal bg-navy-light/40 border border-white/5 rounded-lg p-7 hover:border-oak/40 transition">
        <svg class="w-9 h-9 text-oak mb-4" fill="none" stroke="currentColor" stroke-width="1.5" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M3 21h18M6 21V9m4 12V9m4 12V9m4 12V9M4 9h16L12 3 4 9Z"/></svg>
        <h3 class="font-head font-semibold text-white text-xl mb-2">Geländer & Treppen</h3>
        <p class="text-silver/80 text-sm leading-relaxed">Innen- und Außentreppen, Geländer und Handläufe — filigran, stabil und exakt nach Maß gefertigt.</p>
      </div>

      <div class="reveal bg-navy-light/40 border border-white/5 rounded-lg p-7 hover:border-oak/40 transition">
        <svg class="w-9 h-9 text-oak mb-4" fill="none" stroke="currentColor" stroke-width="1.5" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M3 7l9-4 9 4v10l-9 4-9-4V7Z M3 7l9 4 9-4 M12 11v10"/></svg>
        <h3 class="font-head font-semibold text-white text-xl mb-2">Individuelle Stahlkonstruktionen</h3>
        <p class="text-silver/80 text-sm leading-relaxed">Von der Planung über die Fertigung bis zur Montage — maßgeschneiderte Konstruktionen nach Ihren Vorgaben.</p>
      </div>

      <div class="reveal bg-navy-light/40 border border-white/5 rounded-lg p-7 hover:border-oak/40 transition">
        <svg class="w-9 h-9 text-oak mb-4" fill="none" stroke="currentColor" stroke-width="1.5" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M13 3L4 14h7l-1 7 9-11h-7l1-7Z"/></svg>
        <h3 class="font-head font-semibold text-white text-xl mb-2">Schweißarbeiten</h3>
        <p class="text-silver/80 text-sm leading-relaxed">Saubere, langlebige Schweißnähte — als gelernter Schweißer und Industriemeister mit Anspruch an höchste Qualität.</p>
      </div>

      <div class="reveal bg-navy-light/40 border border-white/5 rounded-lg p-7 hover:border-oak/40 transition">
        <svg class="w-9 h-9 text-oak mb-4" fill="none" stroke="currentColor" stroke-width="1.5" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M14.7 6.3a4 4 0 00-5.4 5.4L3 18v3h3l6.3-6.3a4 4 0 005.4-5.4l-2.5 2.5-2-2 2.5-2.5Z"/></svg>
        <h3 class="font-head font-semibold text-white text-xl mb-2">Metallbau & Reparaturen</h3>
        <p class="text-silver/80 text-sm leading-relaxed">Allgemeine Metallbauarbeiten, Anpassungen und Reparaturen — zuverlässig und termintreu ausgeführt.</p>
      </div>

    </div>
  </div>
</section>
```

- [ ] **Step 2: Verify in browser**

Reload, click "Leistungen" in nav. Expected: 4 service cards, oak-colored icons, readable. Single column at 375px, two columns at ≥640px. Hover shows oak border. Cards may appear invisible until Task 7 wires fade-in — if so, temporarily confirm content by removing `reveal` class mentally (it's still in the DOM). Note: `reveal` elements start at opacity 0; they become visible once Task 7's observer runs. For this task's verification, confirm layout by inspecting or by temporarily commenting the `.reveal{opacity:0}` rule if needed, then restore.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: services section with four cards"
```

---

### Task 4: Über Timo (about) section

**Files:**
- Modify: `index.html` (fill `<section id="ueber">`)

**Interfaces:**
- Consumes: Task 1 document.
- Produces: a two-column (stacked on mobile) about section. Left: portrait placeholder (styled box saying "Foto folgt"). Right: the story text (grew up hands-on → aircraft mechanic training → self-taught Industriemeister & welder → self-employed) plus a small stat/qualification row. Uses `reveal`.

- [ ] **Step 1: Fill the ueber section**

Replace `<section id="ueber"></section>` with:

```html
<section id="ueber" class="py-24 px-5 bg-navy-light/20">
  <div class="max-w-6xl mx-auto grid md:grid-cols-2 gap-12 items-center">

    <div class="reveal">
      <div class="aspect-[4/5] rounded-lg bg-navy-light border border-white/10 flex flex-col items-center justify-center text-center p-8">
        <svg class="w-12 h-12 text-silver/40 mb-3" fill="none" stroke="currentColor" stroke-width="1.5" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M15.75 6a3.75 3.75 0 11-7.5 0 3.75 3.75 0 017.5 0zM4.5 20.25a7.5 7.5 0 0115 0v.75H4.5v-.75z"/></svg>
        <p class="text-silver/50 text-sm">Foto von Timo folgt</p>
      </div>
    </div>

    <div class="reveal">
      <p class="font-head tracking-[0.3em] text-oak text-xs mb-3">ÜBER TIMO BETZ</p>
      <h2 class="font-head font-bold text-white text-3xl sm:text-4xl mb-6">Handwerk im Blut, Präzision aus der Luftfahrt</h2>
      <div class="space-y-4 text-silver/85 leading-relaxed">
        <p>Timo Betz ist handwerklich aufgewachsen — schon immer hat er geschraubt, gebaut und Dinge selbst in die Hand genommen. Aus dieser Leidenschaft wurde ein Beruf.</p>
        <p>Nach seiner Ausbildung zum <strong class="text-white">Fluggerätemechaniker</strong> arbeitete er als Schweißer und bildete sich autodidaktisch zum <strong class="text-white">Industriemeister</strong> weiter. Die Präzision und die Qualitätsstandards aus der Luftfahrt bringt er heute in jedes Metallbau-Projekt ein.</p>
        <p>Mit <strong class="text-white">Stahlnaht Metallbau</strong> hat er sich diesen Anspruch selbständig gemacht: sauberes Handwerk, Zuverlässigkeit und langlebige Lösungen.</p>
      </div>
      <div class="mt-8 flex flex-wrap gap-8">
        <div>
          <div class="font-head font-bold text-oak text-2xl">Fluggerät-</div>
          <div class="text-silver/60 text-sm">mechaniker (Ausbildung)</div>
        </div>
        <div>
          <div class="font-head font-bold text-oak text-2xl">Industrie-</div>
          <div class="text-silver/60 text-sm">meister</div>
        </div>
        <div>
          <div class="font-head font-bold text-oak text-2xl">Speyer</div>
          <div class="text-silver/60 text-sm">& Umgebung</div>
        </div>
      </div>
    </div>

  </div>
</section>
```

- [ ] **Step 2: Verify in browser**

Reload, click "Über Timo". Expected: on desktop, portrait placeholder box left + story text right; stacked on mobile (375px) with placeholder on top. Qualification row wraps cleanly. Text readable, "Fluggerätemechaniker" and "Industriemeister" bolded white.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: about section with Timo's story and portrait placeholder"
```

---

### Task 5: Referenzen (gallery) section with lightbox

**Files:**
- Modify: `index.html` (fill `<section id="referenzen">` and add lightbox markup + JS)

**Interfaces:**
- Consumes: Task 1 document; `assets/images/treppe-frontal.jpeg`, `treppe-galerie.jpeg`, `treppe-detail.jpeg`.
- Produces: a responsive gallery grid of the 3 real photos (each clickable), plus a lightbox overlay (`#lightbox`) with an `<img id="lightbox-img">` and a close button. Defines JS function `openLightbox(src, alt)` and closes on overlay/button click or Escape. New photos are added by copying one `<button class="gallery-item">` block. Uses `reveal`.

- [ ] **Step 1: Fill the referenzen section**

Replace `<section id="referenzen"></section>` with:

```html
<section id="referenzen" class="py-24 px-5 bg-navy-deep">
  <div class="max-w-6xl mx-auto">
    <p class="font-head tracking-[0.3em] text-oak text-xs mb-3">REFERENZEN</p>
    <h2 class="font-head font-bold text-white text-3xl sm:text-4xl mb-3">Ausgewählte Projekte</h2>
    <p class="text-silver/70 mb-12 max-w-lg">Eine maßgefertigte Stahltreppe mit Eichenstufen und filigranem Geländer — geplant, gefertigt und montiert von Stahlnaht.</p>

    <!-- To add a new project: copy one button block below and change src/alt. -->
    <div class="grid sm:grid-cols-2 lg:grid-cols-3 gap-4">
      <button type="button" onclick="openLightbox('assets/images/treppe-frontal.jpeg','Stahltreppe frontal')"
              class="reveal gallery-item group relative overflow-hidden rounded-lg aspect-[3/4] bg-navy-light">
        <img src="assets/images/treppe-frontal.jpeg" alt="Stahltreppe frontal" class="w-full h-full object-cover transition duration-500 group-hover:scale-105">
        <span class="absolute inset-0 bg-navy-deep/0 group-hover:bg-navy-deep/20 transition"></span>
      </button>

      <button type="button" onclick="openLightbox('assets/images/treppe-galerie.jpeg','Stahltreppe mit Galerie')"
              class="reveal gallery-item group relative overflow-hidden rounded-lg aspect-[3/4] bg-navy-light">
        <img src="assets/images/treppe-galerie.jpeg" alt="Stahltreppe mit Galerie" class="w-full h-full object-cover transition duration-500 group-hover:scale-105">
        <span class="absolute inset-0 bg-navy-deep/0 group-hover:bg-navy-deep/20 transition"></span>
      </button>

      <button type="button" onclick="openLightbox('assets/images/treppe-detail.jpeg','Detail der Stahltreppe')"
              class="reveal gallery-item group relative overflow-hidden rounded-lg aspect-[3/4] bg-navy-light">
        <img src="assets/images/treppe-detail.jpeg" alt="Detail der Stahltreppe" class="w-full h-full object-cover transition duration-500 group-hover:scale-105">
        <span class="absolute inset-0 bg-navy-deep/0 group-hover:bg-navy-deep/20 transition"></span>
      </button>
    </div>
  </div>
</section>

<!-- LIGHTBOX -->
<div id="lightbox" class="fixed inset-0 z-[60] hidden items-center justify-center bg-black/90 p-4">
  <button type="button" onclick="closeLightbox()" aria-label="Schließen"
          class="absolute top-5 right-6 text-white/80 hover:text-white text-4xl leading-none">&times;</button>
  <img id="lightbox-img" src="" alt="" class="max-h-[90vh] max-w-full rounded-lg object-contain">
</div>
```

- [ ] **Step 2: Add lightbox JS**

Inside the existing `<script>` at the bottom (after the nav scroll code), add:

```javascript
    // lightbox
    const lightbox = document.getElementById('lightbox');
    const lightboxImg = document.getElementById('lightbox-img');
    function openLightbox(src, alt) {
      lightboxImg.src = src;
      lightboxImg.alt = alt || '';
      lightbox.classList.remove('hidden');
      lightbox.classList.add('flex');
      document.body.style.overflow = 'hidden';
    }
    function closeLightbox() {
      lightbox.classList.add('hidden');
      lightbox.classList.remove('flex');
      lightboxImg.src = '';
      document.body.style.overflow = '';
    }
    lightbox.addEventListener('click', (e) => { if (e.target === lightbox) closeLightbox(); });
    document.addEventListener('keydown', (e) => { if (e.key === 'Escape') closeLightbox(); });
```

- [ ] **Step 3: Verify in browser**

Reload, click "Referenzen". Expected: 3 project photos in a grid (1 col at 375px, 2 at 640px, 3 at 1024px). Hover zooms the image slightly. Clicking a photo opens a full-screen dark lightbox with the large image; clicking the × , the backdrop, or pressing Escape closes it and restores scrolling.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: gallery section with lightbox"
```

---

### Task 6: Kontakt section, fixed WhatsApp button, and footer

**Files:**
- Modify: `index.html` (fill `<section id="kontakt">`, `<footer id="footer">`, add fixed WhatsApp FAB)

**Interfaces:**
- Consumes: Task 1 document; real contact data from Global Constraints; `assets/images/logo.jpeg`.
- Produces: a contact section with large direct-action buttons (WhatsApp, Telegram, Anrufen, E-Mail, LinkedIn, Instagram-placeholder); a fixed WhatsApp floating action button (FAB) bottom-right; and a footer with logo, copyright, and Impressum/Datenschutz placeholder links (anchors to `#impressum`, `#datenschutz` — content stubs included).

- [ ] **Step 1: Fill the kontakt section**

Replace `<section id="kontakt"></section>` with:

```html
<section id="kontakt" class="py-24 px-5 bg-navy-light/20">
  <div class="max-w-3xl mx-auto text-center">
    <p class="font-head tracking-[0.3em] text-oak text-xs mb-3">KONTAKT</p>
    <h2 class="font-head font-bold text-white text-3xl sm:text-4xl mb-4">Projekt anfragen</h2>
    <p class="text-silver/80 mb-10">Schnell und unkompliziert — am liebsten direkt per WhatsApp oder Telegram. Region Speyer & Umgebung.</p>

    <div class="grid sm:grid-cols-2 gap-4 text-left">
      <a href="https://wa.me/4917682137608" target="_blank" rel="noopener"
         class="flex items-center gap-4 bg-navy-light/50 border border-white/10 hover:border-oak/50 rounded-lg px-5 py-4 transition">
        <span class="text-2xl">💬</span>
        <span><span class="block text-white font-semibold">WhatsApp</span><span class="block text-silver/60 text-sm">+49 176 82137608</span></span>
      </a>
      <a href="https://t.me/Timobetz" target="_blank" rel="noopener"
         class="flex items-center gap-4 bg-navy-light/50 border border-white/10 hover:border-oak/50 rounded-lg px-5 py-4 transition">
        <span class="text-2xl">✈️</span>
        <span><span class="block text-white font-semibold">Telegram</span><span class="block text-silver/60 text-sm">@Timobetz</span></span>
      </a>
      <a href="tel:+4917682137608"
         class="flex items-center gap-4 bg-navy-light/50 border border-white/10 hover:border-oak/50 rounded-lg px-5 py-4 transition">
        <span class="text-2xl">📞</span>
        <span><span class="block text-white font-semibold">Anrufen</span><span class="block text-silver/60 text-sm">+49 176 82137608</span></span>
      </a>
      <a href="mailto:info@stahlnaht.de"
         class="flex items-center gap-4 bg-navy-light/50 border border-white/10 hover:border-oak/50 rounded-lg px-5 py-4 transition">
        <span class="text-2xl">✉️</span>
        <span><span class="block text-white font-semibold">E-Mail</span><span class="block text-silver/60 text-sm">info@stahlnaht.de</span></span>
      </a>
      <a href="https://www.linkedin.com/in/timo-betz-59357b321" target="_blank" rel="noopener"
         class="flex items-center gap-4 bg-navy-light/50 border border-white/10 hover:border-oak/50 rounded-lg px-5 py-4 transition">
        <span class="text-2xl">in</span>
        <span><span class="block text-white font-semibold">LinkedIn</span><span class="block text-silver/60 text-sm">Timo Betz</span></span>
      </a>
      <span
         class="flex items-center gap-4 bg-navy-light/30 border border-dashed border-white/10 rounded-lg px-5 py-4 opacity-60">
        <span class="text-2xl">📷</span>
        <span><span class="block text-white font-semibold">Instagram</span><span class="block text-silver/60 text-sm">folgt in Kürze</span></span>
      </span>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Fill the footer and add the fixed WhatsApp FAB**

Replace `<footer id="footer"></footer>` with:

```html
<footer id="footer" class="bg-navy-deep border-t border-white/10 px-5 py-12">
  <div class="max-w-6xl mx-auto flex flex-col sm:flex-row items-center justify-between gap-6 text-center sm:text-left">
    <div class="flex items-center gap-3">
      <img src="assets/images/logo.jpeg" alt="Stahlnaht Metallbau Logo" class="h-12 w-auto rounded">
      <div>
        <div class="font-head font-bold text-white tracking-wider">STAHLNAHT METALLBAU</div>
        <div class="text-silver/50 text-sm">Timo Betz · Speyer & Umgebung</div>
      </div>
    </div>
    <div class="text-silver/50 text-sm">
      <a href="#impressum" class="hover:text-white transition">Impressum</a>
      <span class="mx-2">·</span>
      <a href="#datenschutz" class="hover:text-white transition">Datenschutz</a>
      <div class="mt-2">© 2026 Stahlnaht Metallbau</div>
    </div>
  </div>

  <!-- Legal stubs: Timo must complete before going live -->
  <div id="impressum" class="max-w-6xl mx-auto mt-12 pt-8 border-t border-white/5 text-silver/50 text-xs leading-relaxed">
    <h3 class="font-head text-silver text-sm mb-2">Impressum</h3>
    <p>Angaben gemäß § 5 DDG:<br>
    Timo Betz — Stahlnaht Metallbau<br>
    [Straße und Hausnummer] · [PLZ] Speyer<br>
    Telefon: +49 176 82137608 · E-Mail: info@stahlnaht.de<br>
    [Ggf. USt-IdNr. ergänzen]</p>
    <p class="mt-2 text-oak/70">⚠️ Platzhalter — vor dem Live-Gang mit echten Daten ausfüllen.</p>
  </div>
  <div id="datenschutz" class="max-w-6xl mx-auto mt-6 text-silver/50 text-xs leading-relaxed">
    <h3 class="font-head text-silver text-sm mb-2">Datenschutz</h3>
    <p>Diese Website erhebt keine personenbezogenen Daten, setzt keine Cookies und nutzt kein Tracking. Beim Klick auf externe Links (WhatsApp, Telegram, LinkedIn) gelten die Datenschutzbestimmungen der jeweiligen Anbieter.</p>
    <p class="mt-2 text-oak/70">⚠️ Platzhalter — vor dem Live-Gang prüfen/ergänzen.</p>
  </div>
</footer>

<!-- Fixed WhatsApp FAB -->
<a href="https://wa.me/4917682137608" target="_blank" rel="noopener" aria-label="WhatsApp"
   class="fixed bottom-5 right-5 z-50 bg-[#25D366] text-white rounded-full w-14 h-14 flex items-center justify-center shadow-lg shadow-black/30 hover:scale-105 transition">
  <svg class="w-7 h-7" fill="currentColor" viewBox="0 0 24 24"><path d="M.057 24l1.687-6.163a11.867 11.867 0 01-1.587-5.945C.16 5.335 5.495 0 12.05 0a11.82 11.82 0 018.413 3.488 11.82 11.82 0 013.48 8.414c-.003 6.557-5.338 11.892-11.893 11.892a11.9 11.9 0 01-5.688-1.448L.057 24zm6.597-3.807c1.676.995 3.276 1.591 5.392 1.592 5.448 0 9.886-4.434 9.889-9.885.002-5.462-4.415-9.89-9.881-9.892-5.452 0-9.887 4.434-9.889 9.884a9.86 9.86 0 001.51 5.26l-.999 3.648 3.738-.981zm11.387-5.464c-.074-.124-.272-.198-.57-.347-.297-.149-1.758-.868-2.031-.967-.272-.099-.47-.149-.669.149-.198.297-.767.967-.94 1.165-.173.198-.347.223-.644.074-.297-.149-1.255-.462-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.297-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.074-.148-.669-1.611-.916-2.206-.242-.579-.487-.5-.669-.51l-.57-.01c-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.095 3.2 5.076 4.487.709.306 1.263.489 1.694.626.712.226 1.36.194 1.872.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.29.173-1.414z"/></svg>
</a>
```

- [ ] **Step 3: Verify in browser**

Reload, click "Kontakt". Expected: 6 contact tiles (WhatsApp, Telegram, Anrufen, E-Mail, LinkedIn active; Instagram dashed/dimmed "folgt in Kürze"). Each active tile opens the correct app/link. Footer shows logo + name + Impressum/Datenschutz links that scroll to the legal stubs (with ⚠️ placeholder notes). A green WhatsApp FAB sits bottom-right on every scroll position and opens wa.me. Check tiles stack to 1 column at 375px.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: contact section, WhatsApp FAB, footer with legal stubs"
```

---

### Task 7: Scroll fade-in animations (reveal)

**Files:**
- Modify: `index.html` (add IntersectionObserver JS)

**Interfaces:**
- Consumes: all `.reveal` elements added in Tasks 3–6.
- Produces: JS that adds the `visible` class to each `.reveal` element as it scrolls into view, triggering the CSS fade-in defined in Task 1.

- [ ] **Step 1: Add the IntersectionObserver JS**

Inside the bottom `<script>` (after the lightbox code), add:

```javascript
    // scroll reveal
    const revealObserver = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible');
          revealObserver.unobserve(entry.target);
        }
      });
    }, { threshold: 0.15 });
    document.querySelectorAll('.reveal').forEach((el) => revealObserver.observe(el));
```

- [ ] **Step 2: Verify in browser**

Reload and scroll slowly from top to bottom. Expected: service cards, about columns, and gallery items fade + slide up into view as they enter the viewport (not all at once). Elements already in view on load appear immediately. Nothing stays invisible. Confirm at 375px and desktop.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: scroll fade-in animations via IntersectionObserver"
```

---

### Task 8: Final polish, image optimization note, and README

**Files:**
- Create: `README.md`
- Modify: `index.html` (add `loading="lazy"` to non-hero images, verify meta/OG tags)

**Interfaces:**
- Consumes: complete site from Tasks 1–7.
- Produces: lazy-loading on gallery/footer images, Open Graph tags for link sharing, and a `README.md` documenting how to run locally, how to deploy (GitHub Pages / Netlify), how to add gallery photos, and what placeholders Timo must fill.

- [ ] **Step 1: Add `loading="lazy"` to gallery + footer images**

In each gallery `<img>` (Task 5) and the footer logo `<img>` (Task 6), add `loading="lazy"`. Do NOT add it to the hero image (Task 2) — it must load immediately. Example gallery img becomes:

```html
<img src="assets/images/treppe-frontal.jpeg" alt="Stahltreppe frontal" loading="lazy" class="w-full h-full object-cover transition duration-500 group-hover:scale-105">
```

- [ ] **Step 2: Add Open Graph tags to `<head>`**

After the `<meta name="description">` line in `<head>`, add:

```html
  <meta property="og:title" content="Stahlnaht Metallbau — Timo Betz | Speyer">
  <meta property="og:description" content="Individuelle Stahlkonstruktionen, Geländer und Treppen mit Präzision. Speyer & Umgebung.">
  <meta property="og:type" content="website">
  <meta property="og:image" content="assets/images/treppe-frontal.jpeg">
  <meta property="og:locale" content="de_DE">
```

- [ ] **Step 3: Create `README.md`**

```markdown
# Stahlnaht Metallbau — Website

Statische Onepage-Website für Timo Betz / Stahlnaht Metallbau.

## Lokal ansehen
Die Datei `index.html` einfach im Browser öffnen (Doppelklick). Kein Build nötig.

## Aufbau
- `index.html` — die komplette Website (HTML, Tailwind via CDN, JS inline)
- `assets/images/` — Logo und Projektfotos
- `docs/superpowers/` — Spec & Plan

## Neue Projektfotos hinzufügen
1. Foto nach `assets/images/` kopieren (z. B. `projekt2.jpeg`).
2. In `index.html` im Abschnitt `#referenzen` einen der `<button class="gallery-item">`-Blöcke kopieren.
3. In der Kopie `src`, `onclick` und `alt` auf das neue Foto ändern.

## Deployment (kostenlos)
**GitHub Pages:** Repo anlegen, Dateien pushen, in den Repo-Settings unter „Pages" den Branch als Quelle wählen.
**Netlify:** Ordner per Drag & Drop auf app.netlify.com/drop ziehen.

## ⚠️ Vor dem Live-Gang von Timo auszufüllen
- **Impressum** (`#impressum`): echte Anschrift, ggf. USt-IdNr.
- **Datenschutz** (`#datenschutz`): prüfen/ergänzen.
- **Portrait-Foto** im Abschnitt „Über Timo" einsetzen.
- **Instagram-Link** ergänzen, sobald vorhanden.
```

- [ ] **Step 4: Verify**

Open `index.html` and DevTools Network tab. Expected: hero image loads immediately; gallery/footer images load lazily as you scroll. Paste the site URL into a link-preview tester (or check `<head>`) to confirm OG tags present. `README.md` renders correctly. Full page still works end-to-end at 375px and desktop: nav, hero, services, about, gallery+lightbox, contact links, FAB, fade-ins.

- [ ] **Step 5: Commit**

```bash
git add index.html README.md
git commit -m "chore: lazy-load images, OG tags, README with deploy + edit guide"
```

---

## Self-Review

**1. Spec coverage:**
- Technischer Rahmen (single file, Tailwind CDN, fonts, vanilla JS) → Task 1 ✓
- Design-Richtung (Navy/Silber/Eiche, Oswald/Inter) → Task 1 config + used throughout ✓
- Hero (fullscreen photo, headline, WhatsApp + Projekte buttons) → Task 2 ✓
- Leistungen (4 cards) → Task 3 ✓
- Über Timo (story, portrait placeholder) → Task 4 ✓
- Referenzen/Galerie (real photos, lightbox, extensible) → Task 5 ✓
- Kontakt (direct buttons, exact links, no form) → Task 6 ✓
- Footer (logo, copyright, Impressum/Datenschutz) → Task 6 ✓
- Fixed WhatsApp button → Task 6 ✓
- Scroll fade-ins → Task 7 ✓
- Nav transparent→navy on scroll → Task 1 ✓
- Legal placeholders → Task 6 stubs + Task 8 README note ✓
- Mobile-first / responsive → verified every task at 375px ✓
- SEO/sharing polish + edit guide → Task 8 ✓

**2. Placeholder scan:** No "TBD/implement later". Legal/Instagram/portrait placeholders are intentional content stubs, clearly marked as Timo's to complete — not plan gaps. All code shown in full.

**3. Type/name consistency:** `openLightbox(src, alt)` / `closeLightbox()` defined in Task 5, referenced by gallery buttons in Task 5 ✓. `.reveal`/`.visible` defined Task 1, applied Tasks 3–6, observed Task 7 ✓. `#nav`, section IDs (`#leistungen`, `#ueber`, `#referenzen`, `#kontakt`, `#impressum`, `#datenschutz`) consistent between nav/footer links and section definitions ✓. Color/font names (`navy`, `silver`, `oak`, `font-head`) defined in Task 1 config, used consistently ✓.
