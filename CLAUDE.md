# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A LaTeX template (in German) for academic/thesis-style papers at DHBW (Duale Hochschule Baden-Württemberg), built on KOMA-Script `scrartcl`. All comments and prose in the `.tex` files are German; keep new comments in German for consistency.

## Build

```
latexmk -pdf main.tex
```

`latexmk` (configured via `.latexmkrc`) chains pdfLaTeX, biber (bibliography), and `makeglossaries` (acronyms) in the correct order and rebuild count — do not invoke `pdflatex` directly, the acronym and citation lists will end up stale or empty.

To clean generated build artifacts: `latexmk -C`.

There are no tests/lint commands in this repo; correctness is verified by a successful `latexmk` build (check `main.log` for LaTeX warnings/errors, e.g. undefined references or missing glossary entries).

## Architecture

- `main.tex` — single entry point. Holds all package imports/config and the document's `\input` order (front matter → verzeichnisse → chapters → literature → appendix). Chapters and verzeichnisse are plain `\input`s in sequence, not a multi-file class; adding a new chapter means creating the file under `Kapitel/` and adding an `\input{Kapitel/...}` line in `main.tex` at the desired position.
- `Kapitel/*.tex` — one file per chapter/section of the document. Numbered files (`1_einleitung.tex`, `2_grundlagen.tex`, ...) are content chapters; note the file numbering has a gap (no `4_...`) and mixed German/English filenames — new chapters can follow either convention already present.
- `Kapitel/*verzeichnis.tex` — auto-generated-content pages (table of contents, list of figures/tables/listings, acronym list). Each uses `\setupverzeichnis{<Titel>}` (see below) rather than `\setupchapter`.
- `Ressourcen/abkuerzungen.tex` — acronym definitions for the glossaries package (`\newacronym{label}{Kurzform}{Langform}`), `\input` from `main.tex` before `\begin{document}`.
- `Ressourcen/vorlagen.tex` — **not compiled**, a copy-paste snippet reference (figures, tables, code listings, cleveref usage, acronym usage). Consult this before writing new figures/tables/code blocks/cross-references to match established conventions instead of inventing new patterns.
- `Ressourcen/Bilder/` — images, referenced without path prefix (`\graphicspath` is set in `main.tex`).
- `Ressourcen/Literatur/literatur.bib` — bibliography source for biblatex/biber; can be auto-updated by Zotero's LaTeX integration.

### Header/footer + chapter title convention

`main.tex` defines two helper macros used at the top of every chapter/verzeichnis file instead of touching `fancyhdr` directly:
- `\setupchapter{<Titel>}` — content chapters. Sets `\sectionname`, switches to the `fancy` pagestyle.
- `\setupverzeichnis{<Titel>}` — TOC/list-of-X pages. Also redefines the `plain` pagestyle, since LaTeX forces `plain` on a list's first page.

Both set header-left/footer to `\sectionname`/`\creator`/`\company`. When adding a new chapter or verzeichnis file, call the appropriate macro as the first line rather than styling headers manually.

### Citations, cross-references, acronyms

- Citations: `biblatex` + biber backend, IEEE style (`\parencite{}`, keys from `literatur.bib`).
- Cross-references: `cleveref` — use `\cref{}`/`\Cref{}`, not raw `\ref{}`.
- Acronyms: `glossaries` package — reference with `\gls{label}` after defining the acronym in `Ressourcen/abkuerzungen.tex`; first use in the document auto-expands, later uses show only the short form.
- Figures/tables get `\caption{}\label{fig:...}` / `\label{tab:...}`; code listings use `\begin{lstlisting}[caption=...,label={lst:...}]` with caption/label on the listing itself (not a separate `figure`) so it lists in the Listingverzeichnis rather than the Abbildungsverzeichnis.

## Customization points

Personal/project values are set once as macros near the top of `main.tex` (`\creator`, `\company`) and used throughout headers/footers and `Kapitel/titelblatt.tex`. Update these rather than hardcoding names elsewhere.
