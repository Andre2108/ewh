---
name: arbeit-anlegen
description: Neue Klassenarbeit, Klausur oder Nachschrift für den Klausurmanager in der Kopie einer Kollegin anlegen (Ordner eigene-klausuren/), z. B. aus hochgeladener Aufgabenstellung, Erwartungshorizont und Kürzelliste. Auch für Korrekturen an einer dort angelegten Arbeit. Nicht im Original-Repository andre2108/ewh verwenden.
---

# Arbeit im Klausurmanager anlegen (Kopie einer Kollegin)

Gilt nur, wenn es `eigene-klausuren/index.json` gibt. Fehlt der Ordner, zuerst
den Skill `kopie-einrichten` ausführen. Die Lehrkraft ist meist am iPad und
keine Programmiererin – kurz und auf Deutsch antworten.

## Was du brauchst

1. **Aufgabenstellung** (PDF/Foto) und, falls vorhanden, **Erwartungshorizont**
   bzw. Lösungsskizze mit Punkten. Fehlt der EWH, einen Vorschlag machen und
   die Punkte ausdrücklich bestätigen lassen.
2. **Kürzelliste** des Kurses – **nur Kürzel, niemals Klarnamen** ins
   Repository (es ist öffentlich). Bekommst du Namen, selbst Kürzel bilden
   (Initialen Vorname+Nachname, bei Dopplungen verlängern) und die Zuordnung
   nur im Chat zurückgeben. Klarnamen trägt die Lehrkraft in der App selbst ein
   (bleiben auf dem Gerät).
3. Kurs (z. B. `D-7b`, `D GK Q1`), Datum, Nummer der Arbeit.
   Lehrerkürzel und Schuljahr stehen in `eigene-klausuren/lehrkraft.json`.

## Vorbilder – immer zuerst lesen

Die Arbeiten in `klausuren/` stammen aus dem Original und zeigen das Format
vollständig. Neue Dateien **genauso** aufbauen:

- Sek I (Klassenarbeit, Punkte → Note prozentual, halbe Punkte, Wahlaufgabe,
  Hilfekarten): `klausuren/ewh-westbuehl-6c.json`
- Sek II (Klausur, Notenpunkte, Darstellungsleistung, Abwertung):
  `klausuren/ewh-heimsuchung-tuchfabrikant-d-gk-q2.json`
- Nachschrift: `klausuren/ewh-heimsuchung-frau-des-architekten-nachschrift-d-gk-q2.json`

`klausuren/` selbst **nie ändern**, ebenso wenig `index.html`, `.github/`,
`.claude/` – sie kommen automatisch aus dem Original. Wünsche an die App
selbst bitte an Andre weitergeben (er entwickelt sie im Original weiter).

## Dateien

### `eigene-klausuren/<id>.json`

`<id>`: `ewh-<stichwort>-<kurs>`, klein, ohne Umlaute/Leerzeichen, z. B.
`ewh-kurzgeschichte-7b`. Muss eindeutig sein und gleich `meta.storage_id`.

