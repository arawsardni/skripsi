# Konteks Skripsi: SER Indonesia — Strategi Fine-Tuning Dua Tahap

> **Cara pakai:** Rangkuman diskusi perencanaan riset (Claude.ai, bukan sesi ini), supaya bisa dipahami tanpa histori chat asli. **Bagian 6** sekarang sebagian besar SUDAH SELESAI (bukan lagi murni to-do) — cek status di tiap poin. **Bagian 7** berisi keputusan yang masih terbuka dan paling butuh perhatian saat ini.

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

- **Praproposal**: disetujui Bu Tirana.
- **Proposal Bab 1-3**: draf sekarang sudah cukup matang — Rumusan Hipotesis (1.3), Tujuan (1.4), Manfaat (1.5), Batasan Masalah (1.6), Sistematika (1.7), Jadwal (1.8), teori lengkap Bab 2 (2.2–2.9), dan Tabel 4-kondisi di Bab 3 semuanya sudah ada. Lihat Bagian 6 untuk status detail per item.
- **Section 3.10.2 (Perangkat Keras)**: sudah ada isi (bukan placeholder lagi), TAPI berisi angka VRAM yang perlu dikoreksi — lihat Bagian 3.5.
- **Isu paling mendesak sekarang**: bukan lagi soal kelengkapan struktur, tapi soal **kelayakan teknis desain eksperimen** di hardware sebenarnya + timeline sebenarnya. Lihat Bagian 2.1 dan Bagian 7.

### 2.1 Timeline Resmi (dari sistem akademik, per 31 Agustus 2026)

| Tahapan                   | Deadline    | Sisa waktu   |
| ------------------------- | ----------- | ------------ |
| Penilaian Kemajuan 1 (P1) | 27 Okt 2026 | ~8 minggu    |
| Penilaian Kemajuan 2 (P2) | 11 Des 2026 | ~14,5 minggu |
| Pendaftaran Semhas/Ujian  | 22 Des 2026 | ~16 minggu   |

**Implikasi**: eksperimen inti (fine-tuning + evaluasi) idealnya selesai substansial sekitar P2, supaya masih ada waktu menulis Bab 4-6 dan bersiap Semhas. ~14 minggu adalah budget riil, bukan "1 semester longgar" seperti bunyi Jadwal Penelitian di Bab 1.

## 3. Inti Riset

### 3.1 Rumusan Masalah & Tujuan (revisi terbaru — ⚠️ belum ditulis ulang di draf)

**Perubahan dari versi sebelumnya**: RM1 tadinya menyebut metrik eksplisit ("akurasi dan F1-score") di level rumusan masalah. Setelah dibandingkan ke pola beberapa skripsi kating (RM pakai istilah generik "performa"/"kinerja"; metrik spesifik baru dirinci di Tujuan, bukan di RM), RM1 disederhanakan dan metrik spesifik dipindah ke Tujuan.

Rumusan Masalah (final):

1. Bagaimana perbedaan performa klasifikasi emosi antara model yang dilatih langsung (single-stage) pada dataset emosi berbahasa Indonesia dan model yang melalui fine-tuning dua tahap (pra-latih pada korpus emosi berbahasa Inggris, lalu diadaptasi ke data Indonesia)?
2. Apakah terdapat perbedaan performa yang signifikan secara statistik antara strategi full fine-tuning dan strategi fine-tuning efisien-parameter (LoRA) pada tahap adaptasi ke data target, **baik pada skema single-stage maupun two-stage**?
3. Bagaimana konsistensi hasil klasifikasi emosi antar-fold pada skema validasi silang untuk masing-masing strategi fine-tuning yang diuji?

Tujuan (final, selaras RM + sudah mengakomodasi koreksi interpretasi metrik di Bagian 6):

1. Menganalisis perbedaan performa klasifikasi emosi antara model single-stage dan model yang melalui fine-tuning dua tahap, diukur melalui akurasi, presisi, recall, dan F1-score.
2. Menguji signifikansi statistik perbedaan performa antara strategi full fine-tuning dan LoRA pada tahap adaptasi ke data target, serta membandingkan efisiensi komputasinya dari sisi waktu pelatihan dan penggunaan memori.
3. Mengevaluasi konsistensi hasil klasifikasi emosi antar-fold pada skema validasi silang untuk setiap strategi fine-tuning yang diuji.

