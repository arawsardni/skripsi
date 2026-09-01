# Review Iterasi 1 (28-8-2026 9.30) ((done))

saya akan mengevaluasi hasil generate latex main_proposal.pdf kondisi saat ini setelah dilakukan command "xelatex -interaction=nonstopmode main_proposal.tex":

1. Daftar Isi, Daftar Gambar dan Daftar Tabel masih belum terbuat otomatis saat di generate
2. ukuran font judul dengan tulisan "PROPOSAL SKRIPSI" di cover nampak masih sama
3. jarak antar tulisan PROPOSAL SKRIPSI dengan Disusun Oleh terlalu lebar
4. paragraf pertama di latar belakang belum terindentasi
5. sistematika pembahasan masih sampai bab3 saja, seharusnya sampai bab kesimpulan dan saran
6. di bab 2 setiap sub bab hanya membahas 1 objek bahasan, ada beberapa sub bab contohnya 2.3 membahas 2 objek yaitu self supervised dan wav2vec/xlsr jika ingin membahas keduanya maaka harus di pisah berbeda sub bab
7. untuk setiap sub bab di bab 2 sertakan sitasi pendukung darimana penjelasan utama/penjelsana konsep tersebut diambil bukan dari penelitian sebelumnya yang sudha dibahas di sub bab penelitian terdahulu/kajian pustaka , contoh yang sudah benar ada di sub bab 2.4 untuk sitasi penjelasannya, namun jangan sama sekali bahass penelitihan tedahulu lagi di sini. jika ingin membahas buat sub bab baru di 2.1, jadi misal 2.1.1
8. buat setiap sub bab selain kajian pustaka berisi hanya penjelasannya secara merinci disertai diagram arsitektur, equation konsep nya dan bagaimana cara kerjanya
9. di bab 3 setelah bagian pengumpulan data, jelaskan datanya di bawahnya
10. untuk bab lampiran di proposal ini tidak perlu

# Review Iterasi 2 (28-8-2026 16.00) ((done))

1. Lampiran boleh dikosongin dulu, karena lampiran yang ada di template itu maksudnya lampiran dari dokumen template dengan bagaimana penulisan seacara teknis dan penggunaan bahasanya, jadi buatkan saja 1 haolaman kosong berjudl Lampiran
2. rumusan masalah 2 kebaca cuma soal jalur two-stage aja, harusnya jelas berlaku juga buat single-stage
3. belum ada rumusan hipotesis h0/h1, padahal wajib buat penelitian eksperimen kc
4. metrik evaluasi cuma akurasi sama f1-score, rubrik minta lebih dari 2 metrik
5. belum ada analisis kinerja komputasi sama sekali
6. jadwal penelitian belum ada di bab 1
7. kajian pustaka belum ada sintesis gap per kelompok penelitian pembanding
8. belum ada argumen novelty lokal, belum disebut belum ada skripsi filkom yang angkat ser berbasis audio
9. baris nugroho di tabel kajian pustaka sama di latar belakang kurang tepat, ditulis seolah mereka fine-tune checkpoint sendiri, padahal yang mereka pakai itu 2 model dasar xlsr-wav2vec2 yang sudah di-fine-tune orang lain buat bahasa indonesia (common voice + openslr jawa/sunda)
10. bagian fine-tuning target di bab 3 masih ambigu, strategi nomor 1 "single-stage, dilatih langsung" gak jelas itu full-ft atau lora
11. belum ada penjelasan kenapa pakai xlsr-53 dasar, bukan yang udah di-fine-tune buat bahasa indonesia
12. belum jelas fine-tuning sumber itu dijalanin sekali apa berkali-kali per fold
13. perangkat keras masih placeholder kosong

# Review Iterasi 3 (31-8-2026 16.00) ((done))

1. di bagian latar belakang, nampaknaya paragraf 1 dan 2 bisa digabung adgar latar belakang lebih compact dan mudah untuk dibaca
2. rumusan masalah 1 masih nyebut metrik eksplisit (akurasi dan f1-score), harusnya generik "performa" aja
3. rumusan hipotesis h0 rm1 juga masih ada embel-embel "(akurasi dan f1-score)"
4. tujuan belum nyebut presisi sama recall, cuma akurasi sama f1-score
5. tujuan poin 2 belum nyebut bakal dibandingin juga efisiensi komputasinya (waktu+memori)
6. metrik evaluasi yang keitung buat rubrik ">2 metode evaluasi" kemungkinan cuma metrik kualitas doang, akurasi+f1 masih 2, perlu ditambah presisi+recall
7. bab 2 subbab akurasi/f1-score belum jelasin presisi sama recall sebagai metrik sendiri, cuma numpang lewat di rumus f1
8. dataset target masih dua-duanya sekaligus (e-seravd + indowavesentiment), beda skala jauh (5000an vs 300 sampel) dan beda skema kelas (4 vs 5), bikin beban eksperimen dobel di timeline yang cuma ~14 minggu
9. jadwal penelitian masih placeholder "1 semester" generik, padahal ada tenggat resmi p1/p2/semhas yang jauh lebih ketat
10. bab 3 pengumpulan data belum nyebut pilot run buat verifikasi waktu/vram sebelum lanjut ke grid penuh
11. bab 3 analisis kinerja komputasi belum nyebut cara ngukur waktu/vram secara teknis
12. tabel vram di perangkat keras belum lengkap, angka full fine-tuning "naif" cuma sisa 1gb buat aktivasi itu riskan oom, belum ada mitigasi 8-bit adam optimizer

# Review Iterasi 4 (1-9-2026 11.00) ((done))

1. tambahin daftar isi, daftar tabel, dan daftar gambar di daftar isi
2. sitasi santoso salah total di draf, papernya ternyata cnn+mfcc dilatih dari nol, bukan two-stage fine-tuning/cross-lingual transfer/xlsr sama sekali -- angka 90%/86% f1 bener tapi atribusi metodenya salah
3. judul belum sebut axis metode adaptasi (full-ft vs lora), cuma "dua tahap" doang
4. rumusan masalah 1 & 2 masih pakai kata "perbedaan", harusnya "pengaruh" ngikutin pola skripsi kating
5. paragraf 1 & 2 di latar belakang sama-sama nutup pakai klaim "ser penting" dan sitasi yang sama, harusnya digabung
6. kajian pustaka belum ada rangkaian bukti tidak langsung buat justifikasi kombinasi axis yang belum pernah diuji langsung bareng
7. hipotesis, jadwal penelitian, dan analisis kinerja komputasi dihapus dari proposal (bukan dipindah ke bab 3 kayak saran awal di skripsi_context.md)
