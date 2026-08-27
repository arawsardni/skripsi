# 🎓 Indonesian University Thesis LaTeX Template

A clean, highly organized LaTeX thesis manuscript, adapted from a generic Indonesian
university template to comply with **Panduan Skripsi Filkom UB v3.0** (4-3-3-3 margins,
single spacing, Calibri/Carlito typography). See [`thesis/CATATAN_PERUBAHAN.md`](thesis/CATATAN_PERUBAHAN.md)
for the full list of deviations from the generic template and why.

## ✨ Key Features

- **Standardized Layout:** A4 paper with precise margins (Left 4 cm; Top, Right, Bottom 3 cm).
- **Typography:** Calibri 12pt font (Carlito as a metric-compatible fallback where Calibri isn't installed), single line spacing, 1.27 cm paragraph indentation.
- **Pre-configured Chapters:** A 7-chapter structure, currently scoped to Bab 1–3 (proposal stage).
- **Figures & Tables:** Centered captions, bold labels, and beautifully formatted tables (`booktabs`).
- **Diagrams & Charts:** Out-of-the-box support for `tikz` flowcharts and `pgfplots` data visualizations with predefined clean color palettes.
- **Code Listings:** Professional syntax highlighting for source code (`listings`).
- **Citation:** Harvard-style bibliography (supervisor-approved variant, not Harvard ARU).

## 📁 Project Structure

This repo holds both the thesis manuscript and the research code behind it, kept in
separate top-level folders since they use different toolchains:

```text
skripsi/
├── thesis/                         # LaTeX manuscript — see below
│   ├── preamble.tex                 # Shared packages/styles — \input by both entry points below
│   ├── main_proposal.tex            # Compile this now: Bab 1-3 + referensi + lampiran
│   ├── main_skripsi.tex             # Compile this later: full skripsi (not the current focus)
│   ├── frontmatter/                # Pre-chapter pages
│   │   ├── cover.tex                   # Title page — shared, label set by whichever main file compiles
│   │   ├── lembar_pengesahan.tex       # Approval page (main_skripsi.tex only)
│   │   ├── pernyataan_keaslian.tex     # Declaration of originality (main_skripsi.tex only)
│   │   ├── abstrak.tex                 # Abstract, ID & EN (main_skripsi.tex only)
│   │   └── kata_pengantar.tex          # Acknowledgements (main_skripsi.tex only)
│   ├── chapters/                   # Main content chapters
│   │   ├── bab1.tex                    # Bab 1: Pendahuluan
│   │   ├── bab2.tex                    # Bab 2: Landasan Kepustakaan
│   │   ├── bab3.tex                    # Bab 3: Metodologi Penelitian
│   │   └── bab4.tex … bab7.tex          # Placeholder — count/titles not finalized, see Status below
│   ├── backmatter/                 # Post-chapter pages
│   │   ├── referensi.tex               # References / Bibliography
│   │   ├── lampiran_a.tex              # Required appendix: Persyaratan Fisik dan Tata Letak
│   │   └── lampiran_b.tex              # Required appendix: Penggunaan Bahasa
│   └── assets/                     # Images and graphics
│       └── ub-logo-small.png           # Your university logo here
└── experiments/                    # Research code backing Bab 3–6 (see experiments/README.md)
    ├── data/                           # raw/ + processed/ (gitignored, not committed)
    ├── notebooks/                      # exploratory analysis
    ├── src/                            # reusable pipeline code
    ├── configs/                        # one config per experiment run
    └── results/                        # metrics/figures per run (committed — cited in the thesis)
```

## 🚀 Getting Started

### 1. Prerequisites
You will need a TeX distribution installed on your system. 
- **Windows:** MiKTeX or TeX Live
- **macOS:** MacTeX
- **Linux:** `sudo apt-get install texlive-full` (Ubuntu/Debian)

Alternatively, you can upload this entire folder to [Overleaf](https://www.overleaf.com/) to work entirely in the browser without installing anything!

### 2. Compilation
There are two independent entry points — compile whichever document you need with
**XeLaTeX** (not pdflatex — the preamble uses `fontspec` for the Calibri font). Run it
**twice** so the table of contents and cross-references are accurately generated.

```bash
cd thesis
xelatex -interaction=nonstopmode main_proposal.tex   # the proposal — compile this now
xelatex -interaction=nonstopmode main_proposal.tex

xelatex -interaction=nonstopmode main_skripsi.tex    # the full skripsi — not needed yet
xelatex -interaction=nonstopmode main_skripsi.tex
```

Both share the same `preamble.tex` and the same `chapters/bab1-3.tex`, so there's nothing to
toggle by hand — either PDF can be regenerated at any time, even after work has moved on to
writing Bab 4+.

## 🛠️ Status

Cover, Bab 1–3, the reference list, and Lampiran A/B are filled in for `main_proposal.tex`
(based on an approved praproposal). Known remaining gaps:
- Bab 3 §3.9.2 (Perangkat Keras) is still a `[...]` placeholder pending the final GPU/compute spec.
- Bab 4–7's chapter count and titles aren't finalized — Panduan Skripsi Filkom UB v3.0 suggests
  Hasil/Pembahasan/Penutup (or Hasil+Pembahasan merged) for this thesis's research type, but
  titles are explicitly allowed to be thematic instead. Decide this once the proposal is approved.
- `main_skripsi.tex`'s frontmatter (lembar pengesahan, pernyataan keaslian, abstrak, kata
  pengantar) is still generic-template content — not urgent while the proposal is the focus.

## 📐 Formatting Reference

| Setting | Value |
| :--- | :--- |
| **Paper Size** | A4 |
| **Font** | Calibri (12pt) — Carlito fallback if unavailable |
| **Line Spacing** | Single |
| **Margins** | Left: 4 cm, Right: 3 cm, Top: 3 cm, Bottom: 3 cm |
| **Paragraph Indent** | 1.27 cm |
| **Table Captions** | Above the table, bold, centered |
| **Figure Captions** | Below the figure, bold, centered |

---
*Happy Writing! Semoga sukses sidangnya! 🎓*
