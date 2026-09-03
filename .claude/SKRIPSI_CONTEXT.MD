# Konteks Skripsi: SER Indonesia — E-SERAVD v1.1

> **⚠️ PIVOT BESAR TERJADI.** Desain riset di file versi sebelumnya (2×2 factorial: single/two-stage × full-FT/LoRA, dengan RAVDESS/CREMA-D sebagai korpus sumber) **SUDAH DITINGGALKAN SEPENUHNYA**. Jangan lanjutkan draf Bab 1-3 yang masih berbasis desain lama — semuanya perlu ditulis ulang mengikuti desain baru di Bagian 6. Baca Bagian 3 (Riwayat Pivot) dulu supaya paham kenapa, biar ide lama nggak diusulkan ulang tanpa sadar.

## 1. Identitas & Metadata

| Field                   | Isi                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Nama                    | Gaung Taqwa Indraswara                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| NIM                     | 235150207111043                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Jurusan/Prodi           | Teknik Informatika, FILKOM UB                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Bidang Skripsi          | Komputasi Cerdas (KC)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Jenis / Tipe Penelitian | Non-implementatif — Analitik (dipertahankan meski sempat dipertimbangkan pindah ke implementatif, lihat Bagian 3)                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Calon Pembimbing        | Tirana Noor Fatyanosa (Bu Tirana)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Judul                   | **⚠️ BELUM DIKUNCI, revisi ke-2.** Judul "Strategi Fine-Tuning Dua Tahap" (lama) dan "Representasi Self-Supervised vs Fitur Tangan" (revisi ke-1) sudah tidak cukup mendeskripsikan fokus terbaru. Draf usulan terkini: _"Analisis Pengaruh Skema Pembagian Data terhadap Performa Representasi Self-Supervised dan Fitur Akustik Konvensional pada Pengenalan Emosi Ucapan Berbahasa Indonesia"_ — mencerminkan bahwa topik utama sekarang soal ketahanan (robustness) representasi terhadap skema evaluasi, bukan cuma perbandingan representasi semata. |

## 2. Status Progres & Timeline

- **Praproposal**: disetujui Bu Tirana — tapi berbasis desain lama (dua tahap). Perubahan desain sebesar ini kemungkinan perlu dikomunikasikan ulang ke Bu Tirana sebagai revisi topik, bukan cuma revisi bahasa.
- **Draf Bab 1-3 sebelumnya**: sebagian besar strukturnya (format Bab 1 sesuai template, teori k-fold/statistik di Bab 2, kerangka nonimplementatif-analitik) masih reusable. Tapi seluruh ISI yang spesifik ke desain lama (Latar Belakang soal cross-lingual transfer, RM/Tujuan/Hipotesis/Batasan Masalah, Tabel dataset & kondisi eksperimen) perlu ditulis ulang.

### 2.1 Timeline Resmi (dari sistem akademik, per 31 Agustus 2026)

| Tahapan                   | Deadline    |
| ------------------------- | ----------- |
| Penilaian Kemajuan 1 (P1) | 27 Okt 2026 |
| Penilaian Kemajuan 2 (P2) | 11 Des 2026 |
| Pendaftaran Semhas/Ujian  | 22 Des 2026 |

## 3. Riwayat Pivot (Ringkas) — Supaya Ide Lama Nggak Diusulkan Ulang