### 3.2 Hipotesis (sudah ditambahkan ke draf, Subbab 1.3)

- RM1 — H0: tidak ada perbedaan performa signifikan antara model single-stage dan two-stage. H1: ada.
- RM2 — H0: tidak ada perbedaan performa signifikan antara full-FT dan LoRA (di kedua skema stage). H1: ada.
- RM3 bersifat deskriptif, tidak diuji sebagai hipotesis statistik formal.

### 3.3 Desain Eksperimen: 2×2 Factorial, 4 Kondisi

| Kondisi | Stage        | Metode Adaptasi  |
| ------- | ------------ | ---------------- |
| A       | Single-stage | Full Fine-Tuning |
| B       | Single-stage | LoRA             |
| C       | Two-stage    | Full Fine-Tuning |
| D       | Two-stage    | LoRA             |

Backbone sama persis (XLSR-53) di keempat kondisi. Sudah tertulis eksplisit di Tabel 3.2 draf saat ini — **status: SELESAI ditulis**, tapi cakupannya (tetap 4 kondisi atau disederhanakan) masih jadi keputusan terbuka, lihat Bagian 7.

### 3.4 Backbone & Data

- **Backbone**: `facebook/wav2vec2-large-xlsr-53`, ~317 juta parameter, 24 layer Transformer. Draf Subbab 3.4 sengaja memakai checkpoint XLSR-53 **mentah** (bukan turunan yang sudah di-fine-tune untuk Indonesia), supaya titik awal identik di keempat kondisi.
- **Soal checkpoint Nugroho et al. (2025)**: ✅ **RESOLVED, bukan lagi item terbuka.** Sempat jadi pertanyaan apakah checkpoint mereka sama persis dengan XLSR-53 — tapi karena desain sekarang sengaja TIDAK memakai checkpoint turunan siapa pun (raw XLSR-53 di semua kondisi), ambiguitas itu jadi tidak relevan lagi untuk desain sendiri. Cukup dicatat sebagai keterbatasan pembanding di Landasan Kepustakaan.
- **Data sumber (Tahap 3)**: RAVDESS (1.440 sampel, 8 kelas emosi, EN) dan/atau CREMA-D (7.442 sampel, 6 kelas emosi, EN).
- **Data target (Tahap 4)** — dua kandidat dengan skala **jauh berbeda**, ini penting untuk Bagian 7:
  - **E-SERAVD** (Santoso et al., 2026): ~5.049 sampel, 4 kelas emosi (marah 1.202, senang 1.228, netral 2.075, sedih 899).
  - **IndoWaveSentiment** (Nugroho et al., 2025): ~300 sampel dari 10 speaker, **5 kelas emosi** (bukan 4 — beda skema kelas dari E-SERAVD).
  - Draf Tabel 3.1 saat ini mencantumkan KEDUANYA sebagai target sekaligus — ini belum dikunci. Lihat rekomendasi di Bagian 7.

### 3.5 Hardware — ⚠️ PERLU KOREKSI DI DRAF

- **Perangkat yang benar**: 2× NVIDIA RTX 3060 **6GB** per kartu (**bukan 12GB per kartu** — 12GB itu total gabungan dua kartu, bukan kapasitas satu kartu). Draf Tabel 3.3 sudah pakai angka 6GB yang benar, tapi solusinya belum lengkap.
- **VRAM per kartu (~317 juta parameter)**:

  | Komponen                       | Full-FT (naif)      | Full-FT + 8-bit Adam | LoRA        |
  | ------------------------------ | ------------------- | -------------------- | ----------- |
  | Bobot+master+gradien+optimizer | ~5 GB               | ~1,9 GB              | ~0,6–0,7 GB |
  | Sisa aktivasi (dari 6GB)       | ~1 GB ⚠️ riskan OOM | ~4 GB                | ~5,3 GB     |

  Angka "naif" (fp32 Adam standar) di draf saat ini terlalu mepet untuk aman — overhead CUDA/fragmentasi bisa menghabiskan sisa 1GB sebelum sempat dipakai aktivasi. **Rekomendasi konkret yang perlu ditambahkan ke Subbab 3.10.2**: pakai optimizer 8-bit (library `bitsandbytes`) untuk kondisi full-FT (A & C), plus gradient checkpointing dan batch size kecil (1-2) dengan gradient accumulation. LoRA (B & D) aman tanpa perlakuan khusus.

