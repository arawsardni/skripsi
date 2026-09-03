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

# Review Iterasi 5 (1-9-2026 16.00) ((done))

1. desain riset dua tahap + full-ft vs lora dihapus total, ganti jadi bandingin representasi self-supervised (xlsr-53, dibekukan/frozen) sama fitur hand-crafted (mfcc) di satu dataset e-seravd v1.1 -- bukan direvisi, korpus sumber ravdess/crema-d dan axis lora ikut hilang semua
2. judul ganti total ngikutin desain baru, representasi self-supervised dibandingkan fitur hand-crafted
3. rumusan masalah jadi 2 butir, pengaruh representasi dan konsistensi hasil antar-fold
4. atmaja and sasou (2022) jadi rujukan jangkar baru di kajian pustaka, gantiin posisi yang dulu dipegang argumen cross-lingual transfer
5. baris santoso di tabel kajian pustaka dikoreksi lagi ke angka final 86% akurasi/86% f1-macro, bukan 90% kayak draf sebelumnya
6. subbab cross-lingual transfer dan lora di bab 2 dihapus total, gantinya subbab baru soal mfcc lengkap sama pipeline ekstraksi, equation skala mel, dan diagram
7. bab 3 sudah tidak ada lagi tahap fine-tuning sumber/target terpisah, gantinya ekstraksi representasi -- xlsr-53 dibekukan (bukan di-fine-tune), yang dilatih cuma classifier ringan
8. dataset e-seravd yang dipakai sekarang versi v1.1 dengan n=1.200 terverifikasi, bukan ~5.049 di draf lama yang tidak jelas sumbernya
9. tabel vram dan narasi mitigasi 8-bit adam di perangkat keras dihapus, karena representasi yang dibekukan sudah tidak butuh itu lagi

# Review Iterasi 6 (2-9-2026 11.00) ((done))

1. skema pembagian data acak vs independen-film yang tadinya diam-diam diterapkan sebagai metodologi yang benar, sekarang diangkat jadi dimensi kedua dan inti permasalahan yang diuji eksplisit -- jadi desain 2x2 representasi x skema split, bukan 1 perbandingan doang
2. rumusan masalah 1 ganti fokus ke pengaruh skema pembagian data, rumusan masalah 2 ganti fokus ke apakah pengaruh itu beda antar representasi
3. confound film-emosi di e-seravd yang sebelumnya cuma catatan metodologi di bab 3, sekarang jadi argumen sentral di latar belakang bab 1 lengkap sama angka klip yang didominasi 1-2 film doang
4. kondisi a (mfcc+cnn, split acak) sekarang didesain jadi replikasi setia baseline santoso, classifier mfcc balik pakai cnn ngikutin santoso, bukan classifier seragam kayak revisi sebelumnya
5. tabel 4 kondisi eksperimen muncul lagi di bab 3, tapi axisnya representasi x skema split, beda sama desain paling awal yang axisnya stage x metode adaptasi
6. hipotesis tetap tidak ditulis meski context doc sudah 3 kali menyiratkan sebaliknya, keputusan ini sudah dikonfirmasi ulang tiap kali muncul

# Review Iterasi 7 (2-9-2026 17.00) ((done))

