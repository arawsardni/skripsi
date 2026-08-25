# Experiments

Kode dan data eksperimen yang mendasari Bab 3–6 di [`../thesis`](../thesis). Terpisah dari
`thesis/` karena toolchain-nya beda (Python vs LaTeX) dan supaya `thesis/` tetap berisi
naskah saja.

## Struktur

- `data/raw/` — dataset asli, tidak diubah. Tidak di-commit (lihat README di dalamnya).
- `data/processed/` — hasil praproses, dibangkitkan ulang dari `raw/` lewat skrip di `src/`. Tidak di-commit.
- `notebooks/` — eksplorasi cepat / analisis sekali-pakai. Bukan sumber kebenaran.
- `src/` — kode pipeline yang reusable: loading data, preprocessing, model, evaluasi.
- `configs/` — satu file config per skenario eksperimen (hyperparameter, seed, versi data).
- `results/` — output tiap run: metrics, figures, logs. Di-commit (kecil & jadi bukti untuk sidang).

## Alur kerja

1. Coba ide di `notebooks/`.
2. Kalau sudah stabil, pindahkan jadi fungsi/modul di `src/`.
3. Jalankan eksperimen dengan config dari `configs/<nama-eksperimen>.yaml`.
4. Simpan output ke `results/<nama-eksperimen>/` (metrics + figures), jangan timpa run sebelumnya.
5. Figure/tabel yang dipakai di `thesis/`, salin ke `thesis/assets/` dengan nama yang masih
   bisa ditelusuri balik ke `results/<nama-eksperimen>/` yang menghasilkannya — supaya saat
   sidang ditanya "ini dari eksperimen mana", jawabannya ada di git history, bukan di ingatan.

## Setup

```bash
cd experiments
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```