- **Strategi 2 GPU**: 2 run independen paralel (`CUDA_VISIBLE_DEVICES`), bukan distributed training — model sudah muat di 1 kartu, grid eksperimen (kondisi × fold) bersifat embarrassingly parallel.
- **Soal waktu**: dengan skala dataset target yang kecil (300–5.000 sampel), compute time kemungkinan BUKAN bottleneck utama — risiko lebih besar ada di waktu implementasi/debug pipeline pada hardware yang memori-nya ketat, bukan jam GPU murni.

## 4. Latar Belakang — Alur Logika (Bab 1, sudah tertulis + ada paragraf pembelaan judul)

1. Emosi = inti komunikasi manusia; penting untuk interaksi manusia-mesin personal (Dar & Delhibabu, 2024).
2. Suara = kanal emosi relatif "jujur" → dasar SER.
3. Bias linguistik (Bella et al., 2023) — Indonesia under-resourced, khususnya data ucapan beranotasi emosi.
4. Solusi: cross-lingual transfer (Wu et al., 2021).
5. Celah: skema fine-tuning mana yang tepat untuk SER Indonesia belum diuji sistematis.
6. **Paragraf tambahan (baru)**: menjelaskan eksplisit bahwa "dua tahap" di judul adalah payung strategi utama (single vs two-stage), sementara full-FT vs LoRA adalah keputusan desain di titik adaptasi, diuji lewat RM2 dan desain 4-kondisi — ini jawaban siap pakai untuk pertanyaan penguji soal judul.

## 5. Landasan Kepustakaan — Kelompok Pembanding (Bab 2, sudah lengkap dengan tabel + teori)

| Peneliti                                      | Persamaan                                        | Perbedaan/Gap                                                                     |
| --------------------------------------------- | ------------------------------------------------ | --------------------------------------------------------------------------------- |
| Eljinini & Al-Momani (2025)                   | Fondasi representasi self-supervised             | Umum lintas tugas, tidak menyentuh emosi/Indonesia                                |
| Nugroho et al. (2025)                         | Representasi self-supervised untuk SER Indonesia | Checkpoint hasil pra-latih ASR biasa, bukan fine-tuning korpus emosi bahasa asing |
| Santoso, Budianto & Dutono (2026)             | Skema fine-tuning dua tahap                      | 1 konfigurasi terbaik saja, tanpa perbandingan sistematis/uji statistik           |
| Arisaputra & Zahra (2023); Tang et al. (2023) | Pemanfaatan cross-lingual transfer               | Tidak menyasar korpus emosi Inggris → data emosi Indonesia secara spesifik        |

Novelty lokal: dari 924 skripsi TI FILKOM open-access, belum ada yang mengangkat SER berbasis audio.

Bab 2 sekarang juga punya subbab teori mandiri (bukan cuma positioning): SER (2.2), Self-Supervised Learning (2.3), Wav2Vec2/XLSR (2.4), Cross-Lingual Transfer (2.5), Fine-Tuning Dua Tahap dengan formulasi θ1/θ2 (2.6), LoRA dengan formulasi BA (2.7), K-Fold CV & uji statistik (2.8), Metrik Evaluasi (2.9).

## 6. Status Temuan Review Kelayakan/Kecukupan (Panduan FILKOM v3.0, Lampiran A.1.1 KC-Nonimplementatif)