| Yang dipertimbangkan                                                                                                                                       | Status                                               | Kenapa                                                                                                                                                                                |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Desain 2×2 (single/two-stage × full-FT/LoRA), korpus sumber RAVDESS/CREMA-D                                                                                | ❌ Ditinggalkan                                      | Fondasinya (klaim Santoso et al. "pakai two-stage") ternyata salah — paper aslinya CNN+MFCC murni, tidak ada cross-lingual transfer sama sekali.                                      |
| Pindah ke skripsi implementatif (bangun model CNN)                                                                                                         | ❌ Ditolak                                           | Implementatif vs nonimplementatif itu soal jenis kontribusi (produk vs analisis), bukan soal seberapa tervalidasi premisnya. Manualisasi (alasan awal) sudah ada solusi di jalur SSL. |
| PEFT (full-FT vs LoRA) sebagai axis utama, dimotivasi keterbatasan server bersama                                                                          | ❌ Ditolak                                           | "Server saya terbatas" itu constraint eksekusi personal, bukan urgensi akademik buat orang lain.                                                                                      |
| Continued/adaptive pretraining pada data tak berlabel sebelum fine-tuning                                                                                  | 🔶 Dipertimbangkan, tidak dipakai                    | Well-evidenced (Nowakowski, SAPT, emotion2vec), tapi kalah kuat dari Ide final di bawah setelah ditemukan paper Atmaja & Sasou.                                                       |
| Augmentasi data untuk sampel kecil                                                                                                                         | 🔶 Disimpan sebagai future work                      | Valid, tapi nambah kompleksitas metodologis (pilihan teknik, risiko distorsi label emosi) yang belum tentu sepadan untuk waktu yang tersisa.                                          |
| SSL (XLSR-53) vs fitur akustik tangan sebagai RM tunggal, extend Atmaja & Sasou (2022)                                                                     | 🔶 Superseded                                        | Ide ini tidak hilang, tapi jadi salah satu DIMENSI dari desain 2×2 baru di bawah — bukan lagi berdiri sendiri sebagai RM tunggal.                                                     |
| Split independen-film sebagai "metodologi wajib" pelengkap                                                                                                 | 🔶 Superseded                                        | Awalnya cuma jadi cara-mengerjakan yang benar buat RM di atas; sekarang jadi DIMENSI kedua yang diuji eksplisit, bukan cuma diterapkan diam-diam.                                     |
| **2×2: Representasi (MFCC+CNN vs XLSR-53) × Skema Split (acak vs independen-film), menguji ketahanan (robustness) baseline Santoso terhadap film leakage** | ✅ **DIKUNCI — desain saat ini, jadi bahasan utama** | Lihat Bagian 6. Bukan gabungan dua pertanyaan independen (beda dari 2×2 lama) — ini satu pertanyaan ("apakah SSL lebih tahan leakage?") yang butuh dua dimensi buat dijawab.          |

**Pelajaran penting dari proses ini**: setiap klaim dari paper/dataset pihak lain yang jadi dasar argumen gap HARUS diverifikasi ke sumber primer (bukan cuma abstrak/sitasi orang lain) sebelum dibangun di atasnya — dua kali ditemukan klaim yang ternyata salah (Santoso "two-stage", lalu kompleksitas versi dataset) setelah sumber primernya benar-benar dibaca.

## 4. Dataset: E-SERAVD — Silsilah Lengkap & Spesifikasi Terverifikasi

Dataset ini py ada **4 versi berbeda**, jangan disamakan:

| Versi | Nama                                         | Tanggal                          | Kelas               | Film             | N                            | Status                                             |
| ----- | -------------------------------------------- | -------------------------------- | ------------------- | ---------------- | ---------------------------- | -------------------------------------------------- |
| v1.0  | PENS-SER Dataset                             | Okt 2025                         | 4                   | 6                | 1.200                        | Terverifikasi                                      |
| v1.1  | E-SERAVD                                     | Des 2025                         | 4                   | 6                | 1.200                        | **✅ DIPAKAI — identik byte-for-byte dengan v1.0** |
| —     | (Paper IEEE Access, Santoso/Budianto/Dutono) | submit Des 2025, terbit Jan 2026 | 4                   | 6 (match persis) | subset 1.000 dari pool 1.200 | Baseline pembanding                                |
| v1.2  | E-SERAVD                                     | Jun 2026                         | 6 (+Disgust, +Fear) | 11               | 3.000 (klaim)                | Tidak dipakai — lihat catatan di bawah             |

_(Catatan terpisah: ada juga paper JOIV oleh Santoso, **Aini**, Dutono — pakai 1 serial TV "Imperfect", bukan bagian dari silsilah v1.0-v1.2 ini, jangan dicampur.)_

### 4.1 Spesifikasi v1.1 (terverifikasi langsung dari `Dataset_Specification` — DIPAKAI)

- **N = 1.200 klip**, seimbang sempurna **300 per kelas** — Marah (Anger), Sedih (Sadness), Netral (Neutral), Bahagia (Happiness). Tersedia versi Audio-Only (.wav) dan Audio-Video (.mp4), masing-masing 1.200 file, 1:1 berpasangan.
- **6 film sumber**: AADC (Ada Apa Dengan Cinta), D1990 (Dilan 1990), DHB (Dua Hati Biru), HA3 (Habibie & Ainun 3), HSL (Home Sweet Loan), TAOL (The Architecture of Love) — **cocok persis** dengan daftar film di paper IEEE Access, mengonfirmasi v1.1 kemungkinan besar = dataset yang dipakai paper tersebut.
- Durasi klip: 0,83–6,87 detik, rata-rata ~2,6 detik. Audio: 44.100 Hz, 32-bit float, 2 channel. Video: ~23,98 fps, 1280×720.
- Fleiss Kappa (dari README): Po=0,779, Pe=0,396, **K=0,6338** → "Substantial agreement" — tidak ditemukan kontradiksi matematis internal di angka ini (beda dari v1.2, lihat 4.3).
- **Tidak ada file fitur akustik siap pakai** (beda dari v1.2) — kalau butuh fitur tangan (MFCC atau lainnya), perlu diekstrak sendiri.
- 1.200 nama file semuanya unik, tidak ada duplikat.

