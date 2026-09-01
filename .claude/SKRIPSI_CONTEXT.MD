# Konteks Skripsi: SER Indonesia — Strategi Fine-Tuning Dua Tahap

> **Cara pakai:** Rangkuman diskusi perencanaan riset (Claude.ai, bukan sesi ini), supaya bisa dipahami tanpa histori chat asli. **Baca Bagian 2.2 dulu** — ada koreksi kritis ke sitasi utama yang perlu ditulis ulang sebelum revisi lain. **Bagian 6** berisi checklist lengkap semua yang perlu ditulis/diubah di draf (gabungan temuan rubrik + revisi Bab 1/2 terbaru). **Bagian 7** berisi keputusan yang masih terbuka.

## 1. Identitas & Metadata

| Field                               | Isi                                                                                                                                                                                                                                    |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Nama                                | Gaung Taqwa Indraswara                                                                                                                                                                                                                 |
| NIM                                 | 235150207111043                                                                                                                                                                                                                        |
| Jurusan/Prodi                       | Teknik Informatika, FILKOM UB                                                                                                                                                                                                          |
| Bidang Skripsi                      | Komputasi Cerdas (KC)                                                                                                                                                                                                                  |
| Jenis / Tipe Penelitian             | Non-implementatif — Analitik (Analytical/Explanatory) — **dikonfirmasi ulang tetap ini, lihat Bagian 2.2**                                                                                                                             |
| Calon Pembimbing                    | Tirana Noor Fatyanosa (Bu Tirana)                                                                                                                                                                                                      |
| Judul (draf saat ini)               | _Analisis Pengaruh Strategi Fine-Tuning Dua Tahap terhadap Akurasi Pengenalan Emosi Ucapan Berbahasa Indonesia_                                                                                                                        |
| Judul (diusulkan, ⚠️ belum dikunci) | _Analisis Pengaruh Strategi Fine-Tuning Dua Tahap dan Metode Adaptasi Parameter terhadap Akurasi Pengenalan Emosi Ucapan Berbahasa Indonesia_ — supaya judul mencakup axis full-FT/LoRA juga, bukan cuma axis staging. Lihat Bagian 4. |

## 2. Status Progres

- **Praproposal**: disetujui Bu Tirana. Masih valid meski ada koreksi di Bagian 2.2 — substansi/gap-nya tidak berubah, cuma satu sitasi pendukung yang perlu diperbaiki.
- **Proposal Bab 1-3**: draf sudah cukup matang secara STRUKTUR (semua subbab wajib ada, teori Bab 2 lengkap, Tabel 4-kondisi di Bab 3 ada). Tapi ada beberapa revisi ISI yang belum ditulis ke draf — lihat checklist lengkap di Bagian 6.
- **Isu paling mendesak sekarang**: bukan lagi kelengkapan struktur atau hardware — itu semua sudah settled. Yang mendesak sekarang adalah **koreksi sitasi Santoso** (Bagian 2.2) dan sejumlah revisi Bab 1 (judul, rumusan masalah, lokasi Hipotesis/Jadwal) yang sudah disepakati di diskusi tapi belum ditulis.

### 2.1 Timeline Resmi (dari sistem akademik, per 31 Agustus 2026)

| Tahapan                   | Deadline    | Sisa waktu   |
| ------------------------- | ----------- | ------------ |
| Penilaian Kemajuan 1 (P1) | 27 Okt 2026 | ~8 minggu    |
| Penilaian Kemajuan 2 (P2) | 11 Des 2026 | ~14,5 minggu |
| Pendaftaran Semhas/Ujian  | 22 Des 2026 | ~16 minggu   |

**Implikasi**: eksperimen inti idealnya selesai substansial sekitar P2. ~14 minggu adalah budget riil.

### 2.2 ⚠️ KOREKSI KRITIS — Sitasi Santoso Salah (ditemukan & diverifikasi dari primary source)

