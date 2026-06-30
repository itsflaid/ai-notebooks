# 📘 README — 05: File Operations

## Tentang Notebook Ini

File operations adalah salah satu hal yang paling sering bikin bingung di Jupyter, terutama karena bedanya workflow antara Jupyter lokal, JupyterLab, dan Google Colab. Notebook ini cover semuanya.

---

## Perbedaan Platform

| Operasi | Jupyter Lokal | Google Colab |
|---------|---------------|--------------|
| Upload file | Drag & drop ke file browser, atau `ipywidgets.FileUpload` | `from google.colab import files; files.upload()` |
| Download file | `FileLink`, atau ambil langsung dari direktori | `files.download('nama_file')` |
| Akses file | Path relatif dari working directory | `/content/` sebagai root |
| Google Drive | Tidak ada built-in | `drive.mount('/content/drive')` |
| Persistent storage | Ya — file tetap ada setelah session | Tidak — file hilang setelah session berakhir |

---

## Working Directory di Jupyter

```python
import os
print(os.getcwd())  # Lihat di mana lo "berada" sekarang
```

Di Jupyter lokal, working directory biasanya folder tempat lo jalankan `jupyter notebook`. Semua path relatif berawal dari sini.

```
project/
├── notebooks/
│   └── 05_file_operations.ipynb   ← lo ada di sini
├── data/
│   └── dataset.csv
└── README.md
```

Dari dalam notebook, untuk akses dataset.csv:
```python
df = pd.read_csv('../data/dataset.csv')  # naik satu level dulu
```

---

## Pattern Baca File yang Benar

### Selalu pakai `with` statement

```python
# ✅ BENAR — file otomatis ditutup setelah selesai
with open('file.txt', 'r') as f:
    isi = f.read()

# ❌ SALAH — kalau lupa close, bisa memory leak
f = open('file.txt', 'r')
isi = f.read()
f.close()  # kalau error sebelum ini, file tidak ditutup
```

### Mode buka file

| Mode | Keterangan |
|------|------------|
| `'r'` | Read (default) — error kalau file tidak ada |
| `'w'` | Write — buat file baru / overwrite yang ada |
| `'a'` | Append — tambah ke akhir file |
| `'rb'` | Read binary (untuk gambar, PDF, dll) |
| `'wb'` | Write binary |
| `'r+'` | Read dan write |

---

## Encoding — Jangan Lupa

```python
# Kalau ada karakter non-ASCII (Indonesia, Jepang, dll)
with open('file.txt', 'r', encoding='utf-8') as f:
    isi = f.read()

# Kalau tidak tahu encoding, coba detect:
# pip install chardet
import chardet
with open('file.txt', 'rb') as f:
    result = chardet.detect(f.read())
print(result['encoding'])
```

---

## Pandas read_csv Tips

```python
# File besar — baca per chunk
chunks = pd.read_csv('big_file.csv', chunksize=10000)
for chunk in chunks:
    # proses chunk
    pass

# Hanya baca kolom tertentu
df = pd.read_csv('data.csv', usecols=['nama', 'nilai'])

# Parse tanggal otomatis
df = pd.read_csv('data.csv', parse_dates=['tanggal'])

# Handle missing values
df = pd.read_csv('data.csv', na_values=['N/A', '-', ''])

# Separator lain (titik koma, tab)
df = pd.read_csv('data.csv', sep=';')
df = pd.read_csv('data.tsv', sep='\t')
```

---

## Pathlib vs os.path

`pathlib` adalah cara modern yang lebih readable. Prefer pathlib:

```python
# os.path — cara lama
import os
path = os.path.join('data', 'raw', 'file.csv')
filename = os.path.basename(path)
exists = os.path.exists(path)

# pathlib — cara modern
from pathlib import Path
path = Path('data') / 'raw' / 'file.csv'
filename = path.name
exists = path.exists()
```

### Operasi pathlib yang sering dipakai

```python
p = Path('data/raw/dataset.csv')

p.name       # 'dataset.csv'
p.stem       # 'dataset'
p.suffix     # '.csv'
p.parent     # Path('data/raw')
p.resolve()  # absolute path

p.exists()       # True/False
p.is_file()      # True/False
p.is_dir()       # True/False

p.mkdir(parents=True, exist_ok=True)  # buat direktori
p.unlink()       # hapus file
p.rename(...)    # rename/move

list(p.parent.glob('*.csv'))    # cari file
list(p.parent.rglob('*.py'))    # cari rekursif
```

---

## Gotchas

### 1. FileNotFoundError
```python
# Selalu cek dulu sebelum buka
if Path('data.csv').exists():
    df = pd.read_csv('data.csv')
else:
    print("File tidak ditemukan!")
```

### 2. Path separator beda di Windows vs Mac/Linux
```python
# ❌ Jangan hardcode separator
path = "data\\raw\\file.csv"   # Windows only

# ✅ Pakai pathlib atau os.path.join
path = Path("data") / "raw" / "file.csv"  # cross-platform
```

### 3. File di Colab hilang setelah session
Di Colab, semua file di `/content/` hilang kalau session timeout. Solusinya:
- Mount Google Drive: `drive.mount('/content/drive')`
- Simpan ke Drive setelah proses
- Gunakan `files.download()` untuk ambil hasilnya

---

➡️ **Next:** [README_06.md](README_06.md) — NumPy Fundamentals
