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
xelatex -interaction=nonstopmode main_proposal.tex   # proposal: Bab 1-3 + referensi (no lampiran, see Structure)
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
- `thesis/main_proposal.tex` — entry point for the proposal (Bab 1–3 + referensi, no pengesahan/orisinalitas/abstrak/prakata/lampiran). This is the one to compile right now. Also owns the manual Daftar Isi/Daftar Gambar/Daftar Tabel heading block — see Key conventions.
- `thesis/main_skripsi.tex` — entry point for the full skripsi later (adds the four frontmatter pages and, eventually, Bab 4+). Not the current focus — its frontmatter files are still generic-template placeholders, see Known gaps.
- `thesis/frontmatter/cover.tex` — shared cover; `\doctypelabel`/`\doctypesubtitle` (set by whichever main file is compiled) control the "PROPOSAL SKRIPSI" vs "SKRIPSI" text, so this file itself never needs edits for that.
- `thesis/frontmatter/lembar_pengesahan.tex`, `pernyataan_keaslian.tex`, `abstrak.tex`, `kata_pengantar.tex` — only pulled in by `main_skripsi.tex`; not relevant while working on the proposal.
- `thesis/chapters/bab1–bab3.tex` — Pendahuluan, Landasan Kepustakaan, Metodologi Penelitian — filled in, current scope of `main_proposal.tex`.
- `thesis/chapters/bab4–bab7.tex` — still the generic placeholder scaffold, commented out in `main_skripsi.tex`. Chapter count/titles for this range are not yet decided — see Known gaps before assuming these four filenames are final.
- `thesis/backmatter/referensi.tex` — bibliography, Harvard style (the supervisor-approved variant
  used in the praproposal — not Harvard ARU; see `CATATAN_PERUBAHAN.md`).