**Temuan**: Baik praproposal yang disetujui maupun draf Bab 1-3 saat ini menyebut Santoso, Budianto & Dutono (2026) memakai **"skema fine-tuning dua tahap"** untuk mencapai 90% akurasi/86% F1. Setelah full paper PDF-nya diperoleh dan dibaca langsung (bukan cuma abstrak/search snippet): **paper itu SAMA SEKALI tidak menyebut two-stage, cross-lingual transfer, Wav2Vec2, XLSR, atau self-supervised apapun.** Metodenya murni **CNN dilatih dari nol** dengan fitur **MFCC** (pre-emphasis → framing → Mel filterbank 40 → DCT → 20 koefisien), pada subset **seimbang 1.000 sampel (250/kelas)** — bukan full ~5.049 sampel. Angka 90% akurasi/86% F1 itu benar, tapi sumbernya CNN baseline biasa, bukan two-stage apapun. Kesalahan ini berasal dari sesi Claude.ai sebelumnya yang menyusun praproposal awal — tidak pernah diverifikasi silang sampai sekarang.

**Perbaikan konkret yang perlu ditulis (belum ditulis ke draf):**

- **Tabel 2.1, baris Santoso** — _Metode_: "Pengembangan dataset E-SERAVD; performa dasar diuji menggunakan model CNN dengan fitur MFCC (bukan model pra-latih self-supervised)." _Hasil_: "Model CNN dasar mencapai akurasi 90% dan F1-score 86% pada subset seimbang (1.000 sampel, 250/kelas)."
- **Paragraf gap di Landasan Kepustakaan** — ganti kalimat "Santoso, Budianto and Dutono (2026) menerapkan skema fine-tuning dua tahap, tetapi hanya melaporkan satu konfigurasi terbaik tanpa perbandingan sistematis maupun uji statistik" dengan versi baru: _"Santoso, Budianto and Dutono (2026) menyediakan dataset E-SERAVD dan menetapkan baseline performa menggunakan CNN konvensional tanpa representasi self-supervised lintas bahasa; performa pendekatan seperti XLSR-53 — apalagi dengan skema dua tahap — belum diuji pada dataset ini sama sekali."_
- **Latar Belakang, paragraf soal potensi cross-lingual transfer** — kalimat "...sementara Santoso, Budianto and Dutono (2026) menunjukkan bahwa skema fine-tuning dua tahap dapat menghasilkan akurasi 90%..." perlu dikoreksi jadi merujuk baseline CNN, bukan two-stage. Lihat draf paragraf pengganti lengkap di Bagian 4.
- Catatan tambahan (opsional, untuk kejujuran metodologis): baseline 90%/86% dihitung dari subset seimbang 1.000 sampel, bukan full 5.049 — relevan kalau nanti mau bandingkan secara adil dengan hasil eksperimen sendiri.

**Apakah ini berarti harus ganti topik/model (mis. pindah ke CNN implementatif)?** Dipertimbangkan serius, tapi **diputuskan TIDAK — tetap di jalur XLSR-53 + desain 4-kondisi + nonimplementatif-analitik.** Alasan:

- Implementatif vs nonimplementatif ditentukan dari JENIS KONTRIBUSI (produk/sistem jadi vs analisis statistik), bukan dari seberapa tervalidasi premisnya oleh penelitian lain. Model tetap instrumen, bukan produk akhir, terlepas dari koreksi ini.
- Rumusan masalah sudah/akan ditulis sebagai "bagaimana **pengaruh** X" (lihat Bagian 3.1), bukan "buktikan X lebih baik" — jadi robust terhadap hasil apapun (signifikan, tidak signifikan, atau malah negatif). Semua itu tetap jawaban valid untuk penelitian analitik.
- Manualisasi (alasan awal mempertimbangkan pindah ke CNN) sudah ada solusi di jalur XLSR-53 tanpa ganti backbone — lihat Bagian 7 poin 5.
- Praproposal yang disetujui, ~90% draf Bab 1-3, dan literatur pembanding (Nugroho, Arisaputra & Zahra, Tang et al.) semua tetap valid dan reusable di jalur ini. Pindah topik berarti mulai dari nol dengan P1 tinggal ~8 minggu — risiko jauh lebih besar daripada masalah yang mau diselesaikan.
- **Pilot run** (Bagian 7 poin 3) adalah titik keputusan sebenarnya soal kelayakan riset ini — bukan koreksi sitasi ini. Kalau pilot run nanti menunjukkan masalah fundamental, itu baru alasan kuat untuk mikir ulang arah besar.

**Konsekuensi lanjutan — gap argument perlu direkonstruksi dari "melengkapi" jadi "menguji hipotesis dari bukti tidak langsung"**: lihat rangkaian bukti lengkap di Bagian 5.

