# Konteks Skripsi: SER Indonesia — Strategi Fine-Tuning Dua Tahap

> **Cara pakai:** Dokumen ini merangkum hasil diskusi perencanaan riset (dilakukan di Claude.ai, bukan di sesi ini) supaya bisa dipahami tanpa histori chat aslinya. Baca **Bagian 6** dulu — itu daftar temuan yang harus ditindaklanjuti saat menulis/merevisi Bab 1-3, bukan cuma latar belakang bacaan.

## 1. Identitas & Metadata

| Field                   | Isi                                                                                                             |
| ----------------------- | --------------------------------------------------------------------------------------------------------------- |
| Nama                    | Gaung Taqwa Indraswara                                                                                          |
| NIM                     | 235150207111043                                                                                                 |
| Jurusan/Prodi           | Teknik Informatika, FILKOM UB                                                                                   |
| Bidang Skripsi          | Komputasi Cerdas (KC)                                                                                           |
| Jenis / Tipe Penelitian | Non-implementatif — Analitik (Analytical/Explanatory)                                                           |
| Calon Pembimbing        | Tirana Noor Fatyanosa (Bu Tirana)                                                                               |
| Judul                   | _Analisis Pengaruh Strategi Fine-Tuning Dua Tahap terhadap Akurasi Pengenalan Emosi Ucapan Berbahasa Indonesia_ |

## 2. Status Progres

- **Praproposal**: sudah **disetujui** Bu Tirana (form resmi 7 halaman: Latar Belakang, Landasan Kepustakaan, Rumusan Masalah, Metode, Daftar Pustaka).
- **Proposal skripsi Bab 1-3** (LaTeX, ~17 halaman, base template `github.com/arawsardni/skripsi`): draf sudah pernah dibuat di sesi terpisah — **belum tentu mencerminkan keputusan desain terbaru** (lihat Bagian 6, terutama soal desain 4-kondisi dan metrik evaluasi). Perlu dicek/disinkronkan.
- **Section 3.9.2 (Perangkat Keras)**: masih placeholder. Draf kontennya sudah siap, lihat Bagian 3.5.

## 3. Inti Riset — Sudah Dikunci, Jangan Diubah Substansinya

### 3.1 Rumusan Masalah (verbatim dari praproposal yang disetujui)

1. Bagaimana perbedaan akurasi dan F1-score klasifikasi emosi antara model yang dilatih langsung (single-stage) pada dataset emosi berbahasa Indonesia dan model yang melalui fine-tuning dua tahap (pra-latih pada korpus emosi berbahasa Inggris, lalu diadaptasi ke data Indonesia)?
2. Apakah terdapat perbedaan performa yang signifikan secara statistik antara strategi full fine-tuning dan strategi fine-tuning efisien-parameter (LoRA) pada tahap adaptasi ke data target?
3. Bagaimana konsistensi hasil klasifikasi emosi antar-fold pada skema validasi silang untuk masing-masing strategi fine-tuning yang diuji?

### 3.2 Gap Penelitian (verbatim)

> "Celah penelitian terletak pada belum adanya studi yang membandingkan strategi fine-tuning satu tahap versus dua tahap dan full fine-tuning versus LoRA pada SER Indonesia secara sistematis dan teruji statistik."

### 3.3 Desain Eksperimen: 2×2 Factorial, 4 Kondisi (dikunci di sesi diskusi)

| Kondisi | Stage        | Metode Adaptasi  |
| ------- | ------------ | ---------------- |
| A       | Single-stage | Full Fine-Tuning |
| B       | Single-stage | LoRA             |
| C       | Two-stage    | Full Fine-Tuning |
| D       | Two-stage    | LoRA             |

- Backbone **sama persis** (XLSR-53) di keempat kondisi — yang berbeda cuma resep fine-tuning-nya. Ini disengaja supaya perbedaan hasil bisa ditarik ke strategi fine-tuning, bukan ke arsitektur.
- Alasan pakai 2 axis (bukan 1): axis stage menguji nilai cross-lingual transfer itu sendiri; axis metode menguji efisiensi adaptasi. Menguji dua-duanya bareng menjawab "seberapa murah cara paling efisien" — bukan cuma "apakah staging membantu" (yang sudah dijawab sebagian oleh Santoso et al. 2026 tanpa uji statistik).
- **PENTING**: pastikan Bab 3 yang tertulis di repo ini benar-benar mendeskripsikan 4 kondisi ini secara eksplisit (termasuk single-stage dipecah full-FT/LoRA), bukan versi ambigu sebelumnya di mana single-stage cuma disebut "dilatih langsung" tanpa spesifikasi metode.
- k-fold cross-validation diterapkan di level adaptasi target (bukan di source-stage); source-stage fine-tuning (Tahap 3) idealnya cukup sekali, dipakai ulang sebagai starting point untuk semua fold pada kondisi C & D.