### 4.2 ⚠️ Temuan Penting: Confound Film–Emosi di v1.1

Sebaran film per kelas **tidak merata**, dan pola ketidakmerataannya beda-beda per kelas:

| Kelas       | AADC   | D1990  | DHB     | HA3    | HSL | TAOL |
| ----------- | ------ | ------ | ------- | ------ | --- | ---- |
| Netral      | 50     | 50     | 50      | 50     | 50  | 50   |
| Marah       | 54     | 35     | 54      | 51     | 53  | 53   |
| **Sedih**   | 58     | **7**  | **106** | 32     | 55  | 42   |
| **Bahagia** | **12** | **74** | 41      | **73** | 64  | 36   |

Netral tersebar rata sempurna, tapi **Sedih didominasi DHB (106/300, 35%)** dan **Bahagia didominasi D1990+HA3 (147/300, 49%)**. Risiko: kalau split train/test dilakukan acak (klip dari film yang sama muncul di train DAN test), model berisiko belajar "gaya produksi/suara khas film X" sebagai proxy buat emosi tertentu, bukan pola akustik emosi yang genuinely general. **Ini alasan kenapa split independen-film jadi bagian wajib metodologi (Bagian 6), bukan cuma opsi.**

### 4.3 Catatan v1.2 (tidak dipakai, tapi berguna sebagai referensi)

v1.2 (11 film, 6 kelas, klaim 3.000 seimbang 500/kelas) sempat dipertimbangkan karena match ke celah eksplisit di Atmaja & Sasou soal 6-kelas. **Ditemukan anomali matematis**: tabel Fleiss Kappa di README-nya menunjukkan kategori Disgust cuma dapat 42 rating dari total 15.033 — secara matematis mustahil menghasilkan 500 klip tervalidasi (maksimal ~21 klip di ambang 2/3 rater setuju). **Dicek langsung ke `Dataset_Specification.xlsx` v1.2**: tidak ditemukan bukti duplikasi/padding di level file (500 baris Disgust semuanya unik, tersebar wajar di 11 film, fitur akustiknya cocok persis dengan tabel ringkasan README) — jadi kemungkinan besar ini bukan data palsu, tapi tabel Fleiss Kappa yang menggambarkan tahap anotasi berbeda/lebih awal dari yang menghasilkan set final. Tetap belum terjawab tuntas; bukan masalah kita lagi karena tidak dipakai, tapi dicatat untuk konteks kalau v1.2 dipertimbangkan lagi nanti.

## 5. Baseline Pembanding: Santoso, Budianto & Dutono (2026), IEEE Access

- Backbone: CNN + fitur MFCC (bukan self-supervised sama sekali).
- Dilatih pada subset **1.000 klip seimbang (250/kelas)**, diambil dari pool 1.200 v1.1 — sekarang jelas asal-usulnya, tidak lagi janggal.
- Angka yang dilaporkan **tidak konsisten antar bagian paper** — pakai yang paling bisa ditelusuri: **86% akurasi, 86% F1-macro** (dari confusion matrix di badan teks), bukan 90%/86% (abstrak) atau 94,72% (Tabel 5, tidak jelas asalnya).
- DOI: 10.1109/ACCESS.2026.3652377.

## 6. Desain Riset Saat Ini (DIKUNCI — Bahasan Utama)

### 6.1 Ide Inti: Ketahanan (Robustness) Representasi terhadap Skema Pembagian Data

Ide ini muncul dari temuan sendiri (Bagian 4.2), bukan dipinjam dari paper manapun — nilai lebihnya dibanding ide-ide sebelumnya di riwayat pivot. Santoso, Budianto dan Dutono (2026) melaporkan baseline CNN+MFCC (86%/86%) tanpa menyebut kontrol terhadap sumber film saat split train/test. Karena E-SERAVD v1.1 punya confound film-emosi nyata (Sedih didominasi DHB 106/300; Bahagia didominasi D1990+HA3 147/300), ada risiko performa yang mereka laporkan terinflasi oleh **film leakage** (model belajar ciri produksi/rekaman khas satu film, bukan pola akustik emosi genuine) — bukan cuma dugaan teoretis, tapi risiko konkret yang bisa diuji langsung di data ini.

