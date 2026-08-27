# Catatan Perubahan dari Template Asli

Proposal skripsi ini dibangun dari template LaTeX di
github.com/arawsardni/skripsi, dengan penyesuaian berikut agar sesuai
Panduan Skripsi Filkom UB v3.0:

## Perubahan format (main.tex)
- Font: Helvetica -> Carlito (pengganti metrik-kompatibel resmi untuk
  Calibri; dipakai LibreOffice sebagai substitusi Calibri). Jika Anda
  compile di sistem dengan Calibri asli terpasang (mis. Windows + MS
  Office), cukup ganti "Carlito" -> "Calibri" pada baris \setmainfont.
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