## 3. Inti Riset

### 3.1 Rumusan Masalah & Tujuan — ⚠️ dua putaran revisi, versi final di bawah belum ditulis ke draf

**Putaran 1** (sudah ditulis ke draf terakhir): RM1 tadinya menyebut metrik eksplisit ("akurasi dan F1-score") — disederhanakan jadi "performa", metrik dipindah ke Tujuan, mengikuti pola skripsi kating.

**Putaran 2** (⚠️ BELUM ditulis): dengan judul diusulkan berubah jadi eksplisit "Pengaruh..." (Bagian 1), RM dan Tujuan perlu konsisten pakai kata **"pengaruh"**, bukan "perbedaan" — ini juga cocok pola skripsi kating (contoh: "Bagaimana **pengaruh** resolusi citra wajah terhadap akurasi..."). RM3 TIDAK berubah karena itu bukan klaim pengaruh (pertanyaan konsistensi, jenis berbeda). Hipotesis (Bagian 3.2) tetap boleh pakai kata "perbedaan" karena itu level formal-statistik, beda dari RM yang level konsep.

**Rumusan Masalah (final, versi "pengaruh")**:

1. Bagaimana pengaruh strategi fine-tuning (single-stage dibandingkan fine-tuning dua tahap, yaitu pra-latih pada korpus emosi berbahasa Inggris lalu diadaptasi ke data Indonesia) terhadap performa klasifikasi emosi ucapan berbahasa Indonesia?
2. Bagaimana pengaruh metode adaptasi (full fine-tuning dibandingkan LoRA) terhadap performa klasifikasi emosi pada tahap adaptasi ke data target, baik pada skema single-stage maupun two-stage?
3. Bagaimana konsistensi hasil klasifikasi emosi antar-fold pada skema validasi silang untuk masing-masing strategi fine-tuning yang diuji? _(tetap, tidak berubah)_

**Tujuan (final, selaras RM + akomodasi 4 metrik dari Bagian 6)**:

1. Menganalisis pengaruh strategi fine-tuning (single-stage dibandingkan dua tahap) terhadap performa klasifikasi emosi, diukur melalui akurasi, presisi, recall, dan F1-score.
2. Menganalisis pengaruh metode adaptasi (full fine-tuning dibandingkan LoRA) terhadap performa klasifikasi emosi pada tahap adaptasi ke data target, serta membandingkan efisiensi komputasinya dari sisi waktu pelatihan dan penggunaan memori.
3. Mengevaluasi konsistensi hasil klasifikasi emosi antar-fold pada skema validasi silang untuk setiap strategi fine-tuning yang diuji. _(tetap)_

### 3.2 Hipotesis — ⚠️ isi tetap sama, tapi LOKASI perlu pindah dari Bab 1 ke Bab 3

Dicek langsung ke `Template-ProposalSkripsi-v3_0.docx` resmi: daftar isi Bab 1 template cuma **1.1 Latar Belakang, 1.2 Rumusan Masalah, 1.3 Tujuan, 1.4 Manfaat, 1.5 Batasan Masalah, 1.6 Sistematika Pembahasan** — TIDAK ADA subbab Hipotesis maupun Jadwal Penelitian di Bab 1 versi template. Draf sekarang menambahkan keduanya di Bab 1 (1.3 dan 1.8) mengikuti rubrik Panduan, bukan template.

**Rekomendasi**: jangan hapus isinya (Panduan tetap mewajibkan keduanya), tapi **pindahkan lokasinya**:

- **Hipotesis** → pindah ke Bab 3, ditempatkan tepat sebelum penjelasan prosedur Shapiro-Wilk/paired t-test/Wilcoxon di Subbab Evaluasi K-Fold (hipotesis muncul pas sebelum prosedur yang mengujinya — lebih natural).
- **Jadwal Penelitian** → pindah jadi subbab baru di akhir Bab 3 (setelah Peralatan Pendukung). Ini juga didukung independen oleh Panduan sendiri: syarat Jadwal Penelitian masuk kategori **"Kelayakan Metodologi"**, bukan "Kelayakan Pendahuluan" — jadi secara rubrik pun tempatnya memang Bab 3.
- Bab 1 dipotong pas jadi 1.1–1.6 sesuai template; Subbab 1.7 Sistematika Pembahasan juga perlu disesuaikan teksnya (sebut Hipotesis & Jadwal ada di Bab 3, bukan Bab 1).

