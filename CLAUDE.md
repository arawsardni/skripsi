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

- **Major research-design pivot (2026-09-01 through 09-02, revised three more times)**: the
  thesis abandoned the single/two-stage × full-FT/LoRA design entirely, pivoted to a direct
  SSL-vs-MFCC comparison (XLSR-53 frozen, no fine-tuning), then reframed into a 2×2 factorial
  testing representation × split-scheme, then (Iterasi 8 in `CATATAN_PERBAIKAN.md`) reframed once
  more into its current, settled shape: **algoritma baseline (CNN+MFCC, replicating Santoso et
  al.'s setup) vs algoritma SOTA (XLSR-53+LoRA, the current best approach for Indonesian SER)**,
  each evaluated under both a *random* split and a film-based *movie-independent* (leave-one-
  film-out, k=6) split, on a single dataset (E-SERAVD v1.1). Note LoRA fine-tuning is back — the
  backbone stays frozen but LoRA adapters + classification head are trained end-to-end, unlike
  the purely-frozen-extraction design from the round before. The core question is whether the
  SOTA algorithm is more robust to film leakage than the baseline — Santoso, Budianto and Dutono
  (2026)'s baseline is replicated as Kondisi A and re-tested under the stricter split. Anchor
  references unchanged throughout (Atmaja and Sasou 2022, Santoso et al. 2026). Bab 1–3 were
  rewritten to match each time. Full rationale and the abandoned-alternatives history:
  `.claude/SKRIPSI_CONTEXT.MD` (though Iterasi 8's algoritma-framing came from the user's own
  `CATATAN_PERBAIKAN.md` notes directly, not a `SKRIPSI_CONTEXT.MD` revision).
- Bab 3, subbab 3.8.2 (Perangkat Keras) is filled in (1× RTX 3060 6GB, VRAM no longer a major
  constraint now that the SSL side is frozen-extraction rather than full fine-tuning) but its
  RAM/storage line is still a `[...]` placeholder — fill in once known. Unrelated to the pivot.
- Rumusan Hipotesis, Jadwal Penelitian, and Analisis Kinerja Komputasi were deliberately removed
  from the proposal entirely, on explicit user instruction — not an oversight. This has now been
  reconfirmed three times across three `SKRIPSI_CONTEXT.MD` revisions, each of which briefly
  implied Hipotesis should come back (most recently a dedicated checklist item asking for H0/H1
  per RM — unlike the earlier two mentions, this one was tailored specifically to the new 2×2
  design's main-effect/interaction structure, not obviously leftover boilerplate, yet the user
  still confirmed the deletion stands). If asked about this at sidang, that's the context to
  know.
- Bab 3's dataset table (§3.3) now uses E-SERAVD v1.1's verified count — 1,200 clips, exactly 300
  per class across 4 classes — confirmed directly against `Dataset_Specification` during the
  pivot's claude.ai session. This supersedes the old draft's ~5,049 figure, which was never
  sourced to official documentation in the first place (its origin was never pinned down). See
  `SKRIPSI_CONTEXT.MD` §4.1 if this needs re-verifying independently.
- Bab 4–7 chapter titles are now decided (2026-09-02, Iterasi 7 in `CATATAN_PERBAIKAN.md`): Bab
  4 Perancangan, Bab 5 Implementasi, Bab 6 Hasil dan Pembahasan, Bab 7 Penutup — 7 chapters, not
  the 6 previously sketched. This is the user's own explicit instruction (not `SKRIPSI_CONTEXT.MD`
  boilerplate), given despite "Perancangan"/"Implementasi" usually signaling an implementatif
  thesis — read here as documenting the experiment's pipeline design and code, not a switch away
  from the non-implementatif analitik classification confirmed since the start (including an
  earlier explicit rejection of switching to implementatif). Only the Bab 1 §1.6 preview
  descriptions have been written so far — `chapters/bab4.tex`–`bab7.tex` themselves are still the
  old generic-template scaffold, not yet touched to match.
- Bab 2's Kajian Pustaka needed a citation specifically about split-scheme/data-leakage effects
  (Iterasi 7), and again a citation on SSL-vs-handcrafted robustness under leakage specifically
  (Iterasi 8). WebSearch/WebFetch failed every attempt in this session (2026-09-02, five tries
  across two rounds, all the same backend-model error — a tool/environment issue, not a query
  problem; WebFetch got past a DOI redirect once before hitting the same error on the actual
  content-processing step, confirming it's the shared summarization backend, not the network
  fetch). The user separately ran the search in a claude.ai chat session (which had working
  search) and pasted back a fully-drafted citation set (5 new refs: Antoniou et al. 2023, Kapoor
  and Narayanan 2023, Lashkarashvili et al. 2024, Wang et al. 2023, Wu et al. 2024 — added to
  Bab 1 Latar Belakang, Bab 2 §2.3 and §2.10, and Tabel 2.1). This Claude Code session could not
  independently re-verify those citations' content (same tool outage) — the "cari teks ini"
  anchors in the user's draft were checked against the live files and matched exactly before
  applying, but the citation *content* itself is trusted at the same level as `SKRIPSI_CONTEXT.MD`
  research, not independently confirmed here. Iterasi 8 also removed Eljinini and Al-Momani
  (2025) from Tabel 2.1 (least specific to either leakage or the Indonesian-SOTA argument) to
  rebalance the table toward leakage-relevant work instead of general SSL history. Worth retrying
  WebSearch/WebFetch here directly once it's
  working, or ask the user to supply citations directly.
- Bab 2's figures (SER/SSL/Data Leakage/Hand-Crafted/MFCC/CNN/Wav2Vec2/XLSR-53/LoRA/K-Fold) are
  still custom TikZ diagrams, not the original figures from the source papers. User said they'll
  upload source images themselves (Iterasi 7) to swap in via `\includegraphics` — none had
  arrived as of 2026-09-02. Don't reproduce copyrighted paper figures by fetching them from the
  web without the user's explicit say-so; wait for the upload.
- Bab 2 and Bab 3 (only — not Bab 1) follow a strict no-cross-reference rule since Iterasi 8:
  every subbab must be self-contained, restating what it needs rather than pointing elsewhere
  with "(lihat Subbab X.Y)" or "sebagaimana diuraikan pada Bab Z". Bab 1 is exempt and still
  points into Bab 3 (e.g. "lihat Subbab 3.3" for the film-distribution numbers). Keep this in
  mind for any future edit to Bab 2/3 — don't reintroduce cross-references there by habit.
- `thesis/frontmatter/lembar_pengesahan.tex`, `pernyataan_keaslian.tex`, `abstrak.tex`,
  `kata_pengantar.tex` are still generic-template content, not yet adapted to Filkom UB v3.0
  wording the way Bab 1–3 and the cover were. Not urgent — only pulled in by `main_skripsi.tex`.

## Experiments → thesis convention

Results cited in Bab 5–6 must trace back to a specific run: config in
`experiments/configs/`, output in `experiments/results/<eksperimen>/`. Figures/tables
copied into `thesis/assets/` for use in the manuscript should keep a name that maps back
to the `results/<eksperimen>/` that produced them.