1. Judul sementara berubah jadi "Analisis Pengaruh Skema Pembagian Data pada Dataset E-SERAVD terhadap Performa Klasifikasi Emosi Ucapan Berbahasa Indonesia"
2. fokus skripsi sekarnag di pengaruh skema pembagian data sebagai isi utama dan percobaan representasi fitur lain (ssl) sebagai pembanding (sampingan)
3. daftar daftar di frontmatters kembali hilang saat di generate
4. latar belakang terlalu panjang 1 paragraf mungkin seharusnay berisi 5-7 kalimat saja, dan 4 paragraf saja
5. batasan maslah nomer 1 dan 2 harusnya ditaruh nomer 2 dan 3, serta nomer 3 menjadi nomer 1, dan kalimat nomer 3 tambahkan penjelasan dari 3 versi yg tersedia (1.0, 1.1 dan 1.2), nomer 5 bagi menjadi dua nomer di bagian uji statistik lainnya, nomer 6 tidak perlu ditulis
6. di sistematika pembahasan bagian bab 2 jangan lupa cari paper paper terdahulu yg membahas pengaruh pembagian skema data juga, bagian buat bab 4 menjadi perancangan yang membahas tentang perancangan sistem serta algoritme dalam eksperimen di skripsi ini, lalu bab 5 tentang implmeentasi dimana isisnya implmeentasi kode kode esensial, dan bab 6 isinya adalah hasil dan pembahasan dan uji pengaruh ada di bagian ini, lalu bab 7 penutup
7. bab 2 fokus kepada paper paper yang membahas pengaruh skema pembagian data dan juga bagaiamana pengaruh nay di bagian representasi fitur ssl vs handcrafted dari penelitian terdahulu
8. di bab 2 gunakan diagram dari paper sumber/ rferensi yang terkenal daripada membuat nya sendiri seperti dibagian diagram SER, SSL, Wav2Vec dan XLSR (mohon ini di pecah saja jadi dua sub bab wav 2 vec dan xlsr53), MFCC, dan KFOLD dan pengujian statistik juga pisah sub bab saja jadi 1 sub bab hanya mmebahas 1 lingkup objek/teori

# Review Iterasi 8 (2-9-2026 20.00) ((done))