- [ ] **Metrik evaluasi >2 — interpretasi dikoreksi, akurasi+F1 kemungkinan belum cukup.** Rubrik memisahkan "metode evaluasi" dari "analisis kinerja komputasi" lewat kata "ditambah dengan" — dan daftar contoh ukuran di kriteria lain (Penyajian Hasil: akurasi, error rate, MSE, PSNR, precision, recall, f-measure, ROC, lift-ratio) semuanya metrik kualitas model, tidak ada waktu/memori di situ. Jadi "lebih dari dua metode evaluasi" kemungkinan besar merujuk ke metrik kualitas model saja — akurasi+F1 cuma 2, waktu+memori mungkin TIDAK terhitung ke sini (meski tetap wajib untuk syarat analisis kinerja komputasi). **Solusi**: tambah presisi & recall sebagai metrik yang dilaporkan sendiri → 4 metrik kualitas (akurasi, presisi, recall, F1). Perlu update judul+isi Subbab 2.9.1 jadi "Akurasi, Presisi, Recall, dan F1-Score" (bukan cuma sebut precision/recall sebagai komponen rumus F1 sekilas). Wording Tujuan yang sudah mengakomodasi ini ada di Bagian 3.1.
- [x] **Analisis kinerja komputasi (waktu, memori)** — SELESAI sebagai syarat terpisah (Subbab 2.9.2 teori, 3.7 pelaksanaan) — tetap dipertahankan apa adanya, cuma statusnya sebagai bagian dari hitungan ">2 metode evaluasi" yang dikoreksi di atas.
- [ ] **RM1 terlalu spesifik menyebut metrik.** Dibandingkan ke pola skripsi kating (RM pakai istilah generik "performa"/"kinerja", metrik baru dirinci di Tujuan) — RM1 saat ini menyebut "akurasi dan F1-score" eksplisit, perlu disederhanakan. Wording final ada di Bagian 3.1.
- [x] **Hipotesis eksplisit (H0/H1)** — SELESAI (Subbab 1.3).
- [x] **Pembelaan judul "dua tahap"** — SELESAI (paragraf tambahan di akhir Latar Belakang).
- [x] **RM2 diperjelas cakupan single & two-stage** — SELESAI.
- [x] **Bab 3 refleksikan desain 4-kondisi** — SELESAI (Tabel 3.2).
- [x] **Tujuan, Manfaat, Batasan Masalah, Sistematika, Jadwal** — SELESAI, semua subbab ada.
- [x] **Bab 2 teori mandiri (bukan cuma positioning)** — SELESAI (2.2–2.9).
- [ ] **BARU — koreksi VRAM 6GB belum lengkap.** Tabel 3.3 sudah pakai angka 6GB yang benar, tapi belum menyertakan mitigasi 8-bit Adam optimizer untuk kondisi full-FT — tanpa ini, sisa ~1GB aktivasi berisiko OOM. Lihat Bagian 3.5.
- [ ] **Implementasi pengukuran metrik belum eksplisit di Bab 3.** Perlu 1-2 kalimat teknis: `time.perf_counter()` untuk waktu pelatihan, `torch.cuda.max_memory_allocated()` (setelah `reset_peak_memory_stats()`) untuk puncak VRAM per fold.

## 7. Keputusan Terbuka (Prioritas Sekarang)

1. **Cakupan dataset target: 1 atau 2 dataset?** Draf Tabel 3.1 saat ini pakai E-SERAVD DAN IndoWaveSentiment sekaligus sebagai target. Tapi skalanya jauh berbeda (5.049 vs 300 sampel) DAN skema kelasnya beda (4 vs 5 kelas emosi) — menggabungkan/menjalankan keduanya menggandakan beban eksperimen di tengah timeline 14 minggu. **Rekomendasi**: pilih **satu** (E-SERAVD, karena paling terhubung ke narasi "dua tahap" lewat Santoso et al.), sebut yang lain sebagai arah pengembangan di Bab 6 Penutup. Belum dikunci ke Bu Tirana.
2. **Cakupan axis full-FT vs LoRA: tetap 4 kondisi atau disederhanakan?** Tiga opsi, belum diputuskan:
   - (a) Tetap 4 kondisi + tambahkan 8-bit Adam untuk full-FT — paling lengkap secara akademis, axis ini juga yang memberi isi ke metrik efisiensi (poin baru di atas).
   - (b) Sederhanakan ke 2 kondisi (single vs two-stage saja, metode difiksasi — kemungkinan LoRA karena lebih murah) — separuh beban, paling konservatif untuk timeline.
   - (c) Tetap 4 kondisi, tapi kurangi beban dari sisi lain (k lebih kecil, atau kunci ke 1 dataset dari poin 1) — jaga kekuatan akademis, potong biaya dari lever lain.
   - **Rekomendasi kerja**: (a) atau (c), karena axis ini yang membuat metrik efisiensi (waktu/memori) punya sesuatu untuk dibandingkan — tapi tetap perlu dikonfirmasi ke Bu Tirana dengan angka VRAM/waktu di atas sebagai dasar.