### 3.4 Backbone & Data

- **Backbone**: Wav2Vec2/XLSR-53 (`facebook/wav2vec2-large-xlsr-53`), ~317 juta parameter, 24 layer Transformer, self-supervised, dilatih ~56 ribu jam di 53 bahasa (termasuk Indonesia, lewat komponen CommonVoice).
- **Kenapa XLSR-53** (bukan XLS-R 128-bahasa yang lebih baru): konsistensi dengan Nugroho et al. (2025) dan ekosistem model turunan Indonesia yang sama-sama berbasis XLSR-53; bukan soal compute (XLS-R 0.3B skalanya sama).
- **Belum terverifikasi**: apakah "XLSR-Wav2Vec2" yang disebut Nugroho et al. (2025) benar-benar checkpoint `wav2vec2-large-xlsr-53` yang sama persis. "Checkpoint monolingual Indonesia" yang mereka sebut kemungkinan besar adalah turunan XLSR-53 yang sudah di-fine-tune ke Indonesia (bukan model berbeda) — tapi ini dugaan, bukan konfirmasi dari paper aslinya.
- **Data sumber (Tahap 3)**: korpus emosi Inggris — RAVDESS/CREMA-D.
- **Data target (Tahap 4)**: dataset SER Indonesia — E-SERAVD/IndoWaveSentiment, 4 kelas emosi. Skala dataset terkait (dari tim yang sama, kemungkinan besar E-SERAVD atau versi awalnya): **5.049 sampel** (1.202 marah, 1.228 senang, 2.075 netral, 899 sedih).

### 3.5 Hardware (draf untuk Section 3.9.2)

- **Perangkat**: 2× NVIDIA RTX 3060 12GB (server lab, dipinjamkan).
- **Kecukupan VRAM per model** (~317 juta parameter):
  | Komponen | Full Fine-Tuning | LoRA |
  |---|---|---|
  | Bobot + master + gradien + optimizer Adam | ~5 GB | ~0,6–0,7 GB |
  | Sisa untuk aktivasi (dari 12 GB) | ~7 GB | ~11 GB |
  - Kedua skema feasible di 12GB. Divalidasi silang dengan riset yang secara spesifik memakai RTX 3060 12GB untuk fine-tuning model sekeluarga wav2vec pada ASR bahasa rendah-sumber-daya tanpa kendala memori.
- **Strategi 2 GPU**: bukan distributed training (model sudah muat di 1 kartu) — jalankan **2 run independen paralel**, satu per GPU (`CUDA_VISIBLE_DEVICES`), karena grid eksperimen (4 kondisi × k fold) itu embarrassingly parallel.
- **Rekomendasi teknis**: mixed precision (fp16/bf16), bekukan CNN feature extractor (praktik standar wav2vec2), batch size awal 8–16 (LoRA) / 4–8 (full-FT), gradient checkpointing sebagai cadangan (kemungkinan besar tidak diperlukan).
- Dengan skala dataset ~5 ribu sampel, estimasi realistis per-run: puluhan menit–2 jam (bukan berjam-jam), jadi total waktu penelitian (k=5 → ~21 run) selesai dalam hitungan hari dengan 2 GPU paralel.

## 4. Latar Belakang — Alur Logika (untuk Bab 1)

Struktur piramida yang dipakai (luas → sempit):

1. Emosi = inti komunikasi manusia; penting untuk interaksi manusia-mesin yang personal (Dar & Delhibabu, 2024).
2. Suara = kanal emosi yang relatif "jujur" (spontan, sulit disembunyikan) → dasar berkembangnya SER.
3. Masalah: bias linguistik (Bella et al., 2023) — teknologi bahasa timpang, Indonesia under-resourced, khususnya untuk data ucapan beranotasi emosi.
4. Solusi yang menjanjikan: cross-lingual transfer (Wu et al., 2021).
5. Celah: penerapan spesifik ke SER Indonesia — skema fine-tuning mana yang tepat — belum diuji sistematis. → jadi rumusan masalah.

