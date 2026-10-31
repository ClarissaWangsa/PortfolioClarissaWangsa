# Project Content Sheet

Folder ini adalah template content management sederhana untuk portfolio.
File CSV dapat di-import ke Google Sheets sebagai tiga tab:

- `projects.csv` → satu baris per project
- `sections.csv` → isi detail yang bisa berbeda antar-project
- `media.csv` → gambar, thumbnail, dan gallery

## Struktur gambar

Simpan asset gambar di dalam folder project masing-masing:

```text
project-assets/
├── proj1-daihatsu/
│   ├── thumbnail.jpg
│   ├── gallery-01.jpg
│   └── gallery-02.jpg
├── proj2-house-of-goodness/
├── proj3-dunia-tanpa-syarat/
├── proj4-petualangan-paddle-pop/
├── proj5-freeport-3d-edirectory/
├── proj6-bca-solitaire/
├── proj7-iquartz-organic-growth/
├── proj8-engagement-campaign-surge/
└── proj9-integrated-campaign-bca/
```

Folder sebaiknya berada di dalam repository portfolio ini, bukan hanya di lokasi lain di laptop, supaya gambar bisa ikut tampil saat website dipublish.

## Status yang digunakan

- `published` → siap ditampilkan
- `draft` → masih dikerjakan
- `needs-review` → data perlu dicek
- `needs-upload` → asset gambar belum tersedia

## Cara mengisi project baru

1. Tambahkan satu baris baru di `projects.csv`.
2. Tambahkan satu atau beberapa baris di `sections.csv`.
3. Tambahkan gambar dan catat path-nya di `media.csv`.
4. Gunakan `order` untuk menentukan urutan section atau gambar.
5. Kosongkan field yang memang belum tersedia; tidak perlu mengarang data.

