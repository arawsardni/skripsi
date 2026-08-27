# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repo layout

This repo holds two things: the thesis manuscript (`thesis/`, LaTeX) and the research
code behind it (`experiments/`, Python). Keep them separate — don't put figures/data
generation logic in `thesis/`, and don't put manuscript prose in `experiments/`.

## Build

Compile with **XeLaTeX**, not pdflatex — the preamble uses `fontspec` for the Carlito font:

```bash
cd thesis
xelatex -interaction=nonstopmode main.tex
xelatex -interaction=nonstopmode main.tex   # run twice — TOC/refs need two passes
```

Requires the Carlito font (a metric-compatible Calibri substitute — swap to real Calibri via
the `\setmainfont` line in `main.tex` if it's installed) and, on minimal TeX installs, the
`texlive-lang-other` package for Indonesian babel support. Full rationale for these deviations
from the generic template: `thesis/CATATAN_PERUBAHAN.md`.

## Structure

- `thesis/main.tex` — root file; compile this. Contains all preamble packages and `\include` calls.
- `thesis/frontmatter/cover.tex` — active. The other frontmatter files (approval page, originality
  statement, abstract, acknowledgements) are commented out in `main.tex` — they're post-sidang
  artifacts, not relevant at proposal stage; re-enable once the manuscript is a full skripsi.
- `thesis/chapters/bab1–bab3.tex` — Pendahuluan, Landasan Kepustakaan, Metodologi Penelitian — the
  full current scope, since a Filkom UB proposal covers only Bab 1–3 + referensi.
- `thesis/chapters/bab4–bab7.tex` — still the generic placeholder scaffold, commented out in
  `main.tex`. Fill in and re-enable only after the proposal is approved and the research is
  actually carried out.
- `thesis/backmatter/referensi.tex` — bibliography, Harvard style (the supervisor-approved variant
  used in the praproposal — not Harvard ARU; see `CATATAN_PERUBAHAN.md`).
- `thesis/assets/` — images referenced via `\includegraphics`; `\graphicspath{{assets/}}` set in preamble
- `thesis/CATATAN_PERUBAHAN.md` — every deviation from the generic template (font, spacing, compiler,
  chapter scope, terminology) made to comply with Panduan Skripsi Filkom UB v3.0, and why.
- `experiments/` — Python code, data, and results backing Bab 3–6; see `experiments/README.md` for the data/notebooks/src/configs/results convention

## Key conventions

- Equations numbered per-chapter: `(3.1)`, `(3.2)` — via `\numberwithin{equation}{chapter}`
- Code listings use `lstdefinestyle{kode}` with Indonesian caption name `Kode Sumber`
- TikZ flowchart node styles (`term`, `proc`, `procplain`, `io`, `dec`, `arr`) defined in preamble — use these for all diagrams
- `\gambarbesar{...}` macro for full-width/height-constrained figures
- `siunitx` decimal marker set to `,` (Indonesian convention)
- Body text is single-spaced (Filkom UB v3.0, Lampiran A.4) — not the generic template's 1.5 spacing
- Page numbering: cover = none, frontmatter = roman, main body = arabic

## Known gaps

- Bab 3, subbab 3.9.2 (Perangkat Keras) is still a `[...]` placeholder — fill in with the actual
  GPU/compute spec once the experiment environment is finalized (see `experiments/`).

## Experiments → thesis convention

Results cited in Bab 5–6 must trace back to a specific run: config in
`experiments/configs/`, output in `experiments/results/<eksperimen>/`. Figures/tables
copied into `thesis/assets/` for use in the manuscript should keep a name that maps back
to the `results/<eksperimen>/` that produced them.
