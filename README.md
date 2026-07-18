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
