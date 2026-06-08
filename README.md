# Trainingsplanung

Wochentrainingsplaner-Web-App zum Anlegen, Vorschau und PNG-Export eines Wochenplans (Mo–So) mit Kraft- und Ausdauerblöcken.

## Features

- **7 Tagesabschnitte** (Montag–Sonntag) mit:
  - **Kraftblock**: Workout-Template wählen (Push / Pull / Leg / Oberkörper) oder freier Titel + optionale Notiz — Muskelgruppen werden automatisch per Template gesetzt
  - **Ausdauerblock**: beliebig viele Einheiten mit Sportart (Schwimmen / Radfahren / Laufen / Sonstiges), Titel, Intensität (5-Punkte-Skala), Dauer, Strecke (km/m je Sportart), Pace und Notiz
- **Koppeltraining** kennzeichnen: Toggle zwischen zwei aufeinanderfolgenden Ausdauereinheiten
- **Wochenübersicht** (rechte Seite):
  - Einheiten- und Gesamtzeit-Kacheln
  - Verteilungsbalken leicht / mittel / hart
  - **SVG-Muskelkarte** (Vorder-/Rückansicht) mit 4-stufiger Einfärbung je Trainingsfrequenz (0 = inaktiv → 3+ = intensiv) neben dem Verteilungsbalken
- **Live-Vorschau** als 7-Spalten-Grid, leere Tage/Blöcke werden ausgeblendet
- **PNG-Export** des Wochenplans (1400px breit) via `html2canvas`
- **Persistenz** aller Eingaben in `localStorage` (Schlüssel: `trainingsplanung_v2`, mit Migration aus `_v1`)

## Workout-Templates (Kraft)

| Template | Muskelgruppen |
|---|---|
| Push | Brust, Schultern, Trizeps |
| Pull | Rücken, Bizeps, Unterarme |
| Leg | Beine, Waden |
| Oberkörper mit Armen | Brust, Rücken, Schultern, Bizeps, Trizeps, Bauch/Core |
| Oberkörper ohne Arme | Brust, Rücken, Schultern, Bauch/Core |

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

## Design

Stack: React 18 (UMD), Space Grotesk / Space Mono, dunkles Theme (`#111113`), Akzentfarbe Lila (`#7c6af7`). Prototyp-Vorlage unter `design/trainingsplanung/`.
