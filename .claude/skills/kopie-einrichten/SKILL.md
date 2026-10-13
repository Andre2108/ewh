---
name: kopie-einrichten
description: Einmalige Einrichtung des Klausurmanagers in der Kopie (Fork) einer Kollegin oder eines Kollegen – z. B. wenn jemand schreibt „Richte die App für mich ein“. Nicht im Original-Repository andre2108/ewh verwenden.
---

# Klausurmanager für eine Kollegin / einen Kollegen einrichten

Die Person arbeitet in ihrer **eigenen Kopie** (Fork) von `andre2108/ewh`, mit
eigenem GitHub- und Claude-Konto, meist auf dem iPad. Sie ist in der Regel
**keine Programmiererin**: kurze, freundliche Sätze auf Deutsch, keine
Fachbegriffe, jeden Schritt, den sie selbst antippen muss, genau beschreiben.

## 0. Prüfen, wo du bist

- `git remote get-url origin` – zeigt es auf `andre2108/ewh`, **sofort
  abbrechen**: Das ist das Original, dort wird nichts eingerichtet.
- Gibt es `eigene-klausuren/index.json` schon, ist die Einrichtung erledigt –
  nur noch Schritt 4 (Kontrolle) durchgehen.

## 1. Nachfragen (eine Nachricht, alles auf einmal)

- Name, wie er in Erwartungshorizonten stehen soll (z. B. „Frau Muster“)
- Lehrerkürzel (kommt in `meta.fachlehrer`, z. B. „MUS“)
- Fächer – Standard: nur **Deutsch**
- aktuelles Schuljahr – Standard aus dem heutigen Datum (z. B. `2026-27`)

## 2. Dateien anlegen

`eigene-klausuren/index.json` – leerer Katalog. Sobald dieser Ordner existiert,
zeigt die App **nur** diese Arbeiten (nicht die aus `klausuren/`):

```json
{
 "klausuren": []
}
```

`eigene-klausuren/lehrkraft.json` – Angaben für neue Arbeiten:

```json
{
 "name": "Frau Muster",
 "kuerzel": "MUS",
 "faecher": ["Deutsch"],
 "schuljahr": "2026-27"
}
```

**Nichts anderes ändern** – vor allem nicht `index.html`, `klausuren/`,
`.github/` oder `.claude/`. Diese Dateien kommen automatisch aus dem Original
und würden sonst beim nächsten Update Konflikte erzeugen.

## 3. Speichern

Committen und auf den Zweig der Sitzung pushen. Der Ablauf
`.github/workflows/kopie-aktualisieren.yml` übernimmt den Zweig von selbst nach
`main` und veröffentlicht die Seite (dauert 1–3 Minuten).

## 4. Kontrolle und Abschluss

Mit den GitHub-Werkzeugen (`actions_list` / `actions_get`) die Läufe von
„App aktualisieren und veröffentlichen“ ansehen.

- **Kein Lauf zu sehen** → Actions sind in der Kopie noch aus. Sie soll auf
  github.com in ihrer Kopie den Reiter **Actions** öffnen und
  **„I understand my workflows, go ahead and enable them“** antippen. Danach
  den Ablauf von Hand starten (`actions_run_trigger`, `run_workflow`,
  Datei `kopie-aktualisieren.yml`, ref `main`).
- **Fehler beim Job „veroeffentlichen“** → GitHub Pages ist noch nicht
  eingeschaltet. Sie soll in ihrer Kopie **Settings → Pages** öffnen und bei
  **Source** „**GitHub Actions**“ wählen. Danach den Ablauf erneut starten.
- **Alles grün** → Adresse nennen: `https://<github-name>.github.io/<repo-name>/`
  (aus `origin` ablesen). Dazu:
  1. Adresse in **Safari** öffnen
  2. Teilen-Symbol (Quadrat mit Pfeil) → **„Zum Home-Bildschirm“** → Hinzufügen
  3. Ab jetzt immer über das Symbol „Klausurmanager“ starten – nur dort
     bleiben die Punkte gespeichert.

Zum Schluss erklären, wie es weitergeht: Für eine neue Arbeit einfach eine neue
Sitzung starten, die Klassenarbeit (PDF/Foto), den Erwartungshorizont und die
Kürzelliste hochladen und schreiben „Lege diese Arbeit an“.
