# results

Output tiap run eksperimen: metrics (json/csv), figures (png/pdf), log ringkas. Berbeda
dari `data/`, isi folder ini **di-commit** — ukurannya kecil dan jadi bukti langsung untuk
angka-angka yang dikutip di Bab 5–6 saat sidang.

Kelompokkan per eksperimen, jangan ditimpa:

```
results/
└── eksperimen-a/
    ├── metrics.json
    ├── confusion_matrix.png
    └── run.log
```

Jangan taruh artefak besar di sini (model checkpoint, dump data). Kalau butuh menyimpan
itu, simpan di luar git (mis. Drive/cloud storage) dan cukup catat lokasinya di sini.
