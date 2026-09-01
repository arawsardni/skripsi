# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repo layout

This repo holds two things: the thesis manuscript (`thesis/`, LaTeX) and the research
code behind it (`experiments/`, Python). Keep them separate — don't put figures/data
generation logic in `thesis/`, and don't put manuscript prose in `experiments/`.

## Build

Compile with **XeLaTeX**, not pdflatex — the preamble uses `fontspec` for the Calibri font.
There are two independent entry points, both compilable at any time (no comment/uncomment
dance needed to switch between them):

```bash
cd thesis
xelatex -interaction=nonstopmode main_proposal.tex   # proposal: Bab 1-3 + referensi + lampiran
xelatex -interaction=nonstopmode main_proposal.tex   # run twice — TOC/refs need two passes

xelatex -interaction=nonstopmode main_skripsi.tex    # full skripsi (not the current focus)
xelatex -interaction=nonstopmode main_skripsi.tex
```

Requires the Calibri font. On a machine without it (e.g. Overleaf, Linux), swap the
`\setmainfont` line in `preamble.tex` to Carlito, a metric-compatible substitute, so line
breaks stay identical. Minimal TeX installs also need the `texlive-lang-other` package for
Indonesian babel support. Full rationale for these deviations from the generic template:
`thesis/CATATAN_PERUBAHAN.md`.

## Structure

- `thesis/preamble.tex` — all preamble packages/styles, shared via `\input` by both entry points below. Edit here, not per-entry-point, for anything that should apply to both.
- `thesis/main_proposal.tex` — entry point for the proposal (Bab 1–3 + referensi + Lampiran A/B, no pengesahan/orisinalitas/abstrak/prakata). This is the one to compile right now.
- `thesis/main_skripsi.tex` — entry point for the full skripsi later (adds the four frontmatter pages and, eventually, Bab 4+). Not the current focus — its frontmatter files are still generic-template placeholders, see Known gaps.
- `thesis/frontmatter/cover.tex` — shared cover; `\doctypelabel`/`\doctypesubtitle` (set by whichever main file is compiled) control the "PROPOSAL SKRIPSI" vs "SKRIPSI" text, so this file itself never needs edits for that.
- `thesis/frontmatter/lembar_pengesahan.tex`, `pernyataan_keaslian.tex`, `abstrak.tex`, `kata_pengantar.tex` — only pulled in by `main_skripsi.tex`; not relevant while working on the proposal.
- `thesis/chapters/bab1–bab3.tex` — Pendahuluan, Landasan Kepustakaan, Metodologi Penelitian — filled in, current scope of `main_proposal.tex`.
- `thesis/chapters/bab4–bab7.tex` — still the generic placeholder scaffold, commented out in `main_skripsi.tex`. Chapter count/titles for this range are not yet decided — see Known gaps before assuming these four filenames are final.
- `thesis/backmatter/referensi.tex` — bibliography, Harvard style (the supervisor-approved variant
  used in the praproposal — not Harvard ARU; see `CATATAN_PERUBAHAN.md`).
- `thesis/backmatter/lampiran.tex` — the Lampiran section, intentionally left as an empty page (see `CATATAN_PERBAIKAN.md`, Review Iterasi 2 item 1): the Lampiran A/B text in the official template docx documents the template's *own* formatting/language conventions, it isn't meant to be copied verbatim into each student's submission. `lampiran_a.tex`/`lampiran_b.tex` still exist with that earlier (now unused) content — not `\include`d by either entry point, kept only as a reference in case that reading gets revisited.
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

- Bab 3, subbab 3.9.2 (Perangkat Keras) is filled in (2× RTX 3060 6GB + VRAM analysis) but its
  RAM/storage line is still a `[...]` placeholder — fill in once known.
- Rumusan Hipotesis, Jadwal Penelitian, and Analisis Kinerja Komputasi (waktu pelatihan/VRAM as
  a formally analyzed metric) were deliberately removed from the proposal entirely, on explicit
  user instruction — not an oversight. This goes against `SKRIPSI_CONTEXT.MD`'s own recommendation
  (it suggested relocating Hipotesis/Jadwal to Bab 3 rather than deleting them, since Panduan
  v3.0 lists both as required somewhere in the document). If asked about this at sidang, that's
  the context to know.
- Ou et al. (2024) in `referensi.tex` has incomplete bibliographic details (only "Ling Ou dkk.,
  IEEE 2024" was available) — first-author name order, full author list, venue, and pages all
  need verification before submission.
- Bab 3's dataset table (§3.3) has E-SERAVD's sample count filled in (~5,049, with class
  breakdown in prose), but it's still marked approximate — needs verification against Santoso,
  Budianto and Dutono (2026)'s own documentation before submission.
- Bab 4–7 chapter count/titles are undecided. Panduan Skripsi Filkom UB v3.0 gives a
  "Nonimplementatif Eksperimental" example structure (Bab 4 Hasil, Bab 5 Pembahasan, Bab 6
  Penutup — optionally merging 4+5 into one "Hasil dan Pembahasan" chapter) which matches this
  thesis's research type better than the generic template's 7-chapter implementatif-style
  scaffold currently sitting in `chapters/bab4.tex`–`bab7.tex`. The guide explicitly allows
  thematic/descriptive chapter titles instead of the literal words "Hasil"/"Pembahasan" — don't
  rename these files to force a specific title without the user's say-so.
- `thesis/frontmatter/lembar_pengesahan.tex`, `pernyataan_keaslian.tex`, `abstrak.tex`,
  `kata_pengantar.tex` are still generic-template content, not yet adapted to Filkom UB v3.0
  wording the way Bab 1–3 and the cover were. Not urgent — only pulled in by `main_skripsi.tex`.

## Experiments → thesis convention

Results cited in Bab 5–6 must trace back to a specific run: config in
`experiments/configs/`, output in `experiments/results/<eksperimen>/`. Figures/tables
copied into `thesis/assets/` for use in the manuscript should keep a name that maps back
to the `results/<eksperimen>/` that produced them.