**Pertanyaan inti**: apakah representasi self-supervised (XLSR-53) LEBIH TAHAN terhadap leakage semacam ini dibanding fitur akustik tangan (MFCC), karena representasi self-supervised dipelajari dari tujuan pra-latih yang lebih general (bukan spesifik ke ciri produksi/rekaman tertentu)?

**Penting**: ini BUKAN dua pertanyaan independen yang digabung (beda dari desain 2×2 lama yang menggabung staging+efisiensi tanpa keterkaitan logis) — ini SATU pertanyaan yang butuh dua dimensi buat dijawab. Nggak bisa tahu "apakah representasi X lebih tahan leakage" tanpa memvariasikan representasi DAN skema split sekaligus.

### 6.2 Desain Eksperimen: 2×2 (Representasi × Skema Split)

| Kondisi | Representasi | Skema Split     |
| ------- | ------------ | --------------- |
| A       | MFCC + CNN   | Acak            |
| B       | MFCC + CNN   | Independen-film |
| C       | XLSR-53      | Acak            |
| D       | XLSR-53      | Independen-film |

- **Kondisi A** = replikasi konfigurasi Santoso, Budianto dan Dutono (2026) — target hasil mendekati 86%/86% kalau replikasi berhasil (jadi validasi juga bahwa metodologi replikasi Gaung benar).
- **B vs A**: mengukur langsung seberapa besar performa fitur tangan turun akibat leakage.
- **D vs C**: mengukur hal yang sama untuk XLSR-53.
- **Interaksi (B-vs-A dibanding D-vs-C)**: menjawab pertanyaan inti — apakah penurunan akibat split lebih ketat itu berbeda besarnya antara dua representasi.
- **Beban komputasi**: A dan B ringan (MFCC+CNN, tanpa fine-tuning apapun — sama seperti setup Santoso). C dan D itu fine-tuning XLSR-53 yang memang sudah direncanakan sebelumnya, sekarang dijalankan di 2 skema split — bukan menambah beban baru, cuma direorganisasi di sekitar pertanyaan yang lebih tajam.

### 6.3 Rumusan Masalah (draf, belum ditulis ke dokumen resmi)

1. Bagaimana pengaruh skema pembagian data (acak vs independen-film) terhadap performa klasifikasi emosi pada E-SERAVD v1.1?
2. Apakah pengaruh skema pembagian data tersebut berbeda antara representasi fitur akustik tangan dan representasi self-supervised (XLSR-53)?
3. _(opsional, masih didiskusikan — lihat Bagian 8)_ Bagaimana konsistensi hasil antar-fold untuk masing-masing kondisi?

### 6.4 Gap Statement (draf)

Santoso, Budianto dan Dutono (2026) melaporkan baseline CNN+MFCC pada E-SERAVD tanpa mengontrol sumber film saat pembagian data train/test, berisiko performa yang dilaporkan terinflasi oleh kebocoran informasi terkait film (film leakage) alih-alih murni mengukur generalisasi pola akustik emosi. Atmaja and Sasou (2022) menunjukkan keunggulan representasi self-supervised atas fitur akustik konvensional pada lima dataset SER, namun tidak menguji ketahanan keunggulan tersebut terhadap skema evaluasi yang lebih ketat. Penelitian ini menempatkan diri pada celah tersebut: menguji ulang secara sistematis konfigurasi baseline Santoso et al. di bawah skema pembagian data yang lebih ketat (independen-film), dan membandingkan ketahanan representasi self-supervised versus fitur tangan terhadap perubahan skema tersebut.

### 6.5 Paper Jangkar & Peran Barunya

**Atmaja, B.T. and Sasou, A., 2022**, "Evaluating Self-Supervised Speech Representations for Speech Emotion Recognition," **IEEE Access**, vol. 10, pp. 124396–124407, DOI 10.1109/ACCESS.2022.3225198.