Isi hipotesis (tidak berubah, cuma pindah lokasi):

- RM1 — H0: tidak ada pengaruh signifikan strategi fine-tuning (single-stage vs two-stage) terhadap performa. H1: ada.
- RM2 — H0: tidak ada pengaruh signifikan metode adaptasi (full-FT vs LoRA) terhadap performa (di kedua skema stage). H1: ada.
- RM3 bersifat deskriptif, tidak diuji sebagai hipotesis statistik formal.

### 3.3 Desain Eksperimen: 2×2 Factorial, 4 Kondisi

| Kondisi | Stage        | Metode Adaptasi  |
| ------- | ------------ | ---------------- |
| A       | Single-stage | Full Fine-Tuning |
| B       | Single-stage | LoRA             |
| C       | Two-stage    | Full Fine-Tuning |
| D       | Two-stage    | LoRA             |

Backbone sama persis (XLSR-53) di keempat kondisi. Sudah tertulis eksplisit di Tabel 3.2 draf — **status: SELESAI ditulis**. Cakupannya (tetap 4 kondisi atau disederhanakan) masih keputusan terbuka, lihat Bagian 7 poin 2.

### 3.4 Backbone & Data

- **Backbone**: `facebook/wav2vec2-large-xlsr-53`, ~317 juta parameter, 24 layer Transformer. Draf sengaja memakai checkpoint XLSR-53 **mentah** (bukan turunan yang sudah di-fine-tune untuk Indonesia), supaya titik awal identik di keempat kondisi.
- **Checkpoint Nugroho et al. (2025)**: ✅ RESOLVED — desain sekarang sengaja TIDAK memakai checkpoint turunan siapa pun, jadi ambiguitas checkpoint Nugroho tidak relevan lagi untuk desain sendiri, cukup dicatat sebagai keterbatasan pembanding.
- **Data sumber (Tahap 3)**: RAVDESS (1.440 sampel, 8 kelas emosi, EN) dan/atau CREMA-D (7.442 sampel, 6 kelas emosi, EN).
- **Data target (Tahap 4)** — dikunci ke **E-SERAVD saja** (Santoso et al., 2026): ~5.049 sampel, 4 kelas emosi (marah 1.202, senang 1.228, netral 2.075, sedih 899). IndoWaveSentiment (Nugroho et al., 2025, ~300 sampel, 5 kelas) TIDAK dipakai sebagai target karena skala dan skema kelasnya beda — disebut sebagai arah pengembangan di Bab 6 Penutup. Ini sudah SELESAI ditulis di draf Tabel 3.1 terbaru.

### 3.5 Hardware

- **Perangkat**: 2× NVIDIA RTX 3060 **6GB** per kartu (bukan 12GB per kartu).
- **VRAM per kartu (~317 juta parameter)** — sudah ditulis ke draf Tabel 3.3 dengan benar, termasuk mitigasi 8-bit Adam:

  | Komponen                       | Full-FT (naif)      | Full-FT + 8-bit Adam | LoRA        |
  | ------------------------------ | ------------------- | -------------------- | ----------- |
  | Bobot+master+gradien+optimizer | ~5 GB               | ~1,9 GB              | ~0,6–0,7 GB |
  | Sisa aktivasi (dari 6GB)       | ~1 GB ⚠️ riskan OOM | ~4 GB                | ~5,3 GB     |

  Mitigasi 8-bit Adam (`bitsandbytes`) untuk Kondisi A & C sudah masuk ke Subbab 3.10.1 (software) dan 3.10.2 (hardware) draf terbaru — **status: SELESAI ditulis**.

- **Strategi 2 GPU**: 2 run independen paralel (`CUDA_VISIBLE_DEVICES`), bukan distributed training.
- Pilot run (1 kondisi × 1 fold) sudah masuk ke Jadwal Penelitian (Tahap 3) dan disebut di Subbab 3.5 draf — **status: SELESAI ditulis**.

## 4. Latar Belakang (Bab 1) — ⚠️ beberapa revisi belum ditulis

Alur logika (piramida, luas → sempit):

