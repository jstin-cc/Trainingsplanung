# Wochentrainingsplaner-UI

_Started 2026-04-30 09:54 UTC_

---

## User

Gestalte die Benutzeroberfläche einer Wochentrainingsplaner-Web-App. Die App besteht aus zwei nebeneinanderliegenden Bereichen: links ein Eingabebereich, rechts eine Live-Vorschau.
Linker Bereich - Eingabeformular:
7 Abschnitte, einer pro Wochentag (Montag bis Sonntag). Jeder Tag enthält:

Einen Krafttrainingsblock: Textfeld für den Titel (z.B. "Push / Pull"), optionales Textfeld für eine Notiz (z.B. "ohne Schultern"). Kann leer bleiben.
Einen Ausdauerblock: Intensitätsauswahl mit 5 Punkten (1=sehr locker bis 5=sehr hart). Nur der gewählte Punkt ist farbig (1=hellgrün, 2=grün, 3=gelb, 4=orange, 5=rot), die restlichen 4 sind grau. Optionales Freitextfeld neben den Punkten (z.B. "Intervalle", "4×8'", "lang"). Kann leer bleiben.

Rechter Bereich - Live-Vorschau:
Zeigt den Wochenplan als sauberes Karten-Grid (7 Spalten, eine pro Tag). Jede Tageskarte zeigt:

Den Tagesnamen als Überschrift
Kraftblock: Titel und Notiz in neutralem Stil, keine Farbkodierung
Ausdauerblock: 5 Punkte mit dem gewählten farbig hervorgehoben, darunter das Label wenn vorhanden
Leere Blöcke werden einfach nicht angezeigt

Unten: ein auffälliger Button "Als PNG exportieren".
Stil: sauber, modern, dunkler Hintergrund, gut lesbar, geeignet zum Teilen als Bild. Alle UI-Beschriftungen auf Deutsch.

## Assistant

_[tool: list_files]_

_[tool: invoke_skill]_

_[tool: write_file]_

_[tool: done]_

_[tool: fork_verifier_agent]_

Die App ist fertig! Hier eine kurze Übersicht:

**Was gebaut wurde:**
- **Linkes Panel** — 7 Tagesabschnitte mit Kraft- und Ausdauerblöcken. Intensitätspunkte schalten farbig (grün → gelb → orange → rot), Klick auf denselben Punkt hebt die Auswahl auf.
- **Rechtes Panel** — Live-Vorschau als 7-Spalten-Grid, aktualisiert sich sofort beim Tippen. Leere Blöcke werden ausgeblendet.
- **PNG-Export** — rendert einen versteckten 1400×420px-Canvas und lädt ihn als `trainingsplan.png` herunter.
- **Persistenz** — alle Eingaben werden in `localStorage` gespeichert, bleiben nach einem Refresh erhalten.

**Mögliche nächste Schritte:**
- Wochentitel oder Datum oben im Export eintragen
- Mehr Felder (z.B. Gewichte, Sets/Reps pro Kraftblock)
- Wochen speichern / zwischen Wochen wechseln

## User

Ausdauer soll auch Titel und Notiz haben, möglichkeit weitere Ausdauereinheiten hinzuzufügen

## Assistant

_[tool: read_file]_

_[tool: write_file]_

_[tool: done]_

_[tool: fork_verifier_agent]_

Erledigt! Was sich geändert hat:

- **Titel & Notiz** — jede Ausdauereinheit hat jetzt ein eigenes Titelfeld (z.B. „Lauf", „Rad") und ein Notizfeld
- **Mehrere Einheiten** — per „Ausdauereinheit hinzufügen"-Button können beliebig viele Einheiten pro Tag angelegt werden
- **Entfernen** — jede Einheit lässt sich einzeln mit ✕ löschen
- **Vorschau & Export** — mehrere Einheiten werden im Grid durch feine Trennlinien gegliedert, alle Felder erscheinen im PNG-Export

Alte Daten aus dem localStorage werden automatisch ins neue Format migriert.

## User

ist es gewollt das der linke bereich nicht scrollbar ist

## Assistant

_[tool: str_replace_edit]_

Nein, das ist ein Bug — der linke Bereich sollte scrollen, tut es aber nicht weil dem Flex-Kind `min-height: 0` fehlt. Schnelle Korrektur:

_[tool: done]_

Behoben — der linke Bereich scrollt jetzt korrekt wenn der Inhalt die Höhe übersteigt.

## User

funktioniert nicht

## Assistant

_[tool: eval_js_user_view]_

_[tool: str_replace_edit]_

Das Problem ist klar: `.app-body` expandiert auf 1875px statt auf die Viewport-Höhe begrenzt zu bleiben. `overflow: hidden` fehlt dort:

_[tool: done]_

Jetzt sollte der linke Bereich korrekt scrollen — der Container war zuvor nicht auf die Viewport-Höhe begrenzt.

## User

nein immer noch nicht scrollbar ich muss mit tab durchnavigieren

## Assistant

_[tool: eval_js_user_view]_

_[tool: str_replace_edit]_

`app-body` bleibt auf 1875px trotz `overflow: hidden` — das Flex-System greift nicht. Ich setze eine explizite Höhe:

_[tool: str_replace_edit]_

Let me take a completely different approach and use fixed positioning:

_[tool: grep]_

_[tool: str_replace_edit]_

_[tool: done]_

_[tool: eval_js_user_view]_

Jetzt funktioniert es — `clientHeight` ist 861px, `scrollHeight` 1875px, Scrollen ist aktiv. Der linke Bereich sollte jetzt normal mit dem Mausrad oder per Touch scrollbar sein.

## User

Manchmal mache ich ein Koppeltraining zb laufen und radfahren, bitte noch eine möglichkeit das zu kennzeichnen hinzufügen

## Assistant

_[tool: read_file]_

Ich füge eine „Koppeln"-Funktion hinzu: ein Toggle zwischen zwei Ausdauereinheiten, der sie im Formular visuell verbindet und in der Vorschau als Koppeltraining kennzeichnet.

_[tool: str_replace_edit]_

Now update the export canvas to also show the Koppel badge:

_[tool: str_replace_edit]_

_[tool: grep]_

_[tool: read_file]_

_[tool: str_replace_edit]_

_[tool: run_script]_

_[tool: done]_

Fertig! So funktioniert die Koppel-Funktion:

- **Zwei Ausdauereinheiten anlegen** → zwischen ihnen erscheint automatisch ein „koppeln"-Button
- **Klick darauf** → Button wird orange, zeigt „⛓ Koppeltraining" und markiert die Verbindung
- **Vorschau & Export** → zwischen den gekoppelten Einheiten erscheint ein orangefarbener „⛓ KOPPEL"-Trenner statt der normalen Linie

## User

koppeltenner wird in vorschau nicht angezeigt

## Assistant

_[tool: grep]_

Der Bug: ich prüfe `entry.coupled` am aktuellen Eintrag, aber das Flag gehört zum *vorherigen* — der Trenner vor Eintrag `i` sollte `activeEndurance[i-1].coupled` prüfen.

_[tool: str_replace_edit]_

Dasselbe fix im Export-Canvas:

_[tool: run_script]_

_[tool: done]_

Behoben — der Koppel-Trenner wird jetzt korrekt zwischen den verbundenen Einheiten angezeigt, sowohl in der Vorschau als auch im PNG-Export.