- Landasan teori "SSL vs fitur tangan" tetap dari sini (SSL unggul besar di 5 dataset Inggris/Jepang, effect size 3,60/3,34, Friedman p<10⁻¹⁰) — tapi **framing gap-nya bergeser**: bukan lagi "belum diuji di Indonesia" semata, jadi "belum diuji ketahanannya terhadap bias evaluasi tersembunyi (leakage)".
- XLSR-53 sempat kalah dari model monolingual Inggris di studi mereka (effect size 2,13) — tetap relevan ditulis jujur sebagai nuansa, terlepas dari desain baru ini.
- Template metodologi (WA+UA, Friedman/effect size) masih bisa jadi referensi opsional, tidak wajib diikuti persis.

**Santoso, Budianto dan Dutono (2026)** — perannya berubah dari "angka pembanding pasif" jadi **objek yang direplikasi dan diuji ulang secara aktif** (Kondisi A dalam desain 2×2). Ini juga bikin Manfaat Penelitian jadi lebih konkret: kalau ternyata ada leakage, itu jadi rekomendasi metodologis nyata buat siapapun yang pakai E-SERAVD ke depannya — bukan cuma "memberi bukti empiris" generik.

### 6.6 Yang TIDAK dipakai lagi (eksplisit, supaya jelas)

- ❌ Tidak ada axis single-stage vs two-stage.
- ❌ Tidak ada axis full-FT vs LoRA / PEFT.
- ❌ Tidak ada korpus sumber (RAVDESS/CREMA-D) atau tahap fine-tuning sumber.
- ❌ RM tunggal "SSL vs fitur tangan" (revisi sebelumnya) — sudah diperluas jadi 2×2 dengan dimensi skema split, bukan RM tunggal lagi.
- ❌ Split independen-film sebagai "cara mengerjakan yang benar" secara diam-diam — sekarang diuji eksplisit sebagai salah satu dimensi, bukan diterapkan tanpa dilaporkan pembandingnya.

## 7. Checklist — Yang Perlu Ditulis Ulang

- [ ] Judul (draf ada di Bagian 1, belum dikunci — revisi ke-2)
- [ ] Latar Belakang — narasi baru: motivasi dari isu validitas evaluasi (film leakage) + SSL vs fitur tangan (Atmaja & Sasou) sebagai dua landasan yang bertemu di satu pertanyaan
- [ ] Rumusan Masalah, Tujuan, Hipotesis, Manfaat, Batasan Masalah — semua perlu ditulis ulang dari nol mengikuti RM 2×2 di 6.3 (2 RM utama + 1 opsional)
- [ ] Landasan Kepustakaan — dua rujukan utama sekarang: Atmaja & Sasou (2022, landasan SSL vs fitur tangan) DAN Santoso et al. (2026, objek yang direplikasi/diuji ulang — bukan cuma baseline pasif)
- [ ] Bab 3 Metodologi — desain 2×2 baru: MFCC+CNN vs XLSR-53, × split acak vs independen-film → 4 kondisi (bukan 1 perbandingan tunggal lagi). Perlu diputuskan detail fitur tangan (lihat Bagian 8 poin 1)
- [ ] Tabel karakteristik dataset (N=1.200, 300/kelas, 6 film, durasi) — data sudah lengkap tersedia di Bagian 4.1
- [ ] Tabel/penjelasan confound film-emosi (Bagian 4.2) — ini sekarang bagian SENTRAL dari argumen, bukan cuma catatan metodologi, perlu porsi lebih besar di Bab 1/2
- [ ] Hipotesis — perlu dirumuskan untuk RM1 (pengaruh skema split) dan RM2 (interaksi dengan representasi) secara terpisah
- [ ] Komunikasi ke Bu Tirana soal pivot desain — ini bukan revisi kecil, kemungkinan perlu didiskusikan sebagai perubahan topik dalam payung yang sama

## 8. Keputusan Terbuka

