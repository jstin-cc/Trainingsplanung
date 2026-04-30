# Trainingsplanung

Wochentrainingsplaner-Web-App zum Anlegen, Vorschau und PNG-Export eines Wochenplans (Mo–So) mit Kraft- und Ausdauerblöcken.

## Features

- **7 Tagesabschnitte** (Montag–Sonntag) mit:
  - **Kraftblock**: Titel + optionale Notiz
  - **Ausdauerblock**: beliebig viele Einheiten mit Titel, Intensität (5-Punkte-Skala, hellgrün → rot), Label und Notiz
- **Koppeltraining** kennzeichnen: Toggle zwischen zwei aufeinanderfolgenden Ausdauereinheiten
- **Live-Vorschau** als 7-Spalten-Grid, leere Tage/Blöcke werden ausgeblendet
- **PNG-Export** des Wochenplans (1400px breit) via `html2canvas`
- **Persistenz** aller Eingaben in `localStorage` (Schlüssel: `trainingsplanung_v2`, mit Migration aus `_v1`)

## Starten

`src/index.html` direkt im Browser öffnen — keine Build-Schritte nötig. Abhängigkeiten (React 18, Babel Standalone, html2canvas) werden via CDN geladen.

## Struktur

```
src/index.html      # Komplette App (HTML + CSS + React via Babel)
input/              # (leer) Rohdaten
output/             # (leer) Exporte
logs/               # (leer)
design/             # Original Design-Bundle aus Claude Design (Referenz)
.claude/            # Projektkontext, Agents, Skills
```

## Design-Quelle

Aus Claude Design exportierter Prototyp (siehe `design/trainingsplanung/`). Stack: React 18 (UMD), Space Grotesk / Space Mono, dunkles Theme (`#111113`), Akzentfarbe Lila (`#7c6af7`).