```
meta
  exam_title        Titel wie auf der Arbeit
  thema             Kurzform
  kurs, fach ("Deutsch"), datum ("TT.MM.JJJJ"), schuljahr, fachlehrer, klausur_nr
  klassenstufe      Zahl (Sek I)
  combination_max   Gesamtpunktzahl = Summe aller sections[].max
  storage_id        = <id>
  n_students        = Anzahl Kürzel
  kuerzel           ["AB", "CD", ...]  (Reihenfolge = Reihenfolge der Hefte)
  notenschema       "prozentual" nur Sek I; dazu defizitschwelle 0.5 (in der App
                    auf 0.45 umschaltbar). Sek II: weglassen und "notenfindung"
                    (Tabelle) aus dem Sek-II-Vorbild übernehmen bzw. auf die
                    Gesamtpunktzahl umrechnen.
  aufgabenart       Sek II, z. B. "Ia"
  oberthema         fett im Bereich „Erledigt“
  inhalte           3–5 kurze Stichpunkte zum Inhalt der Arbeit
  nachschrift_von   nur bei Nachschriften: id der Originalarbeit
sections[]          Abschnitte (Aufgaben bzw. Inhalt/Darstellung)
  key               eindeutig, z. B. "a1", "d_inhalt", "darstellung"
  art               "inhalt" oder "darstellung"
  titel, max
  groups[]          Teilaufgaben
    titel, max      max deckelt die Teilaufgabe
    aufgabenformat  "geschlossen" (feste Lösung → Kriterien aufgeklappt) oder
                    "offen" (Schreibaufgabe → zugeklappt)
    aufgeklappt     true/false erzwingt die Anzeige
    criteria[]      Bewertungskriterien
      nr            fortlaufend (optional, die App nummeriert selbst)
      field         eindeutiger Spaltenname, z. B. "Teilaufgabe_A2_1_Punkte",
                    "Darstellung_1_Punkte" – nie doppelt in einer Datei
      max           Punkte
      short         Kurztitel (eine Zeile)
      intro         Satz „Der Prüfling …, etwa:“
      bullets       Erwartungen; Wichtigstes mit **fett** markieren
      schema        optional: interne Bewertungshinweise (nur App)
      sofort        optional: Kurzlösung, immer sichtbar (nur App)
      reserve       true = Reservezeile (ersetzt fehlende Punkte, deckelt)
    type: "wahlaufgabe" + options[{key, label, criteria[]}] statt criteria,
                    wenn die Schüler zwischen Aufgaben wählen (siehe 6c-Vorbild)
hilfekarten         optional, gestufte Hilfen mit Höchstpunkten (siehe 6c-Vorbild)
abwertung           Sek II: Abwertung wegen Sprachrichtigkeit (aus Vorbild)
notenfindung        Sek II Pflicht; Sek I optional (die App rechnet selbst)
```

Darstellungsleistung: Formulierungen aus den Vorbildern übernehmen und an die
Aufgabe anpassen; Punkte genau wie im Erwartungshorizont.

### Eintrag in `eigene-klausuren/index.json`

Neuen Eintrag ans Ende von `"klausuren"` anhängen:

```json
{
 "id": "ewh-kurzgeschichte-7b",
 "titel": "…",
 "kurs": "D-7b",
 "datum": "TT.MM.JJJJ",
 "fach": "Deutsch",
 "file": "eigene-klausuren/ewh-kurzgeschichte-7b.json",
 "art": "Arbeit",
 "oberthema": "…",
 "inhalte": ["…"]
}
```

`art`: `"Arbeit"` (Sek I) oder `"Klausur"` (Sek II). Nachschrift zusätzlich
`"nachschrift_von"` und `"kuerzel"` (nur die Nachschreibenden).

## Prüfen vor dem Speichern

Kurz mit Python prüfen: JSON gültig, `field` eindeutig, Summe der
Kriterien je Gruppe ≥ Gruppen-`max`, Summe der `sections[].max` =
`combination_max`, `n_students` = Anzahl Kürzel, Eintrag im Katalog zeigt auf
die Datei. Dann der Lehrkraft eine **kurze Übersicht** geben (Aufgaben mit
Punkten, Gesamtpunkte) und fragen, ob es so passt.

## Speichern

Committen (Nachricht auf Deutsch, z. B. „Klassenarbeit D-7b: Kurzgeschichte“)
und auf den Zweig der Sitzung pushen. Die Kopie übernimmt das von selbst nach
`main` und veröffentlicht es. Der Lehrkraft sagen: In 1–3 Minuten in der App
auf **„Liste aktualisieren“** tippen (oder die Übersicht herunterziehen).

Spätere Änderungen an einer Arbeit (Punkte, Kriterien, Kürzel) genauso: Datei
anpassen, pushen. Bereits vergebene Punkte bleiben in der App erhalten, solange
`field`-Namen gleich bleiben. Ein Kürzel dauerhaft umbenennen: in `kuerzel`
ändern und `meta.kuerzel_umbenannt` = `{"ALT": "NEU"}` setzen.
