# AG Referenzcurriculum Website (Hugo)

Webseite der Arbeitsgruppe DH-Referenzcurriculum auf Basis von Hugo + Ananke.

## 1) Projekt starten

Im Projektordner:

- `hugo server -D`

Dann im Browser öffnen:

- http://localhost:1313/
- oder http://127.0.0.1:1313/

Hinweis: Das `-D` ist wichtig, damit Entwürfe (`draft: true`) sichtbar sind.

## 2) Projekt bauen (Produktions-Build)

- `hugo`

Der Build landet in `public/`.

## 2.1) Wie Hugo die Seite baut (Überblick)

Hugo nimmt Inhalte aus `content/` (Markdown + Front Matter) und rendert daraus statische HTML-Seiten.

Vereinfacht passiert beim Build:

1. **Content lesen** aus `content/**/*.md`
2. **Front Matter auswerten** (`title`, `date`, `draft`, `summary`, `layout`, usw.)
3. **Passendes Template wählen** aus `layouts/` (oder aus dem Theme, wenn kein lokales Template existiert)
4. **Markdown in HTML umwandeln** und in `public/` schreiben
5. **Zusatzdateien erzeugen** (z. B. `sitemap.xml`, `index.xml` je nach Konfiguration)

Diese Website nutzt z. B. eigene Templates in:

- `layouts/home.html`
- `layouts/news/list.html`
- `layouts/news/single.html`
- `layouts/ag-referenzcurriculum/section.html`
- `layouts/ag-referenzcurriculum/single.html`

## 2.2) Was aus einer Markdown-Datei erzeugt wird

Beispiele für die wichtigsten Routen:

- `content/_index.md` → `public/index.html`
- `content/news/_index.md` → `public/news/index.html`
- `content/news/degree-fair.md` → `public/news/degree-fair/index.html`
- `content/ag-referenzcurriculum/ziele.md` → `public/ag-referenzcurriculum/ziele/index.html`

Wichtig: Hugo erzeugt standardmäßig **Ordner-URLs** (`/news/degree-fair/`) mit einer `index.html` im jeweiligen Ordner.

## 3) Content-Struktur (wichtig)

- Startseite: `content/_index.md`
- AG-Bereich: `content/ag-referenzcurriculum/`
- News-Bereich: `content/news/`
  - Listen-Seite: `content/news/_index.md`
  - Einzelbeiträge: `content/news/*.md`

## 4) Neue Seite anlegen

Beispiel: neue News-Seite

- `hugo new content/news/mein-beitrag.md`

Dann Front Matter prüfen/ergänzen:

```yaml
---
title: "Mein Beitrag"
date: 2026-05-07T10:00:00+02:00
description: "Kurze Beschreibung"
summary: "Kurztext für Karten/Listenansichten"
draft: false
---
```

Danach Textinhalt unterhalb des Front Matter schreiben.

## 4.1) Neue News-Datei: Muss ich sonst noch etwas tun?

Normalerweise: **nein**.

Wenn du eine neue Datei unter `content/news/` anlegst, erscheint sie automatisch:

- auf der News-Übersicht (`/news/`), weil `layouts/news/list.html` über `.RegularPages` iteriert
- als eigene Detailseite unter `/news/<slug>/`

Du musst nur darauf achten, dass:

- die Datei wirklich unter `content/news/` liegt
- Front Matter gültig ist
- `draft` nicht auf `true` steht (oder du mit `-D` arbeitest)
- das Datum nicht ungewollt in der Zukunft liegt

Optional sinnvoll:

- `summary` setzen (für Karten/Listen-Teaser)
- sprechenden Dateinamen nutzen (wird meist Teil der URL)

## 5) Bestehende Seite bearbeiten

1. Datei in `content/...` öffnen
2. Inhalt ändern
3. Prüfen, dass Front Matter gültig bleibt
4. Im Dev-Server aktualisieren

## 6) Warum eine Seite nicht zu sehen ist (Checkliste)

Wenn eine Seite nicht mehr sichtbar ist, fast immer einer dieser Gründe:

1. **Datei wurde umbenannt/verschoben/gelöscht**
	- Beispiel: `content/news/fruehjahrstreffen-2026.md` existiert nicht mehr.
	- Aktuell vorhanden sind z. B. `content/news/degree-fair.md` und `content/news/working-paper-hinweis.md`.

2. **`draft: true` gesetzt**
	- Dann ist die Seite nur mit `hugo server -D` sichtbar.

3. **Datum liegt in der Zukunft**
	- Seiten mit zukünftigem `date`/`publishDate` erscheinen ohne Future-Build nicht.
	- Für lokale Vorschau ggf. mit `hugo server -D --buildFuture` starten.

4. **Front Matter kaputt (YAML/TOML-Fehler)**
	- Falsche Einrückung, Tabs in YAML, fehlende `---`-Trenner etc.

5. **Falscher Ordner**
	- News-Beiträge müssen unter `content/news/` liegen.

## 7) Nützliche Diagnose-Kommandos

Alle Seiten, die Hugo aktuell erkennt:

- `hugo list all`

Nur veröffentlichte Seiten prüfen:

- `hugo list published`

Nur News-Dateien im Repository sehen:

- `ls content/news`

## 8) Layout-Hinweise (für diese Website)

- News-Liste nutzt: `layouts/news/list.html`
- News-Detail nutzt: `layouts/news/single.html`
- AG-Landingseite: `layouts/ag-referenzcurriculum/section.html`

Wenn Content vorhanden ist, aber visuell unerwartet aussieht, zuerst diese Layouts prüfen.

## 9) Deployment mit GitHub Pages (ohne public/ im Repo)

1. Repository nach GitHub pushen.
2. In GitHub: **Settings > Pages** öffnen und bei **Source** auf **GitHub Actions** stellen.
3. Sicherstellen, dass der Default-Branch `main` ist (sonst den Branch im Workflow anpassen).
4. Das Theme-Submodule muss vorhanden sein; der Workflow checkt Submodule bereits rekursiv aus.
5. `public/` und `resources/` bleiben uncommitted, weil der Build in CI erfolgt.
6. Ergebnis-URL bei Project Pages: `https://<username>.github.io/<repo>/`
