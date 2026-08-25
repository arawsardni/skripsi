# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repo layout

This repo holds two things: the thesis manuscript (`thesis/`, LaTeX) and the research
code behind it (`experiments/`, Python). Keep them separate — don't put figures/data
generation logic in `thesis/`, and don't put manuscript prose in `experiments/`.

## Build

```bash
cd thesis
pdflatex -interaction=nonstopmode main.tex
pdflatex -interaction=nonstopmode main.tex   # run twice — TOC/refs need two passes
```

## Structure

- `thesis/main.tex` — root file; compile this. Contains all preamble packages and `\include` calls.
- `thesis/frontmatter/` — cover, approval page, originality statement, abstract (ID+EN), acknowledgements
- `thesis/chapters/bab1–bab7.tex` — Pendahuluan → Tinjauan Pustaka → Metodologi → Perancangan → Implementasi → Pengujian → Kesimpulan
- `thesis/backmatter/referensi.tex` — bibliography (Harvard Anglia Ruskin University style)
- `thesis/assets/` — images referenced via `\includegraphics`; `\graphicspath{{assets/}}` set in preamble
- `experiments/` — Python code, data, and results backing Bab 3–6; see `experiments/README.md` for the data/notebooks/src/configs/results convention

## Key conventions

- Equations numbered per-chapter: `(3.1)`, `(3.2)` — via `\numberwithin{equation}{chapter}`
- Code listings use `lstdefinestyle{kode}` with Indonesian caption name `Kode Sumber`
- TikZ flowchart node styles (`term`, `proc`, `procplain`, `io`, `dec`, `arr`) defined in preamble — use these for all diagrams
- `\gambarbesar{...}` macro for full-width/height-constrained figures
- `siunitx` decimal marker set to `,` (Indonesian convention)
- Page numbering: cover = none, frontmatter = roman, main body = arabic

## Customization checklist

1. `thesis/frontmatter/cover.tex` — replace all `[...]` placeholders
2. `thesis/main.tex` `\hypersetup` — update `pdftitle` and `pdfauthor`
3. `thesis/assets/logo_institusi.png` — replace with actual university logo
4. Each chapter file — replace `[...]` with real content

## Experiments → thesis convention

Results cited in Bab 5–6 must trace back to a specific run: config in
`experiments/configs/`, output in `experiments/results/<eksperimen>/`. Figures/tables
copied into `thesis/assets/` for use in the manuscript should keep a name that maps back
to the `results/<eksperimen>/` that produced them.