1. Emosi = inti komunikasi manusia; penting untuk interaksi manusia-mesin personal (Dar & Delhibabu, 2024).
2. Suara = kanal emosi relatif "jujur" → dasar SER.
3. Bias linguistik (Bella et al., 2023) — Indonesia under-resourced, khususnya data ucapan beranotasi emosi.
4. Solusi: cross-lingual transfer (Wu et al., 2021).
5. Celah: skema fine-tuning mana yang tepat untuk SER Indonesia belum diuji sistematis.
6. Paragraf pembelaan judul "dua tahap" — dengan judul diusulkan diperluas (Bagian 1), paragraf ini kemungkinan **TIDAK PERLU LAGI** atau bisa disederhanakan jadi kalimat transisi biasa.

**Revisi konkret yang perlu ditulis:**

- **Koreksi sitasi Santoso** (Bagian 2.2) — kalimat yang menyebut "Santoso... skema fine-tuning dua tahap... 90%" perlu diganti. Draf paragraf pengganti (menyatu dengan kalimat Nugroho yang sudah ada):
  > "...Nugroho et al. (2025) menunjukkan bahwa model-model dasar XLSR-Wav2Vec2 yang telah di-fine-tune untuk ucapan Indonesia dapat mencapai akurasi SER hingga 91,67%, sementara Santoso, Budianto and Dutono (2026) menetapkan baseline performa 90% akurasi dan 86% F1-score pada dataset E-SERAVD menggunakan model CNN konvensional dengan fitur MFCC, tanpa melibatkan representasi self-supervised lintas bahasa."
- **Gabungkan paragraf 1 & 2**: keduanya berakhir dengan klaim yang sama ("SER penting/berkembang"), sama-sama sitasi Dar & Delhibabu (2024). Gabung jadi satu paragraf mengalir, buang kalimat penutup P1 ("Oleh karena itu, kajian mengenai pengenalan emosi secara otomatis terus berkembang dan relevan lintas disiplin"), biarkan klaim "SER penting" cukup muncul sekali di penutup P2 ("...SER bukan sekadar topik teoretis, melainkan teknologi berpotensi dampak nyata...").
- **Tambahkan rangkaian bukti tidak langasung** untuk gap argument (menjawab "kenapa axis ini yang diuji, padahal belum pernah ada yang coba kombinasi persis ini") — draf lengkap ada di Bagian 5.

## 5. Landasan Kepustakaan (Bab 2)

### Tabel Kelompok Pembanding — ⚠️ baris Santoso perlu dikoreksi (lihat Bagian 2.2)

| Peneliti                                           | Persamaan                                                                     | Perbedaan/Gap                                                                                                                                   |
| -------------------------------------------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Eljinini & Al-Momani (2025)                        | Fondasi representasi self-supervised                                          | Umum lintas tugas, tidak menyentuh emosi/Indonesia                                                                                              |
| Nugroho et al. (2025)                              | Representasi self-supervised untuk SER Indonesia                              | Checkpoint hasil pra-latih ASR biasa, bukan fine-tuning korpus emosi bahasa asing                                                               |
| **Santoso, Budianto & Dutono (2026)** — ⚠️ KOREKSI | ~~Skema fine-tuning dua tahap~~ → **Baseline CNN+MFCC pada dataset E-SERAVD** | ~~1 konfigurasi terbaik saja, tanpa uji statistik~~ → **Tidak melibatkan representasi self-supervised atau cross-lingual transfer sama sekali** |
| Arisaputra & Zahra (2023); Tang et al. (2023)      | Pemanfaatan cross-lingual transfer                                            | Tidak menyasar korpus emosi Inggris → data emosi Indonesia secara spesifik                                                                      |

Novelty lokal: dari 924 skripsi TI FILKOM open-access, belum ada yang mengangkat SER berbasis audio.

### Rangkaian Bukti Tidak Langsung — ⚠️ paragraf baru, belum ditulis ke draf

Karena tidak ada satupun penelitian yang menguji kombinasi persis (XLSR-53 + Inggris→Indonesia + SER — apalagi setelah koreksi Santoso di atas), gap argument perlu eksplisit dirangkai dari bukti berdekatan, supaya gap-nya terasa well-motivated, bukan muncul tiba-tiba:

- **Axis 1 (single vs two-stage)**: Wu et al. (2021) — transfer lintas bahasa efektif secara teoretis untuk pemrosesan ucapan secara umum. Arisaputra & Zahra (2023) — pada backbone DAN bahasa target yang **sama persis** dengan skripsi ini (XLSR-53, Indonesia), transfer lintas bahasa terbukti menurunkan word error rate pada ASR (beda tugas: ASR, bukan SER). Tang et al. (2023) — pada **tugas yang sama** (SER), transfer lintas bahasa terbukti menunjukkan potensi generalisasi (beda pasangan bahasa, bukan Inggris→Indonesia).
- **Axis 2 (full-FT vs LoRA)**: Hu et al. (2022), paper LoRA asli — LoRA menyamai/mengungguli full fine-tuning pada model bahasa besar dengan parameter yang dilatih jauh lebih sedikit. Riset LoRA pada speech model besar secara umum (bukan spesifik Indonesia — lihat Bagian 7 poin 6) juga menunjukkan LoRA mengungguli full fine-tuning dengan biaya jauh lebih rendah, di domain SUARA (bukan cuma teks/LLM).
- **Kenapa kombinasi keduanya (bukan salah satu saja)**: kalau axis 1 (staging) membantu, pertanyaan lanjutan "seberapa murah cara mendapatkan manfaat itu" muncul dari LOGIKA INTERNAL kombinasi tersebut (relevan buat peneliti bersumber daya terbatas — motivasi awal skripsi ini) — bukan dari ada-tidaknya paper yang menguji kombinasi persis ini.

**Framing yang tepat untuk ditulis**: penelitian ini MENGUJI HIPOTESIS yang didukung bukti tidak langsung, pada kombinasi yang belum pernah diuji langsung — bukan MELENGKAPI temuan yang sudah ada (framing lama, sudah tidak akurat setelah koreksi Santoso). Ini pola justifikasi yang sah untuk riset analitik/eksploratif, dan konsisten dengan RM yang ditulis "bagaimana pengaruh" (robust terhadap hasil apapun — lihat Bagian 3.1).

Bab 2 juga sudah punya subbab teori mandiri (bukan cuma positioning) — status SELESAI: SER (2.2), Self-Supervised Learning (2.3), Wav2Vec2/XLSR (2.4), Cross-Lingual Transfer (2.5), Fine-Tuning Dua Tahap dengan formulasi θ1/θ2 (2.6), LoRA dengan formulasi BA (2.7), K-Fold CV & uji statistik (2.8), Metrik Evaluasi (2.9).

## 6. Checklist Lengkap — Semua yang Perlu Ditulis/Diubah di Draf

**Dari koreksi Santoso (Bagian 2.2) — prioritas tertinggi:**

- [ ] Tabel 2.1 baris Santoso — ganti Metode & Hasil (teks siap pakai di Bagian 2.2).
- [ ] Paragraf gap Landasan Kepustakaan — ganti kalimat soal Santoso (teks siap pakai di Bagian 2.2).
- [ ] Latar Belakang — ganti kalimat "Santoso...90%" (teks siap pakai di Bagian 4).
- [ ] Tambahkan paragraf rangkaian bukti tidak langsung ke Landasan Kepustakaan (teks siap pakai di Bagian 5).

**Dari diskusi revisi Bab 1:**

- [ ] Judul — pertimbangkan perluas ke versi yang mencakup axis full-FT/LoRA (Bagian 1). Belum dikunci, perlu didiskusikan/dikonfirmasi.
- [ ] RM1 & RM2 — ganti dari "perbedaan" jadi "pengaruh" (teks final di Bagian 3.1). RM3 tidak berubah.
- [ ] Tujuan 1 & 2 — selaraskan dengan RM versi "pengaruh" (teks final di Bagian 3.1).
- [ ] Pindahkan Hipotesis dari Bab 1 (1.3) ke Bab 3, dekat Evaluasi K-Fold (detail di Bagian 3.2).
- [ ] Pindahkan Jadwal Penelitian dari Bab 1 (1.8) ke Bab 3, subbab baru di akhir (detail di Bagian 3.2).
- [ ] Update Subbab 1.7 Sistematika Pembahasan — sebutkan Hipotesis & Jadwal sekarang ada di Bab 3, bukan Bab 1.
- [ ] Gabungkan paragraf 1 & 2 Latar Belakang (teks arahan di Bagian 4).
- [ ] Evaluasi ulang apakah paragraf pembelaan judul "dua tahap" masih perlu, setelah judul (kalau) diperluas.