1. ada perubahan besar lagi daripada membahas membandingkan representasi fitur antara mfcc dan ssl, akhirnya kita cuma mau fokus membandingkan algorimta baseline dari paper dengan algoritma SOTA untuk SER indonesia ssaati ini yaitu xlsr-53+lora , jadi membandignkan apakah ekpserimen risiko group leakage nanti yang akan kita coba ini berpaengaruh di model baseline dengan hand crafted feature nya dan sota dengan ssl nya
2. jadi judulnya pun bisa diganti menjadi "Pengaruh Skema Pembagian Data Berbasis Film terhadap Akurasi Pengenalan Emosi Ucapan Berbahasa Indonesia pada Dataset E-SERAVD"
3. paragraf 2 dari latar belakang, harusnya membahas pembuatan dataset dulu daripada hasil model baseline, jadi boleh di kalimat pertama membahas paper pembuatan dataset dulu agar nyambung sama kalimat terakhir pargarf 1, jangan menyebutkan versi dataset nya dilatar belakang (tuliskan versi nya di batasan masalah aja).
4. untuk paragarf 3 yang membahas tentang percobaan ketahanaan ssl vs handcrafted dalam skema pembagian data nampaknya perlu diganti dan dicari lebih dari 1 paper atau kalau paper yg ada sekarnag sudah cukup juga gak papa, bisa cari lebih dulu paper paper yg mendukung untuk latar belakang kenapa kita mencoba xlsr-53 sebagai sota, jadi paragraf nya seperti membuktikan kalau memnag untuk ser indoensia yg paling baik itu sekarang xlsr dengan ssl nya daripada menceritakan apa yang akan kita lakukan di penelitian kali ini
5. baru untukk pargarf 4 kurang lebih sama seperti yang sekarnag tapi jangan lupa tailored dengan pembaruan paragraf paragraf diatas nya
6. di rumusan maslaah dengan kalimat tnaya juga seperti bagaimana/apakah, jangan gunakan "sebagai pembanding", begitu juga unntuk sub bab tujuan dan manfaatt jangan gunakan sebagai pembanding,
7. jadi rumusan maslah bisa seperti ini: RM1: bagaimana pengaruh skema pembagian data berbasis film (random vs movie-independent) terhadap performa klasifikasi pengenalan emosi ucapan berbahasa indonesia? RM2: Apakah pengaruh tersebut konsisten pada algoritma berbasis fitur tangan (CNN+MFCC) maupun representasi pralatih (XLSR-53+LoRA)?
8. dan untuk sub bab tujuan nay menjadi: T1 : mengetahui pengaruh skema pembagian data berbasis film (random vs movie-independent) terhadap performa klasifikasi pengenalan emosi ucapan berbahasa. T2 : Menganalisis konsistensi pengaruh skema pembagian data berbasis film (random split vs movie-independent split) terhadap akurasi pengenalan emosi ucapan Berbahasa Indonesia pada algoritma berbasis fitur tangan (CNN+MFCC) dan algoritma berbasis representasi pralatih (XLSR-53+LoRA).
9. di sub bab batasan maslaah tidak perlu membahas terlalu dalam tentang versi dataset bilang saja versi 1.1, nanti pembahasan lebih lanjut dijelaskan di bab 3
10. lebih jelaskan lagi bagaimana yang dimaksud dengan acak vs independen film di skripsi ini dibagian bab 2, khususnya di sub baba bari data leakage/group leakage
11. untuk kajian pustaka nampaknaya perlu pivot ke paper lain yang lebih relevan ke maslaha group leakage ini daripada membahas penelitian terdahulu tentang ssl terlalu banyak
12. pembahasan ssl di kajian pustaka juga tolong carikan paper yang relevan dengan skripsi ini yaitu yang membahas juga ssl vs handcrafted terhadap skema pembagian atau ketahanan nya dalam risiko data leakage/group leakage kalau ada
13. buat pargaf penjelas setelah tabel penelitian di kajian pustakanya boleh dihapuskan saja, namun buat isi tabel 2.1 lebih lengkap lagi di bagian metode dan hasil, metode boleh berisi 2-3 kalimat
14. di bab 2 tambahkan sub bab data leakage/group leakage di letakan setelah sub bab ser, lalu diikuti sub bab baru hand crafted, lalu diikuti subbab baru mfcc, lalu sub bab baru ttg cnn, lalu sub bab ssl, lalu baru wav2vec, lalu xlsr, lalu setelah itu diiktui sub bab akurasi presisi, recall dan f1 score yang ganti judul sub bab nya menjadi evaluasi model namun isisnya tetap sama membahas 4 metriks itu, lalu diikuti sub bab validasi silang k fold dan terakhir sub bab pengujian signifikaansi statistik
15. karena foksunaya sudah bukan ekstraksi representasi, bab 3.4 ubah menjadi perancangan algoritma lebih ke menjalasakan perancangan sistem klasifikasi dari 4 kondisinyanya nanti seperti apa
16. paragraf 2 di evaluasi kfold kan membahas uji signifikansi ini seharunsya berada di sub bab analisis hasil
17. di semua sub bab dari bab 2 dan 3 jangna suka memention hal hal di bab lain jika ingin menjelaskan jelaskan lagi di sub bab tersebu, jangan sampai setiap sub bab me linking sesuatu di luar sub bab nya

# Review Iterasi 9 (3-9-2026) ((done))

1. tambah 5 sitasi baru soal group leakage dan lora-pada-ucapan, hasil websearch dari sesi claude chat terpisah (websearch/webfetch di claude code masih error)
2. kapoor and narayanan (2023) jadi sitasi definisi data leakage/group leakage di latar belakang paragraf 2 dan bab 2.3, sebelumnya istilah itu gak ada sitasinya sama sekali
3. antoniou et al. (2023) nambahin bukti spesifik ke ser soal speaker-dependent vs speaker-independent split (79,58% vs 73,01% wa), masuk ke latar belakang paragraf 2 dan baris baru tabel 2.1 setelah tang et al.
4. wu et al. (2024) emo-superb nambahin angka skala-lapangan ke latar belakang paragraf 4, 80% studi ser gak reproducible dan leakage bikin performa naik semu ~4%
5. wang et al. (2023) dan lashkarashvili et al. (2024) nambahin bukti lora spesifik ke wav2vec2-family dan ser di bab 2.10, sebelumnya cuma nyitasi hu et al. yang general-purpose nlp
6. sitasi baru ini gak diverifikasi ulang di claude code karena websearch/webfetch masih error, dipercaya di level yang sama kayak riset skripsi_context.md
