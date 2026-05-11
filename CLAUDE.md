# Stundensatzkalkulation – a sEpp-App

## Übersicht

Reine HTML/CSS/JS-App zur Berechnung von Stundensätzen, Deckungsbeiträgen und Gehaltskorridoren. Keine externen Abhängigkeiten, kein Server.

- **Live-URL:** `https://seppofaz.github.io/Stundensatzkalkulation/`
- **GitHub-Repo:** `https://github.com/sEppofaz/Stundensatzkalkulation`
- **Lokaler Ordner:** `~/Dropbox/Apps/Claude/Stundensatzkalkulation/`
- **Quelldatei Excel:** `Stundensatzkalkulation V1.0.xlsx` (Referenz, wird nicht deployed)

## Deployment

```bash
cd ~/Library/CloudStorage/Dropbox/Apps/Claude/Stundensatzkalkulation
git add index.html
git commit -m "Beschreibung der Änderung"
git push
```
→ GitHub Pages deployed automatisch. Alle Nutzer sehen die neue Version beim nächsten Öffnen der URL.

## PWA / Homescreen-Icon

- `manifest.json`: short_name „hrs calc", theme_color `#1e40af`, display standalone
- Icons: `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` (180px), `favicon.png` (32px)
- Generiert mit Pillow (Python) – Blau-Gradient, €/h-Symbol, abgerundete Ecken
- Bei Icon-Änderung: Python-Script neu ausführen, alle 4 PNGs mit committen

## Reiter & Berechnungslogik

| Reiter | Eingaben (gelb) | Ergebnis (grün) | Kernformel |
|--------|----------------|-----------------|------------|
| **hrs calc** | AR%, Stunden/Tag | Arbeitsstunden/Jahr | 261 × AR% × hrs |
| **Salary → Bill Rate** | Gehalt, NWL%, Overhead%, CM%, Stunden | Stundensatz €/h | Gehalt×(1+NWL%)×(1+OH%) ÷ (1−CM%) ÷ Stunden |
| **Salary → CM%** | Gehalt, NWL%, Overhead%, Stunden, Bill Rate | CM% | 1 − (Kostenrate ÷ Bill Rate) |
| **Bill Rate → Salary** | Bill Rate €/$, NWL%, CM% min/max, Stunden, Währung | Jahres-/Monatsgehalt min/max | Bill Rate×(1−CM%) ÷ (1+NWL%)×Stunden |
| **Work Package** | Rollengehälter, FTE/Jahr, Inflation, Laufzeit, Gesamtstunden | Mixed Rate, Angebotswert, Rabatt/Runde | Gewichteter Ø-Stundensatz×Inflation→Ø aller Jahre×Gesamtstunden |

## Persistenz

- `localStorage` Key: `ssk_v1` – speichert alle Eingabefelder + aktiven Tab + FTE-State
- Beim Öffnen wird der gespeicherte Stand wiederhergestellt
- Reset-Button in der Info-Seite löscht localStorage und lädt neu

## Farbcodierung

- **Gelb** (`#fffde7`): Eingabefeld – editierbar, wird gespeichert
- **Grün** (`#dcfce7` / `#f0fdf4`): Berechnetes Ergebnis – read-only
- **Grau** (`#f8fafc`): Fixer Referenzwert

## Kennzahlen (Standardwerte aus Excel)

| Kennzahl | Standardwert | Herkunft |
|----------|-------------|---------|
| NWL% | 21,5 % | AG-Lohnnebenkosten Deutschland |
| AR% | 77 % | Ø letzter 5 Jahre ~76 % |
| Stunden/Jahr (40h) | 1.608 h | 261 Arbeitstage × 8 h × ~77 % |
| Stunden/Jahr (35h) | 1.407 h | 261 Arbeitstage × 7 h × ~77 % |
| CM% Ziel | 25–30 % | projektüblich |
| Inflation p.a. | 2,5 % | Work Package Default |

## PDF-Export

- **Button:** „⬇ PDF" ganz rechts in der Tab-Leiste (neben ℹ️ Info)
- **Funktion:** `window.print()` – öffnet Browser-Print-Dialog → „Als PDF speichern"
- **Print-CSS (`@media print`):** Tab-Leiste + PDF-Button ausgeblendet; nur aktiver Tab sichtbar; Hintergrundfarben erhalten (`print-color-adjust: exact`); Schatten entfernt
- **Keine externe Bibliothek** – rein browser-nativ

## Work Package – Besonderheiten

- **BCC Romania** hat keinen Gehaltseintrag – direkter Stundensatz (Default: 27 €/h)
- FTE-Werte pro Rolle und Jahr werden beim Rebuild aus `wpFteState` wiederhergestellt
- Angebotsrunden (4 Stück): Einreichungsdatum + Betrag → Rabatt auf Angebotswert
- Mixed Rate = AVERAGE(Rate inkl. Inflation aller Jahre)
