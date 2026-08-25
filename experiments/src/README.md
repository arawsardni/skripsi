# src

Kode pipeline yang reusable: loading/cleaning data, ekstraksi fitur, definisi model,
training, evaluasi. Setiap skrip idealnya bisa dijalankan dari command line dengan sebuah
config dari `../configs/`, misalnya:

```bash
python src/train.py --config configs/eksperimen-a.yaml
```

Hindari menaruh path atau hyperparameter hardcode di sini — taruh di `configs/` supaya
tiap run eksperimen jelas parameternya apa.