1. **Fitur tangan yang mana buat Kondisi A/B?** MFCC persis seperti Santoso (paling penting untuk kredibilitas replikasi Kondisi A — kalau beda fitur, "replikasi"-nya jadi kurang valid) vs 7-fitur Praat/Librosa ala v1.2. **Condong ke MFCC** sekarang, karena Kondisi A butuh semirip mungkin ke setup asli Santoso supaya perbandingan A-vs-B (dan justifikasi "ini benar-benar replikasi") kredibel — beda dari desain sebelumnya di mana fitur tangan cuma pembanding umum, bukan replikasi spesifik.
2. **RM3 soal konsistensi antar-fold** — dipertahankan seperti pola desain-desain sebelumnya (mengikuti keempat kondisi A-D), atau cukup 2 RM saja? Belum diputuskan.
3. **Definisi "independen-film" buat k-fold** — dengan cuma 6 film sumber, k-fold yang independen-film otomatis terbatas maksimal k=6 (atau k lebih kecil kalau beberapa film digabung per fold). Ini perlu dipikirkan konkret di Bab 3 — beda dari k-fold acak yang k-nya bebas ditentukan (5, 10, dst).
4. **Manualisasi** — masih belum dikonfirmasi ke Bu Tirana. Desain 2×2 baru ini (4 kondisi, tapi 2 di antaranya ringan/tanpa fine-tuning) mengembalikan sedikit kompleksitas dibanding desain SSL-tunggal — opsi manualisasi uji statistik tetap paling relevan dan feasible dibanding manualisasi model.
5. **Augmentasi data (Ide C lama)** — tetap disimpan sebagai arah pengembangan di Bab 6 Penutup, bukan bagian inti.

## 9. Daftar Pustaka (Diperbarui)

Referensi lama yang masih relevan sebagai latar belakang umum (SER, self-supervised learning, Wav2Vec2/XLSR) dipertahankan. Yang berubah signifikan peran/relevansinya:

- **Atmaja, B.T. and Sasou, A., 2022.** Evaluating self-supervised speech representations for speech emotion recognition. **IEEE Access**, 10, pp.124396-124407. https://doi.org/10.1109/ACCESS.2022.3225198 — **RUJUKAN UTAMA/JANGKAR, baru.**
- Santoso, T.B., Budianto, A.N. and Dutono, T., 2026. Development of a speech emotion recognition dataset for Indonesian. IEEE Access. https://doi.org/10.1109/ACCESS.2026.3652377 — **tetap dipakai, sekarang sebagai baseline pembanding langsung (bukan lagi salah satu dari 4 kelompok gap).**
- Referensi soal cross-lingual transfer (Wu et al. 2021, Arisaputra & Zahra 2023, Tang et al. 2023) — **peran berkurang drastis**, tidak lagi jadi tulang punggung gap argument. Tang et al. (2023) masih relevan sebagai preseden metodologi speaker-independent split (Bagian 6.4). Wu et al. dan Arisaputra & Zahra kemungkinan tidak lagi disitasi di Bab 2 versi baru, atau cuma disebut sekilas di Bab 6 sebagai arah lanjutan.
- Hu et al. 2022 (LoRA) — **kemungkinan tidak lagi relevan** untuk disitasi di Bab 2 inti, karena axis LoRA sudah tidak dipakai.
- Nugroho et al. (2025) — masih relevan sebagai bagian gambaran umum riset SER Indonesia, meski tidak lagi jadi salah satu dari "4 kelompok gap" versi lama.

_(Daftar Pustaka lengkap versi lama masih ada di riwayat file context sebelumnya kalau perlu ditelusuri kembali — tidak diulang semua di sini karena sebagian besar relevansinya berubah.)_

## 10. Catatan Format Dokumen

- Bahasa Indonesia sepanjang dokumen.
- Kompilasi **XeLaTeX** (jalankan 2×), font **Carlito** (substitusi Calibri, praktik lazim), spasi tunggal, sitasi **UB Harvard**.
- Base template: `github.com/arawsardni/skripsi`.
- Bab 1 mengikuti struktur `Template-ProposalSkripsi-v3_0.docx` resmi (1.1–1.6, tanpa subbab Hipotesis/Jadwal — keduanya di Bab 3, lihat catatan di riwayat file sebelumnya kalau detail ini perlu ditelusuri ulang).

## 11. Sumber Dokumen

Konteks di atas digali dari: Claude Project (praproposal lama, Panduan Skripsi FILKOM v3.0, Template docx, 3 contoh skripsi FILKOM), full paper PDF Santoso, Budianto & Dutono (2026) yang diunggah dan dibaca utuh, full paper Santoso, Aini & Dutono (JOIV 2025), full text Atmaja & Sasou (IEEE Access, diakses via academia.edu mirror), README.md untuk E-SERAVD v1.0/v1.1/v1.2, dan file `Dataset_Specification` (.xlsx) untuk v1.0/v1.1/v1.2 yang diunggah dan diverifikasi langsung (dibaca dengan pandas/openpyxl — bukan cuma dipercaya dari deskripsi). File-file ini tidak ada di repo ini — untuk detail lebih lanjut atau verifikasi ulang, balik ke sesi Claude.ai terkait atau upload ulang.
