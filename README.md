# 🎓 Indonesian University Thesis LaTeX Template

A clean, highly organized LaTeX thesis manuscript, adapted from a generic Indonesian
university template to comply with **Panduan Skripsi Filkom UB v3.0** (4-3-3-3 margins,
single spacing, Calibri/Carlito typography). See [`thesis/CATATAN_PERUBAHAN.md`](thesis/CATATAN_PERUBAHAN.md)
for the full list of deviations from the generic template and why.

## ✨ Key Features

- **Standardized Layout:** A4 paper with precise margins (Left 4 cm; Top, Right, Bottom 3 cm).
- **Typography:** Carlito (Calibri-metric-compatible) 12pt font, single line spacing, 1.27 cm paragraph indentation.
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
│   ├── main.tex                    # The main file (Compile this!)
│   ├── frontmatter/                # Pre-chapter pages
│   │   ├── cover.tex                   # Title page / Cover
│   │   ├── lembar_pengesahan.tex       # Approval page
│   │   ├── pernyataan_keaslian.tex     # Declaration of originality
│   │   ├── abstrak.tex                 # Abstract (ID & EN)
│   │   └── kata_pengantar.tex          # Acknowledgements
│   ├── chapters/                   # Main content chapters
│   │   ├── bab1.tex                    # Bab 1: Pendahuluan
│   │   ├── bab2.tex                    # Bab 2: Landasan Kepustakaan
│   │   ├── bab3.tex                    # Bab 3: Metodologi Penelitian
│   │   ├── bab4.tex                    # Bab 4: Perancangan Sistem
│   │   ├── bab5.tex                    # Bab 5: Implementasi
│   │   ├── bab6.tex                    # Bab 6: Pengujian & Analisis
│   │   └── bab7.tex                    # Bab 7: Kesimpulan & Saran
│   ├── backmatter/                 # Post-chapter pages
│   │   └── referensi.tex               # References / Bibliography
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
To generate the PDF, compile `thesis/main.tex` with **XeLaTeX** (not pdflatex — the preamble
uses `fontspec` for the Carlito font). Run it **twice** so the table of contents and
cross-references are accurately generated.

```bash
cd thesis
xelatex -interaction=nonstopmode main.tex
xelatex -interaction=nonstopmode main.tex
```

## 🛠️ Status

Cover, Bab 1–3, and the reference list are filled in (based on an approved praproposal).
Known remaining gap: Bab 3 §3.9.2 (Perangkat Keras) is still a `[...]` placeholder pending
the final GPU/compute spec. Bab 4–7 stay as the generic placeholder scaffold, commented out
in `main.tex`, until the proposal is approved and the research is carried out.

## 📐 Formatting Reference

| Setting | Value |
| :--- | :--- |
| **Paper Size** | A4 |
| **Font** | Carlito / Calibri (12pt) |
| **Line Spacing** | Single |
| **Margins** | Left: 4 cm, Right: 3 cm, Top: 3 cm, Bottom: 3 cm |
| **Paragraph Indent** | 1.27 cm |
| **Table Captions** | Above the table, bold, centered |
| **Figure Captions** | Below the figure, bold, centered |

---
*Happy Writing! Semoga sukses sidangnya! 🎓*
