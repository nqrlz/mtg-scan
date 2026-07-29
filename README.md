# Magic → Cardmarket (statische Web-App)

Zwei eigenständige HTML-Seiten — **kein Server, keine Abhängigkeiten, keine Kosten**, deckt **ganz Magic** ab.
Preise/Erkennung laufen live über die CORS-fähige Scryfall-API direkt im Browser.

## [index.html](index.html) — Foto-Scan (Kernfunktion)
Je **ein Foto pro Karte** (mehrere gleichzeitig auswählbar). Ablauf:
1. Karten fotografieren (eine pro Bild, Name gut sichtbar).
2. Alle Fotos in `scan.html` laden. **Set-Feld** ausfüllen und pro Durchgang nur Karten *aus diesem Set*
   scannen → exakter Druck & Preis. **Foils**-Haken für Foil-Batches.
3. OCR liest die Namen im Browser (`tesseract.js`), Scryfall liefert Druck & Preis.
   Nicht/falsch gelesene Namen unten **antippen und tippen** (Enter → neu gesucht).
4. Prüfen, **Cardmarket-CSV herunterladen** → mit der `cardmarket-bulk-import`-Erweiterung einstellen.

**Ehrliche Grenzen der leichten OCR:** ~6–7 von 9 Namen werden automatisch erkannt; dunkle Karten
(wenig Kontrast im Namen) und OCR-Aussetzer korrigierst du in Sekunden über das editierbare Namensfeld.
Der **Name allein bestimmt den Preis nicht** — deshalb pro Batch das Set angeben (sortenrein scannen),
sonst greift die Fuzzy-Suche irgendeinen Druck.

## [import.html](import.html) — CSV-Import (Alternative, ohne Foto)
Falls du eine Sammlung schon in **ManaBox / Delver Lens** gescannt hast: deren CSV-Export hier laden
(enthält Scryfall-IDs → exakte Drucke). Preis-Filter + Cardmarket-CSV wie bei scan.html, aber ohne OCR
und ohne die Set-/Erkennungs-Unsicherheit.

## Ausliefern (gratis, ohne Kreditkarte)
- **Lokal:** Datei im Browser öffnen (Scryfall erlaubt CORS; tesseract.js lädt von CDN → Internet nötig).
- **Für Freunde:** die Dateien auf einen Static-Host legen — **Netlify Drop** (Ordner reinziehen → URL),
  **Cloudflare Pages** oder **GitHub Pages**. Keine Server-Wartung, keine laufenden Kosten.

## Ausgabe-CSV (verifiziertes Extension-Format)
`Name, Set code, Foil, Rarity, Quantity, Purchase price, Condition, Language, Comment(#Nr)` ·
Preis = `max(1,00 €, Scryfall-Preis)` (Foil → `eur_foil`) · nur Karten ab 1 €.