3. **Pilot run sebelum commit ke grid penuh.** Disarankan jalankan 1 kondisi × 1 fold dulu di hardware 6GB sebenarnya, catat waktu & VRAM riil, baru putuskan poin 2 berdasarkan data nyata — bukan estimasi di atas kertas.
4. **(Opsional, murah) Baseline frozen-backbone.** Backbone dibekukan total, hanya melatih classification head kecil di atasnya (linear probing) — hampir gratis secara compute karena tidak ada gradien untuk 317 juta parameter backbone. Bukan pengganti 4 kondisi (tidak menjawab RM manapun langsung), tapi berguna sebagai referensi "seberapa besar manfaat fine-tuning dibanding representasi mentah" kalau waktu memungkinkan.
5. **Manualisasi** — masih perlu dikonfirmasi ke Bu Tirana apakah wajib. Panduan v3.0 tidak menyebutnya sebagai syarat resmi di manapun; kemungkinan besar konvensi pembimbing, bukan aturan fakultas. Manualisasi backbone asli tidak feasible (317 juta parameter). Alternatif yang feasible: manualisasi mekanisme LoRA di skala toy, satu blok self-attention mini, atau (paling relevan) uji statistik (Shapiro-Wilk/t-test/Wilcoxon) dengan angka contoh.
6. **Catatan verifikasi literatur (untuk dicek ulang manual, jangan diasumsikan clean)**: sempat muncul sinyal ada paper "Parameter-Efficient Fine-Tuning Large Speech Model Based on LoRA" (Ling Ou dkk., IEEE 2024) yang preview pencariannya menyebut IndoWaveSentiment secara spesifik. Setelah ditelusuri ke abstrak resmi di IEEE Xplore, paper itu ternyata tentang LoRA untuk speech model secara umum, **tidak menyebut Indonesia/IndoWaveSentiment sama sekali** — kemungkinan besar artefak caching search engine, bukan gap yang nyata. Klaim bahwa belum ada yang uji full-FT vs LoRA untuk SER Indonesia kemungkinan besar masih valid, tapi worth dicek manual sekali lagi (coba cari langsung di Google Scholar) sebelum dianggap final.

## 8. Daftar Pustaka (lengkap, sesuai draf terkini, format UB Harvard)

