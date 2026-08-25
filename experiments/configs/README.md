# configs

Satu file config (yaml/json) per skenario eksperimen: hyperparameter, seed, versi/split
data, model yang dipakai. Tujuannya supaya tiap angka yang dilaporkan di Bab 6 bisa
ditelusuri balik ke satu file config yang persis, bukan ke kombinasi argumen command-line
yang sudah lupa cara menjalankannya lagi.

Nama file disarankan cocok dengan nama folder di `../results/`, misalnya
`configs/eksperimen-a.yaml` menghasilkan `results/eksperimen-a/`.
