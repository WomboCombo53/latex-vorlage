## Latex-Vorlage
In diesem Repo befindet sich eine Latex-Vorlage für die Erstellung von wissenschaftlichen Arbeiten.

### Nutzung
1. Repo klonen
2. Benutzerdefinierte Werte in `main.tex` anpassen
3. Gewünschte Kapitel schreiben und in `main.tex` einfügen

### Build
Zum Kompilieren wird `latexmk` empfohlen, da es die richtige Reihenfolge von pdfLaTeX, biber und makeglossaries automatisch übernimmt:
```
latexmk -pdf main.tex
```

### Zitatstil
Zitate erfolgen im IEEE-Stil (`biblatex` mit `style=ieee`, biber-Backend, konfiguriert in `main.tex`), inklusive Anzeige des URL-Abfragedatums bei Websites.
Bei Nutzung von Zotero kann das Programm automatisch die `/Ressourcen/Literatur/literatur.bib` aktualisieren, damit die Quellen erkannt werden.