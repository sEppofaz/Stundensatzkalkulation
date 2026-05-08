# Stundensatzkalkulation

Eine sEpp-App zur Berechnung von Stundensätzen, Deckungsbeiträgen und Gehaltskorridoren im Projektgeschäft.

**Live:** https://seppofaz.github.io/Stundensatzkalkulation/

## Reiter

| Reiter | Funktion |
|--------|----------|
| hrs calc | Arbeitsstunden/Jahr auf Basis von AR% und Stunden/Tag |
| Salary → Bill Rate | Stundensatz aus Jahresgehalt berechnen |
| Salary → CM% | Deckungsbeitrag aus Gehalt und Bill Rate |
| Bill Rate → Salary | Gehaltsband aus Bill Rate zurückrechnen |
| Work Package | Multi-Rollen-Projektkalkulation über mehrere Jahre |

## Technisch

- Reine HTML/CSS/JS-App, keine externen Abhängigkeiten
- Alle Werte werden lokal im Browser (localStorage) gespeichert
- Läuft direkt im Browser, kein Server nötig