- `thesis/backmatter/lampiran.tex` — the Lampiran page. Was an intentionally-empty placeholder page since Iterasi 2 (the Lampiran A/B text in the official template docx documents the template's *own* formatting/language conventions, not something to copy verbatim), but as of Iterasi 10 item 4 (2026-09-03, "hapus aja page lampiran") it's not `\include`d by `main_proposal.tex` at all anymore — the page itself is gone from the compiled proposal, not just emptied. The file is still on disk, still has a working `\chapter*{Lampiran}` + `\addcontentsline`, in case this gets reversed later. `lampiran_a.tex`/`lampiran_b.tex` similarly still exist with the original template content, not `\include`d by either entry point, kept only as reference.
- `thesis/assets/` — images referenced via `\includegraphics`; `\graphicspath{{assets/}}` set in preamble
- `thesis/CATATAN_PERUBAHAN.md` — every deviation from the generic template (font, spacing, compiler,
  chapter scope, terminology) made to comply with Panduan Skripsi Filkom UB v3.0, and why.
- `experiments/` — Python code, data, and results backing Bab 3–6; see `experiments/README.md` for the data/notebooks/src/configs/results convention

## Key conventions

- Equations numbered per-chapter: `(3.1)`, `(3.2)` — via `\numberwithin{equation}{chapter}`
- Code listings use `lstdefinestyle{kode}` with Indonesian caption name `Kode Sumber`
- TikZ flowchart node styles (`term`, `proc`, `procplain`, `io`, `dec`, `arr`) defined in preamble — use these for all diagrams. As of Iterasi 10, most Bab 2 conceptual figures were replaced by real images from source papers (see Known gaps) — TikZ is still the right tool for original diagrams like Bab 3's alur-metode-penelitian flowchart, just no longer the default for Bab 2's theory figures.
- `\gambarbesar{...}` macro for full-width/height-constrained figures — works for both TikZ pictures and `\includegraphics` content, wrap either the same way
- `siunitx` decimal marker set to `,` (Indonesian convention)
- Body text is single-spaced (Filkom UB v3.0, Lampiran A.4) — not the generic template's 1.5 spacing; `\parskip` adds 8pt of vertical space *between* paragraphs on top of that (Iterasi 10 item 3) — a separate axis from line spacing, not a contradiction
- Page numbering: cover = none, frontmatter = roman, main body = arabic
- Heading font sizes (Iterasi 10 item 4): chapter-level (`\chapter`) = 16pt, centered, uppercase; `\section` = 14pt; `\subsection` = 12pt. Set via explicit `\fontsize{}{}` in `preamble.tex`, not the class's relative size commands.
- **`\chapter*` gotcha**: `\titleformat{\chapter}` in `preamble.tex` only reliably restyles the *numbered* form. Direct `\chapter*{...}` calls (e.g. `referensi.tex`'s "Daftar Referensi") fall back to `\@makeschapterhead`, which is explicitly redefined right after the `\titleformat` block to match (centered/16pt/uppercase) — keep that redefinition if editing that area. `\tableofcontents`/`\listoffigures`/`\listoftables` are a *separate* problem: `tocloft` replaces their heading mechanism entirely and ignores both of the above, so as of Iterasi 10 their headings ("Daftar Isi"/"Daftar Gambar"/"Daftar Tabel") are written by hand in `main_proposal.tex` — an explicit centered/16pt/uppercase block followed by `\@starttoc{toc|lof|lot}` (the low-level call that actually reads the `.toc`/`.lof`/`.lot` file), bypassing `\tableofcontents` etc. entirely. Each of the three is preceded by its own `\clearpage` (Iterasi 12 item 1 — `tocloft` doesn't clearpage automatically like `\chapter*` does, so without it Daftar Gambar/Tabel can end up sharing a page). If a 4th unnumbered heading is ever needed, follow this same manual pattern rather than assuming any of the three mechanisms above will "just work" for it.
- **TOC entries uppercase** (Iterasi 12 item 2): not just the on-page headings but the *entries as listed inside Daftar Isi* ("BAB 1 PENDAHULUAN", "DAFTAR REFERENSI", etc.) are uppercase too. `\cftchapfont` etc. are font declarations, not text-transforming commands, so they can't apply `\MakeUppercase` to whatever text follows — the fix instead is at the source: `\chapter{\MakeUppercase{Pendahuluan}}` in bab1–3.tex, `\addcontentsline{toc}{chapter}{\MakeUppercase{Daftar Referensi}}` in referensi.tex, and the same for the three manual Daftar Isi/Gambar/Tabel entries in `main_proposal.tex`. Nesting is harmless (`\MakeUppercase` of already-uppercase text is a no-op) since the on-page heading *also* uppercases via `\titleformat{\chapter}`'s 4th argument.
- TOC (`tocloft`, added Iterasi 10, spacing retightened Iterasi 12 item 3 then again Iterasi 13 item 1): chapter entries show a "BAB X" prefix (`\cftchappresnum`) with dotted leaders enabled at chapter level too (`\cftchapdotsep`), dots set denser than default (`\cftdotsep=1`), vertical gap before entries set to 0pt at every level that appears in this document — chapter (`\cftbeforechapskip`), section (`\cftbeforesecskip`), figure (`\cftbeforefigskip`), table (`\cftbeforetabskip`; the last three are easy to forget since tocloft's defaults for them don't match the chapter-level default and each needed its own explicit override) — and nothing in the TOC is bold (`\cftchapfont`/`\cftchappagefont` forced to `\normalfont`). Daftar Isi lists itself as the first entry via a manual `\addcontentsline{toc}{chapter}{...}` right before its `\@starttoc{toc}` call (see the gotcha above).
- **Space above chapter headings** (Iterasi 13 item 2): `\titlespacing*{\chapter}{0pt}{0pt}{20pt}`'s "before" value of `0pt` was *not* actually zero visible space — titlesec/the class still reserved a visible gap above "BAB 1 PENDAHULUAN" etc. beyond the page's top margin, for reasons not fully root-caused. Fixed empirically by making the "before" value negative: `\titlespacing*{\chapter}{0pt}{-40pt}{20pt}`. This was tuned by rendering and visually comparing against the top margin, not derived analytically — if the page geometry, font size, or `\baselineskip` for `\chapter` ever changes, re-check this value rather than assuming `-40pt` still lands flush.
- Tables (Iterasi 10 item 11, sizing revised same iteration by user feedback): all tables use `longtable` with a full `|c|c|...|` grid (vertical rules + `\hline` after every row), not `booktabs`/`adjustbox` — so a table that overflows a page break cleanly to the next one, repeating its header row via `\endfirsthead`/`\endhead`. Template is in a comment block in `preamble.tex` right above where `booktabs` is loaded (that package is still loaded but no longer used by any table in the manuscript — harmless to leave). Column widths use `p{N\textwidth}` fractions, not fixed cm — for an N-column bordered table, keep the fractions' sum comfortably under `\textwidth` minus the tabcolsep/rule overhead (overhead = `N × 2 × \tabcolsep` + a fraction of a point per rule; default `\tabcolsep`=6pt) or XeLaTeX reports an overfull hbox on the alignment — widening `\tabcolsep` (see below) shrinks the safe fraction sum accordingly. Tabel 2.1 (Kajian Pustaka, dense multi-sentence cells) is wrapped in `{\small ... }` to fit more per line and cut down on ragged/underfull-looking wrapped lines; Tabel 3.1–3.3 (short cells) are each wrapped in a local `{\renewcommand{\arraystretch}{1.3}\setlength{\tabcolsep}{N pt} ... }` group instead, to look less cramped without shrinking text. To center a header cell's text in a `p{}` column, prefix it with `\centering\arraybackslash` (not just `\centering` — without `\arraybackslash`, `\centering`'s local redefinition of `\\` swallows the row-ending `\\` and produces "Misplaced \noalign"/"Misplaced \omit" errors on the following `\hline`).
- Figure captions (Iterasi 10 item 3, format finalized Iterasi 12 item 4): when a figure is reproduced from an external source, the attribution goes on its own `\normalfont Sumber : ...` line placed *after* `\caption{}`/`\label{}`, not inside the caption text — captions are bold (`textfont=bf` in `\captionsetup`) and the source line must not be. Format is literally `Sumber : ` (space before the colon) followed by `Author and Author (Year)` or `Author et al. (Year)` for 3+ authors — matching the in-text citation style used everywhere else in the manuscript, e.g. `\normalfont Sumber : Alashban and Alotaibi (2023)`. All 7 of Bab 2's reproduced figures use this now; see Known gaps for how the 6 that initially only had a title got their real author/year.
- Terminology (Iterasi 10 item 10, unified across Bab 1–3): use "fitur \emph{hand-crafted}" (not "fitur tangan") and "representasi \emph{self-supervised}" (not "representasi pralatih") consistently — the English term italicized, Indonesian noun head kept. Keep new prose consistent with this; don't reintroduce the old Indonesian calques.
- Sistematika Pembahasan (Bab 1 §1.6, revised Iterasi 10 item 5): each "Bab X ..." bold label is followed by a blank line (real paragraph break, not `\\`) before its description, and the description's first word is capitalized ("Membahas...", "Menyajikan...", "Memuat...") since it now reads as the start of a new sentence/paragraph, not a continuation.

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
- Bab 3 §3.8 (Peralatan Pendukung, software/hardware listing) was removed from the proposal
  entirely on 2026-09-03 (Iterasi 10 item 14, "hapus aja dulu peralatan pendukung") — the RAM/
  storage `[...]` placeholder that used to live there is gone along with the rest of the section,
  not fixed. The user's wording ("dulu", "for now") suggests this section is meant to come back
  later once real specs/software choices are settled, not a permanent deletion — if it returns,
  it'll need the RAM/storage line filled in for real at that point.
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
- Bab 2's figures for SER, MFCC, CNN, Self-Supervised, Wav2Vec2, LoRA-vs-full-fine-tuning, and
  K-Fold are, as of Iterasi 10 (2026-09-03), real images the user uploaded to `thesis/assets/`
  (`SER-Overview-Der-Delbabu-2024.png`, `MFCC-Block-DIagram.png`, `Basic-CNN-Diagram.png`,
  `Self-Supervised-Diagram.png`, `WAV2VEC2-Diagram.png`,
  `Regular-Fine-TUne-VS-LoRA-diagram-.png`, `K-Fold-Diagram.png` — note the inconsistent
  capitalization/trailing dash in some filenames, copy them exactly), not TikZ anymore. Data
  Leakage/Group Leakage, Fitur Hand-Crafted, and XLSR-53 still have no figure (never did, never
  requested). Some of the new images depict a meaningfully different thing than the old TikZ did
  (SER's is now a database/feature/classifier taxonomy, not a linear pipeline; K-Fold's is a
  generic illustration, not specifically $k=5$ or $k=6$) — the surrounding prose in `bab2.tex` was
  rewritten to match what each image actually shows, not just swap the `\includegraphics` call.
  Captions attribute each to its source with a "Sumber : Author and Author (Year)" line below the
  caption (not bold, not inside `\caption{}` — see Key conventions). Iterasi 10 could only find
  paper *titles* (WebSearch/WebFetch failed, 6th consecutive failure this engagement, same
  backend-model error each time) so captions briefly cited by title; Iterasi 12 (2026-09-03)
  resolved this properly by using the **Browser tool** (`mcp__Claude_Browser__*`, i.e. a real
  browser doing Google/dblp/arXiv searches) instead of WebSearch/WebFetch — it hit none of that
  backend error and found all 6 real author/year pairs in a few searches. **If WebSearch/WebFetch
  ever fail again in this repo, try the Browser tool before giving up** — it's a genuinely
  different pipeline, not just a retry, and worked cleanly here. The 6 new citations (Alashban and
  Alotaibi 2023; Ashfaque and Iqbal 2019; Del Pup and Atzori 2023; Lu et al. 2024; Ovalle et al.
  2024; Phung and Rhee 2019) are now in `referensi.tex` in their alphabetical slots, cross-checked
  against dblp/arXiv/IEEE Xplore/Google Scholar (multiple independent sources agreed on author
  names and years) — trusted at a similar confidence level to the rest of the bibliography, no
  verification flag added. The 7th (SER overview) reuses the existing Dar and Delhibabu (2024)
  citation, unchanged.
- Bab 2 and Bab 3 (only — not Bab 1) follow a strict no-cross-reference rule since Iterasi 8:
  every subbab must be self-contained, restating what it needs rather than pointing elsewhere
  with "(lihat Subbab X.Y)" or "sebagaimana diuraikan pada Bab Z". Bab 1 is exempt and still
  points into Bab 3 (e.g. "lihat Subbab 3.3" for the film-distribution numbers). Keep this in
  mind for any future edit to Bab 2/3 — don't reintroduce cross-references there by habit.
- `thesis/frontmatter/lembar_pengesahan.tex`, `pernyataan_keaslian.tex`, `abstrak.tex`,
  `kata_pengantar.tex` are still generic-template content, not yet adapted to Filkom UB v3.0
  wording the way Bab 1–3 and the cover were. Not urgent — only pulled in by `main_skripsi.tex`.
- Title wording: the user hand-edited `cover.tex` and Bab 1 §1.6 directly (outside any reviewed
  round) to "Pengaruh Skema Pembagian Data Berbasis Film **pada Dataset E-SERAVD** terhadap
  Performa Klasifikasi Pengenalan Emosi Ucapan Berbahasa Indonesia" — note "pada Dataset
  E-SERAVD" moved right after "Berbasis Film" instead of trailing at the end, and "Performa
  Klasifikasi Pengenalan Emosi" instead of "Akurasi Pengenalan Emosi". `preamble.tex`'s `\title{}`
  and `pdftitle=` were out of sync with this (still had the older "...terhadap Akurasi...pada
  Dataset E-SERAVD" wording from Iterasi 8) until Iterasi 10 brought them in line. If the title
  changes again, remember it's declared in three places: `preamble.tex` (`\title`, `pdftitle`),
  `cover.tex`, and Bab 1 §1.6's Sistematika Pembahasan sentence — keep all three in sync.

## Experiments → thesis convention

Results cited in Bab 5–6 must trace back to a specific run: config in
`experiments/configs/`, output in `experiments/results/<eksperimen>/`. Figures/tables
copied into `thesis/assets/` for use in the manuscript should keep a name that maps back
to the `results/<eksperimen>/` that produced them.
