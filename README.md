# SAMPEU-AI Training

Klasifikasi penyakit daun singkong. Perbandingan MobileNetV2, ResNet50, dan EfficientNetB0 pada dataset yang sama.

## Dataset

Kompetisi Kaggle: `cassava-leaf-disease-classification` (21.397 gambar, 5 kelas: CBB, CBSD, CGM, CMD, Healthy).

Tambahkan 2 input di Kaggle:

1. Kompetisi `cassava-leaf-disease-classification`
2. File `manifest/manifest_split_seed42.csv` dari repo ini (upload sebagai dataset Kaggle privat lalu Add Input)

Verifikasi manifest sebelum training. SHA-256 harus sama:

```
aac30db60c0f6172776a39d2077a9dba4e1377f6036c5bd8f510aee9b96f4b48
```

Isi: train 14.978, validation 3.210, test 3.209.

## Urutan run

1. `notebooks/01_audit_dataset.ipynb` — audit train.csv vs train_images
2. `notebooks/02_training_mobilenetv2.ipynb` — Adam
3. `notebooks/03_training_resnet50.ipynb` — Arif
4. `notebooks/04_training_efficientnetb0.ipynb` — Daffa

Tiap notebook training dijalankan 3 kali dengan seed 42, 123, 2026. Tiap seed diawali Restart. Config: input 224, batch 32, Adam, Fase 1 LR 1e-3 (maks 10 epoch), Fase 2 LR 1e-4 (maks 20 epoch), patience 3, checkpoint dari val_loss terkecil. Pemilihan model dari macro-F1 validation, bukan test.

## Hasil

Ringkasan ada di `results/`. Tiap run menyimpan `config.json`, `validation_metrics.json`, `confusion_matrix.csv`, `history.csv`.

Validation (3.210 gambar):

| Model | Accuracy | Macro-F1 |
| --- | --- | --- |
| EfficientNetB0 | 0.7915 ± 0.0090 | 0.6793 ± 0.0051 |
| MobileNetV2 | 0.7474 ± 0.0204 | 0.6449 ± 0.0187 |
| ResNet50 | 0.7373 ± 0.0114 | 0.6351 ± 0.0140 |

Test internal hanya untuk EfficientNetB0 (`results/test_eval_final_efficientnetb0/`): accuracy 0.7915 ± 0.0145, macro-F1 0.6864 ± 0.0086, n = 3.209.

## Model final

Model aplikasi tidak disimpan di repo ini (batas 100 MB per file GitHub). File `efficientnetb0_seed123_best.keras` (47 MB) ada di repo aplikasi Daffa.

- Repo aplikasi: https://github.com/Wandarix/Sampeu.ai
- SHA-256: `992e479f5790bca11e13a6d1a8c0dfa799d6be84c04805440600810dfcb53b01`
- Preprocessing: resize 224x224 RGB + `efficientnet.preprocess_input`, tanpa augmentasi
- Urutan kelas: CBB, CBSD, CGM, CMD, Healthy
