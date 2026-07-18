# Stahlnaht Metallbau — Website Design-Spec

**Datum:** 2026-07-18
**Kunde:** Timo Betz — Stahlnaht Metallbau, Speyer & Umgebung
**Erstellt von:** Freund/Ersteller der Website

---

## 1. Ziel & Zweck

Eine professionelle Onepage-Website, die Timo Betz und sein Unternehmen **Stahlnaht Metallbau** vorstellt. Sie soll:

- **Referenzen zeigen** — echte Projektbilder (Portfolio/Galerie)
- **Vertrauen aufbauen** — seriöse Online-Präsenz, die man „googeln" kann
- **Zur Kontaktaufnahme bewegen** — über **Social Media, WhatsApp, Telegram** (KEIN Kontaktformular)

Zielgruppe: Privat- und Gewerbekunden in Speyer und Umgebung, die individuellen Metallbau (Geländer, Treppen, Stahlkonstruktionen) suchen.

## 2. Technischer Rahmen

- **Eine einzige `index.html`** — statisch, kein Build-Schritt, doppelklickbar.
- **Tailwind CSS via CDN** — mobile-first, responsiv auf Handy/Tablet/Desktop.
- **Google Fonts** via CDN (Oswald/Archivo für Headlines, Inter für Fließtext).
- **Vanilla JavaScript** (minimal) für Scroll-Effekte, Lightbox, Nav-Verhalten.
- **Keine Abhängigkeiten / kein Framework / kein Backend.**
- Bilder in `assets/images/`.
- **Hosting:** kostenlos über GitHub Pages / Netlify (statisch). 0 € laufende Kosten, keine Wartung.

**Begründung (Recherche):** Für eine Visitenkarte-/Portfolio-Seite ohne täglichen Content-Bedarf ist statisch + Tailwind die beste Wahl: schnellste Ladezeit, beste Mobil-Darstellung, volle Design-Kontrolle, kostenlos, wartungsarm. WordPress/Wix wären nur sinnvoll, wenn Timo selbst regelmäßig ohne Hilfe pflegen wollte — was für den Start nicht der Fall ist.

## 3. Design-Richtung: „Präzision in Stahl"

Abgeleitet aus dem Logo (Silber-Metallverlauf auf tiefem Navy) und den echten Projektbildern (schwarzer Stahl + helle Eiche).

**Farben:**
- Hintergrund: tiefes Navy/Indigo (`#14142a` – `#1a1a2e`)
- Text/Akzent: Silber-Grau (`#c0c6d8`), Weiß für Headlines
- Sekundär-Akzent: warmes Eiche-Holz (`~#c9a56a`) — aus den Projektbildern

**Typografie:**
- Headlines: kräftige breite Versalien (Oswald oder Archivo — passt zum Logo-Schriftzug)
- Fließtext: ruhige, gut lesbare Sans (Inter)

**Kernbotschaft:** Timo baut keine groben Zäune — er baut Design-Objekte aus Stahl mit Präzision aus der Luftfahrt.

## 4. Seitenstruktur (Onepage, von oben nach unten)

Fixierte, transparente Navigation oben (wird beim Scrollen dezent navy), springt zu den Ankern.

1. **Hero** — Vollbild-Treppenfoto mit Overlay. Headline „PRÄZISION IN STAHL", Untertitel „Metallbau · Geländer · Treppen — Speyer & Umgebung". Zwei Buttons: WhatsApp (primär) + „Projekte ansehen" (scrollt zur Galerie).
2. **Leistungen** — 4 Kacheln: *Geländer & Treppen*, *Individuelle Stahlkonstruktionen*, *Schweißarbeiten*, *Metallbau & Reparaturen*. Jeweils Icon + kurzer Text.
3. **Über Timo** — Story: handwerklich aufgewachsen → Ausbildung Fluggerätemechaniker → Industriemeister (autodidaktisch) & Schweißer → Selbständigkeit. Betont Präzision/Zuverlässigkeit/langlebige Lösungen. Portrait-Platzhalter (echtes Foto kommt später von Timo).
4. **Referenzen / Galerie** — echte Treppen-Fotos, groß, im Raster, anklickbar (Lightbox). So gebaut, dass neue Projekte einfach ergänzt werden können.
5. **Kontakt** — KEIN Formular. Große Direkt-Buttons: **WhatsApp**, **Telegram**, **E-Mail**, **LinkedIn**. Hinweis Region Speyer & Umgebung.
6. **Footer** — Logo, © Stahlnaht Metallbau · Timo Betz, Links zu Impressum & Datenschutz.

## 5. Interaktion & Feinschliff

- Fixierte Navigation, beim Scrollen navy werdend
- Dezente Fade-in-Animationen beim Scrollen (IntersectionObserver)
- Galerie-Lightbox (Klick → Bild groß)
- **Fixierter WhatsApp-Button** unten rechts (mobil immer griffbereit)
- Voll responsiv, mobile-first, große Touch-Flächen, formatfüllende Fotos auf Mobil
- Smooth-Scroll für Anker-Links

## 6. Rechtliches (Deutschland)

- **Impressum** (Pflicht nach § 5 DDG): eigener Bereich/Seite mit Platzhaltern für Name, Anschrift, Kontakt, ggf. USt-IdNr. — von Timo mit echten Daten zu füllen.
- **Datenschutzerklärung**: Platzhalter-Bereich. Da keine Formulare/Cookies/Tracking → minimal, aber vorhanden.
- Hinweis im Dokument, dass Timo diese Angaben vor dem Live-Gang vervollständigen/prüfen muss.

## 7. Inhalte & Assets

**Vorhanden:**
- Logo (`assets/images/logo.jpeg`) — Silber auf Navy
- 3 echte Projektfotos der Loft-Treppe (`treppe-detail`, `treppe-frontal`, `treppe-galerie`)
- LinkedIn-Profil von Timo

**Platzhalter (später von Timo zu liefern):**
- Portrait-Foto
- WhatsApp-Nummer, Telegram-Handle, E-Mail-Adresse
- Impressums-/Datenschutz-Angaben
- Weitere Projektfotos

## 8. Bewusst NICHT im Scope (YAGNI)

- Kein Kontaktformular (Kontakt läuft über Direkt-Kanäle)
- Kein Backend, keine Datenbank
- Kein Blog / Shop / Buchungssystem
- Kein Build-Tooling / npm
- Kein Mehrsprachen-Support (nur Deutsch)
- Kein Cookie-Banner (kein Tracking eingebaut)

## 9. Erfolgskriterien

- Sieht auf Handy, Tablet und Desktop sauber und professionell aus
- Lädt schnell (statisch, optimierte Bilder)
- Besucher findet in <5 Sek., was Timo macht und wie man ihn erreicht
- Galerie zeigt echte Arbeiten überzeugend
- Neue Projektbilder lassen sich ohne Technikwissen ergänzen (klare Struktur/Kommentare)