**Dari review kelayakan/kecukupan sebelumnya (sudah SELESAI, dicatat sebagai referensi):**

- [x] Metrik evaluasi jadi 4 (akurasi, presisi, recall, F1) — SELESAI, Subbab 2.9.1.
- [x] Analisis kinerja komputasi (waktu, memori) — SELESAI, Subbab 2.9.2 & 3.7.
- [x] Implementasi pengukuran metrik (`time.perf_counter()`, `torch.cuda.max_memory_allocated()`) — SELESAI, Subbab 3.7.
- [x] Bab 3 refleksikan desain 4-kondisi — SELESAI, Tabel 3.2.
- [x] Dataset target dikunci ke E-SERAVD saja — SELESAI, Tabel 3.1.
- [x] Koreksi VRAM 6GB + mitigasi 8-bit Adam — SELESAI, Tabel 3.3 & Subbab 3.10.
- [x] Pilot run masuk Jadwal Penelitian — SELESAI.

## 7. Keputusan Terbuka

1. **Cakupan axis full-FT vs LoRA: tetap 4 kondisi atau disederhanakan?** Belum diputuskan. Opsi (a) tetap 4 kondisi + 8-bit Adam — paling lengkap, axis ini juga yang mengisi metrik efisiensi. (b) Sederhanakan ke 2 kondisi (single vs two-stage saja, metode difiksasi). (c) Tetap 4 kondisi, kurangi beban dari k-fold. Rekomendasi kerja: (a) — sudah dimitigasi dengan 8-bit Adam, dan axis ini yang bikin argumen efisiensi (Bagian 5, evidence chain) punya sesuatu untuk diuji.
2. **Pilot run sebelum commit ke grid penuh.** Sudah masuk Jadwal Penelitian (Tahap 3) — ini titik keputusan sebenarnya soal kelayakan riset secara keseluruhan (lihat Bagian 2.2).
3. **(Opsional, murah) Baseline frozen-backbone/linear-probing** — belum diputuskan, bukan prioritas kalau waktu ketat.
4. **Manualisasi** — masih perlu dikonfirmasi ke Bu Tirana apakah wajib. Panduan v3.0 tidak menyebutnya sebagai syarat resmi eksplisit di manapun. Manualisasi backbone asli tidak feasible (317 juta parameter). Alternatif feasible: manualisasi mekanisme LoRA di skala toy, satu blok self-attention mini, atau (paling relevan) uji statistik (Shapiro-Wilk/t-test/Wilcoxon) dengan angka contoh. **Ini bukan lagi alasan untuk pindah ke model lain** (sempat dipertimbangkan, sudah diputuskan tidak — lihat Bagian 2.2).
5. **Paper "Parameter-Efficient Fine-Tuning Large Speech Model Based on LoRA" (Ling Ou dkk., IEEE 2024)** — direvisi pemahamannya: paper ini BUKAN soal IndoWaveSentiment/Indonesia (sinyal awal itu artefak caching search), tapi TETAP berguna sebagai bukti tidak langsung generik "LoRA mengungguli full fine-tuning pada speech model besar" — dipakai di rangkaian bukti Bagian 5. Belum disitasi resmi di Daftar Pustaka; pertimbangkan tambahkan kalau dipakai di teks.

## 8. Daftar Pustaka (format UB Harvard)

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

## 10. Sumber Dokumen

Konteks di atas digali dari Claude Project terpisah: praproposal yang disetujui, `PanduanSkripsiFilkom-v3_0.pdf`, `Template-ProposalSkripsi-v3_0.docx` (dicek langsung untuk struktur Bab 1), 3 contoh skripsi FILKOM (Rifky 2026 implementatif; Ulfah 2019 & Wahyu 2021 nonimplementatif), plus full paper PDF Santoso, Budianto & Dutono (2026) yang diunggah langsung dan dibaca utuh (bukan cuma abstrak) untuk verifikasi Bagian 2.2, dan pencarian web untuk verifikasi teknis lain (spek XLSR-53/GPU, literatur LoRA). File-file itu tidak ada di repo ini — untuk detail lebih lanjut, balik ke sesi Claude.ai terkait atau upload ulang.
