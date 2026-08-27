# Catatan Perubahan dari Template Asli

Proposal skripsi ini dibangun dari template LaTeX di
github.com/arawsardni/skripsi, dengan penyesuaian berikut agar sesuai
Panduan Skripsi Filkom UB v3.0:

## Perubahan format (main.tex)
- Font: Helvetica -> Calibri (font yang diwajibkan Panduan Skripsi
  Filkom UB v3.0, Lampiran A.3; terpasang di mesin pengembangan ini jadi
  dipakai langsung). Kalau compile di mesin lain yang TIDAK punya
  Calibri (mis. Overleaf, Linux, Mac tanpa MS Office), ganti "Calibri"
  -> "Carlito" pada baris \setmainfont di preamble.tex -- itu pengganti
  metrik-kompatibel resmi untuk Calibri (dipakai LibreOffice sebagai
  substitusinya), sehingga tata letak/line-break tetap identik.
- Spasi: 1,5 -> spasi tunggal (badan teks), sesuai Lampiran A.4 Panduan
  Skripsi Filkom UB v3.0.
- Kompilasi WAJIB pakai XeLaTeX (bukan pdflatex), karena pakai fontspec:
    xelatex -interaction=nonstopmode main.tex   (jalankan 2x)

## Perubahan cakupan (main.tex)
- \include dibatasi ke chapters/bab1-bab3 + backmatter/referensi.
  Proposal skripsi Filkom UB hanya mencakup Bab 1-3 + Daftar Referensi
  (bab4-bab7 masih ada di folder chapters/ sebagai draft kosong,
  di-comment di main.tex, untuk dipakai nanti setelah proposal disetujui
  dan penelitian benar-benar dilaksanakan).
- frontmatter/lembar_pengesahan, pernyataan_keaslian, abstrak, dan
  kata_pengantar di-comment di main.tex karena isinya khas skripsi FINAL
  pasca-sidang (mis. "telah diuji dan dinyatakan lulus", abstrak yang
  mensyaratkan hasil penelitian) -- belum relevan untuk tahap proposal.
  Aktifkan kembali saat naskah menjadi skripsi lengkap.

## Perubahan istilah bab
- "Tinjauan Pustaka" -> "Landasan Kepustakaan" (istilah resmi Panduan
  Skripsi Filkom UB v3.0).
- Urutan subbab Bab 1: Latar Belakang, Rumusan Masalah, Tujuan, Manfaat,
  Batasan Masalah, Sistematika Pembahasan (urutan Manfaat sebelum
  Batasan Masalah, mengikuti pola 3 contoh skripsi Filkom yang dibaca).

## Perubahan gaya sitasi (backmatter/referensi.tex)
- Harvard Anglia Ruskin University (ARU) -> gaya Harvard yang sudah
  dipakai dan disetujui pembimbing pada dokumen praproposal skripsi ini.
  Kedelapan referensi diisi dari praproposal yang sudah disetujui.

## Isi Bab 1-3
Diisi penuh berdasarkan dokumen praproposal yang sudah disetujui
pembimbing (Bu Tirana), dikembangkan mengikuti pola struktural dari tiga
contoh skripsi Filkom (nonimplementatif 2019, nonimplementatif 2021,
implementatif 2026).

## Yang MASIH PERLU DIISI
- Bab 3, subbab 3.9.2 Perangkat Keras (Hardware): masih placeholder
  [...]. Isi sesuai spesifikasi GPU/komputasi yang sebenarnya digunakan.

## Dependensi sistem yang ditambahkan saat pengujian
Environment kompilasi awalnya belum punya paket berikut -- jika Anda
compile di mesin lain (bukan Overleaf) dan menemui error serupa, install:
- texlive-lang-other (untuk babel bahasa Indonesia, paket "indonesian.ldf")
- Font Carlito (biasanya sudah ada di Linux modern / paket fonts-crosextra-carlito)

Paket "sansmathfonts" yang direferensikan di komentar main.tex versi
asli TIDAK tersedia di lingkungan pengujian dan sudah dihapus dari
dependensi; font matematika memakai default XeLaTeX (Latin Modern Math).