## 5. Landasan Kepustakaan — 4 Kelompok Pembanding (untuk Bab 2)

| Peneliti                                      | Persamaan                                        | Perbedaan/Gap                                                                     |
| --------------------------------------------- | ------------------------------------------------ | --------------------------------------------------------------------------------- |
| Eljinini & Al-Momani (2025)                   | Fondasi representasi self-supervised             | Umum lintas tugas, tidak menyentuh emosi/Indonesia                                |
| Nugroho et al. (2025)                         | Representasi self-supervised untuk SER Indonesia | Checkpoint hasil pra-latih ASR biasa, bukan fine-tuning korpus emosi bahasa asing |
| Santoso, Budianto & Dutono (2026)             | Skema fine-tuning dua tahap                      | 1 konfigurasi terbaik saja, tanpa perbandingan sistematis/uji statistik           |
| Arisaputra & Zahra (2023); Tang et al. (2023) | Pemanfaatan cross-lingual transfer               | Tidak menyasar korpus emosi Inggris → data emosi Indonesia secara spesifik        |

Catatan tambahan: dari 924 skripsi TI FILKOM open-access yang disurvei, belum ada satu pun yang mengangkat SER berbasis audio — penguat argumen novelty di level lokal UB.

## 6. ⚠️ TEMUAN REVIEW KELAYAKAN/KECUKUPAN — ACTION ITEMS

Hasil pengecekan langsung terhadap **Panduan Skripsi FILKOM v3.0** (Bagian 2.3 Kelayakan, 2.4 Kecukupan, Lampiran A.1.1 kriteria khusus KC-Nonimplementatif). Ini bukan basa-basi bahasa — ini kekurangan isi yang perlu ditambahkan sebelum sidang:

- [ ] **Metrik evaluasi harus >2.** Rubrik KC-Nonimplementatif eksplisit: _"Terdapat lebih dari dua macam metode evaluasi yang digunakan."_ Saat ini cuma akurasi + F1-score. **Rekomendasi**: tambah **waktu training** dan **penggunaan memori/VRAM** — sekaligus menutup poin berikutnya.
- [ ] **Analisis kinerja komputasi belum ada.** Rubrik yang sama juga minta _"analisis kinerja komputasi (kompleksitas waktu, lamanya waktu eksekusi, penggunaan memori, dsb.)"_ selain parameter standar metode. Relevan langsung ke axis full-FT vs LoRA (soal efisiensi).
- [ ] **Hipotesis eksplisit (H0/H1) belum terlihat.** Wajib per kriteria "Hipotesis" penelitian simulasi/eksperimen KC, dan memang dibutuhkan Shapiro-Wilk + paired t-test/Wilcoxon. Contoh arah: H0 = tidak ada perbedaan performa signifikan antarstrategi.
- [ ] **Judul cuma menyebut "dua tahap"**, tidak menyebut axis full-FT vs LoRA yang sama pentingnya di gap statement. Siapkan pembelaan ("dua tahap" sebagai payung, full-FT/LoRA sebagai keputusan di titik adaptasi) atau pertimbangkan revisi judul.
- [ ] **RM2 perlu diperjelas** agar eksplisit mencakup KEDUA jalur (single-stage & two-stage), bukan cuma terbaca sebagai "di dalam jalur two-stage saja" ("...pada tahap adaptasi ke data target").
- [ ] **Cek Bab 2** sudah punya penjelasan **teori** (LoRA, k-fold CV, Shapiro-Wilk, paired t-test/Wilcoxon) sebagai subbab tersendiri — bukan cuma positioning terhadap 4 kelompok riset pembanding. Ini indikator terpisah di rubrik kelayakan landasan kepustakaan.
- [ ] **Cek keberadaan & kelengkapan** (wajib per Kelayakan Bagian Pendahuluan, TIDAK perlu ada di praproposal tapi WAJIB ada di proposal Bab 1-3 penuh): Tujuan, Manfaat Penelitian, Batasan Masalah, Sistematika Laporan, dan Jadwal Penelitian (maks 1 semester). File proposal aslinya tidak bisa dicek dari sesi diskusi tsb — perlu dicek manual di repo ini.
  - Draf kasar Batasan Masalah (belum diverifikasi, cuma saran awal): dibatasi 4 kelas emosi dasar sesuai dataset; backbone dibatasi XLSR-53 (tidak membandingkan arsitektur lain); eksperimen dijalankan di 2×RTX 3060 12GB.

## 7. Item Terbuka (Belum Diputuskan)