```
Arisaputra, P. and Zahra, A., 2023. Indonesian automatic speech recognition with
XLSR-53. arXiv:2308.11589 [cs.CL].

Baevski, A., Zhou, H., Mohamed, A. and Auli, M., 2020. wav2vec 2.0: a framework
for self-supervised learning of speech representations. Advances in Neural
Information Processing Systems (NeurIPS), 33, pp.12449-12460.

Bella, G., Helm, P., Koch, G. and Giunchiglia, F., 2023. Towards bridging the digital
language divide. arXiv:2307.13405 [cs.CL].

Cao, H., Cooper, D.G., Keutmann, M.K., Gur, R.C., Nenkova, A. and Verma, R., 2014.
CREMA-D: crowd-sourced emotional multimodal actors dataset. IEEE Transactions
on Affective Computing, 5(4), pp.377-390.

Conneau, A., Baevski, A., Collobert, R., Mohamed, A. and Auli, M., 2021. Unsupervised
cross-lingual representation learning for speech recognition. In: Proceedings of
Interspeech 2021, pp.2426-2430.

Dar, G.H.M. and Delhibabu, R., 2024. Speech databases, speech features, and
classifiers in speech emotion recognition: a review. IEEE Access, 12,
pp.151122-151152. https://doi.org/10.1109/ACCESS.2024.3476960

Eljinini, M.A.H. and Al-Momani, W.E., 2025. Advancements in speech recognition:
a systematic review of deep learning transformer models, trends, innovations,
and future directions. IEEE Access. https://doi.org/10.1109/ACCESS.2025.3550855

Hu, E.J., Shen, Y., Wallis, P., Allen-Zhu, Z., Li, Y., Wang, S., Wang, L. and Chen, W.,
2022. LoRA: low-rank adaptation of large language models. In: International
Conference on Learning Representations (ICLR).

Kohavi, R., 1995. A study of cross-validation and bootstrap for accuracy estimation
and model selection. In: Proceedings of the 14th International Joint Conference
on Artificial Intelligence (IJCAI), 2, pp.1137-1143.

Liu, X., Zhang, F., Hou, Z., Mian, L., Wang, Z., Zhang, J. and Tang, J., 2023.
Self-supervised learning: generative or contrastive. IEEE Transactions on
Knowledge and Data Engineering, 35(1), pp.857-876.

Livingstone, S.R. and Russo, F.A., 2018. The Ryerson Audio-Visual Database of
Emotional Speech and Song (RAVDESS): a dynamic, multimodal set of facial and
vocal expressions in North American English. PLOS ONE, 13(5), e0196391.

Nugroho, K.S., Iriananda, S.W., Akbar, I., Isnan, M. and Pardamean, B., 2025.
Advancing end-to-end Indonesian speech emotion recognition using
XLSR-Wav2Vec2. In: 2025 International Conference on Information Technology
Research and Innovation (ICITRI). IEEE.
https://doi.org/10.1109/ICITRI67507.2025.11232655

Pan, S.J. and Yang, Q., 2010. A survey on transfer learning. IEEE Transactions on
Knowledge and Data Engineering, 22(10), pp.1345-1359.

Powers, D.M.W., 2011. Evaluation: from precision, recall and F-measure to ROC,
informedness, markedness and correlation. Journal of Machine Learning
Technologies, 2(1), pp.37-63.

Santoso, T.B., Budianto, A.N. and Dutono, T., 2026. Development of a speech
emotion recognition dataset for Indonesian. IEEE Access.
https://doi.org/10.1109/ACCESS.2026.3652377

Shapiro, S.S. and Wilk, M.B., 1965. An analysis of variance test for normality
(complete samples). Biometrika, 52(3-4), pp.591-611.

Tang, D., Kuppens, P., Geurts, L. and van Waterschoot, T., 2023. End-to-end
transfer learning for speaker-independent cross-language and cross-corpus
speech emotion recognition. arXiv:2311.13678 [eess.AS].

Wilcoxon, F., 1945. Individual comparisons by ranking methods. Biometrics
Bulletin, 1(6), pp.80-83.

Wu, P., Shi, J., Zhong, Y., Watanabe, S. and Black, A.W., 2021. Cross-lingual
transfer for speech processing using acoustic language similarity.
arXiv:2111.01326 [eess.AS].
```

## 9. Catatan Format Dokumen

- Bahasa Indonesia sepanjang dokumen.
- Kompilasi **XeLaTeX** (jalankan 2×), font **Carlito** (substitusi Calibri, praktik lazim — bukan aturan resmi), spasi tunggal, sitasi **UB Harvard**.
- Base template: `github.com/arawsardni/skripsi`. Package: `fontspec`, `tikz`, `booktabs`.

## 10. Sumber Dokumen (tidak tersedia di repo ini)

Konteks di atas digali dari Claude Project terpisah: praproposal yang disetujui, `PanduanSkripsiFilkom-v3_0.pdf`, dan 3 contoh skripsi FILKOM (Rifky 2026 implementatif; Ulfah 2019 & Wahyu 2021 nonimplementatif), plus pencarian web untuk verifikasi teknis (spek XLSR-53/GPU, cek literatur LoRA). File-file itu tidak ada di repo ini — untuk detail lebih lanjut, balik ke sesi Claude.ai terkait atau upload ulang.