## Pemisahan main.tex -> preamble.tex + main_proposal.tex + main_skripsi.tex

`main.tex` tunggal di atas (yang mengandalkan comment/uncomment manual
untuk berpindah antara cakupan proposal dan skripsi penuh) dipecah jadi:
- `preamble.tex`: semua paket/style, di-\input bareng oleh kedua entry
  point di bawah supaya tidak dobel-tulis.
- `main_proposal.tex`: cakupan proposal (Bab 1-3 + referensi + lampiran,
  tanpa pengesahan/orisinalitas/abstrak/prakata). Ini yang dikumpulkan
  sekarang.
- `main_skripsi.tex`: cakupan skripsi penuh (menambahkan keempat halaman
  frontmatter tadi; Bab 4 dst. masih di-comment). Belum dipakai, tapi
  sudah bisa langsung di-compile begitu proposal disetujui -- tidak
  perlu comment/uncomment main.tex lagi.

Alasan: isi Bab 1-3 identik di kedua tahap (proposal adalah snapshot dari
skripsi yang sama, bukan dokumen terpisah), jadi kalau dibuat dua folder
dengan isi di-copy, revisi Bab 1-3 pasca-seminar proposal berisiko cuma
kesinkron di salah satu salinan. Dengan satu sumber per bab dan dua entry
point tipis, kedua PDF selalu bisa di-generate ulang kapan saja tanpa
risiko drift.

`frontmatter/cover.tex` diparameterisasi lewat `\doctypelabel` dan
`\doctypesubtitle` (didefinisikan kosong di preamble.tex, di-override per
entry point) supaya teks "PROPOSAL SKRIPSI" vs "SKRIPSI" tidak perlu dua
salinan cover.

## Lampiran A & B ditambahkan

Kedua template resmi (proposal maupun skripsi penuh) mewajibkan Lampiran
A (Persyaratan Fisik dan Tata Letak) dan Lampiran B (Penggunaan Bahasa)
sebagai apendiks -- sebelumnya tidak ada sama sekali di LaTeX ini.
Ditambahkan sebagai `backmatter/lampiran_a.tex` dan `lampiran_b.tex`,
isinya disalin verbatim dari Panduan Skripsi Filkom UB v3.0 (bukan boleh
diparafrase bebas, karena ini teks aturan resmi yang memang wajib
dilampirkan apa adanya).

## Bab 4 dst.: belum diputuskan, jangan asumsikan judulnya

Dibaca dari Panduan Skripsi Filkom UB v3.0 langsung: judul bab TIDAK
wajib literal "Hasil"/"Pembahasan" -- boleh diganti nama yang lebih
deskriptif/tematik ("Judul bab pun tidak harus secara eksplisit 'Hasil'
dan 'Pembahasan' tetapi dapat digantikan dengan nama yang lebih
deskriptif dan tematik"). Contoh struktur resmi untuk penelitian
nonimplementatif eksperimental (paling cocok untuk topik skripsi ini --
perbandingan strategi fine-tuning secara statistik, bukan pembangunan
sistem) adalah:

    Bab 1 Pendahuluan
    Bab 2 Landasan Kepustakaan
    Bab 3 Metodologi Penelitian
    Bab 4 Hasil
    Bab 5 Pembahasan
    Bab 6 Penutup

dengan catatan resmi "Jika diperlukan, Bab 4 dapat digabungkan dengan
Bab 5, menjadi Hasil dan Pembahasan" (jadi 5 bab total). Scaffold lama di
`chapters/bab4.tex`-`bab7.tex` (Perancangan Sistem/Implementasi/Pengujian
dan Analisis/Kesimpulan dan Saran, 4 bab) sebenarnya meniru pola
"implementatif pembangunan" di panduan yang sama, bukan pola
nonimplementatif -- jumlah dan judul bab akhir masih perlu diputuskan
bareng dosen pembimbing, bukan diasumsikan dari scaffold generik ini.