- **Manualisasi**: masih perlu dikonfirmasi ke Bu Tirana apakah wajib. Panduan v3.0 **tidak** menyebut "perhitungan manual" sebagai syarat resmi di manapun — kemungkinan besar konvensi pembimbing/penguji, bukan aturan fakultas (dicek dari 3 contoh skripsi: Rifky 2026 implementatif pakai LSTM mini 1 unit; Ulfah 2019 & Wahyu 2021 nonimplementatif pakai KNN/algoritma klasik skala kecil).
  - Manualisasi backbone XLSR-53 asli **tidak feasible** (317 juta parameter vs skala puluhan-ratusan yang lazim dimanualisasi).
  - Alternatif yang feasible & lebih relevan kalau tetap diminta: (a) manualisasi mekanisme LoRA vs full-FT di skala toy (matriks kecil), (b) manualisasi satu blok self-attention mini, (c) manualisasi uji statistik (Shapiro-Wilk/t-test/Wilcoxon) dengan angka contoh — opsi (c) paling nyambung ke kontribusi utama skripsi.
- **Checkpoint Nugroho et al. (2025)**: klaim "perbedaan" terhadap grup literatur ini bergantung pada checkpoint spesifik yang mereka uji — belum diverifikasi dari paper aslinya (lihat Bagian 3.4).

## 8. Daftar Pustaka (siap pakai, format UB Harvard)

```
Arisaputra, P. and Zahra, A., 2023. Indonesian automatic speech recognition with
XLSR-53. arXiv:2308.11589 [cs.CL].

Bella, G., Helm, P., Koch, G. and Giunchiglia, F., 2023. Towards bridging the digital
language divide. arXiv:2307.13405 [cs.CL].

Dar, G.H.M. and Delhibabu, R., 2024. Speech databases, speech features, and
classifiers in speech emotion recognition: a review. IEEE Access, 12,
pp.151122-151152. https://doi.org/10.1109/ACCESS.2024.3476960

Eljinini, M.A.H. and Al-Momani, W.E., 2025. Advancements in speech recognition:
a systematic review of deep learning transformer models, trends, innovations,
and future directions. IEEE Access. https://doi.org/10.1109/ACCESS.2025.3550855

Nugroho, K.S., Iriananda, S.W., Akbar, I., Isnan, M. and Pardamean, B., 2025.
Advancing end-to-end Indonesian speech emotion recognition using
XLSR-Wav2Vec2. In: 2025 International Conference on Information Technology
Research and Innovation (ICITRI). IEEE.
https://doi.org/10.1109/ICITRI67507.2025.11232655

Santoso, T.B., Budianto, A.N. and Dutono, T., 2026. Development of a speech
emotion recognition dataset for Indonesian. IEEE Access.
https://doi.org/10.1109/ACCESS.2026.3652377

Tang, D., Kuppens, P., Geurts, L. and van Waterschoot, T., 2023. End-to-end
transfer learning for speaker-independent cross-language and cross-corpus
speech emotion recognition. arXiv:2311.13678 [eess.AS].

Wu, P., Shi, J., Zhong, Y., Watanabe, S. and Black, A.W., 2021. Cross-lingual
transfer for speech processing using acoustic language similarity.
arXiv:2111.01326 [eess.AS].
```

## 9. Catatan Format Dokumen

- Bahasa Indonesia sepanjang dokumen.
- Kompilasi **XeLaTeX** (jalankan 2×), font **Carlito** (substitusi Calibri, sesuai Panduan FILKOM 3.6.1.3–3.6.1.4 hlm. 47 — ini solusi praktis, bukan aturan resmi), spasi tunggal, sitasi **UB Harvard**.
- Base template: `github.com/arawsardni/skripsi`. Package LaTeX yang dipakai: `fontspec`, `tikz`, `booktabs`.

## 10. Sumber Dokumen (tidak tersedia di repo ini)

Konteks di atas digali dari dokumen-dokumen di Claude Project terpisah (bukan repo ini): praproposal yang disetujui, `PanduanSkripsiFilkom-v3_0.pdf`, dan 3 contoh skripsi FILKOM (Rifky 2026 implementatif; Ulfah 2019 & Wahyu 2021 nonimplementatif). Kalau butuh detail lebih lanjut dari dokumen-dokumen itu (kutipan persis, nomor halaman, dsb.), balik ke sesi Claude.ai yang bersangkutan atau upload ulang file-nya di sini.
