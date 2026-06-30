# 📘 README — 03: Imports & Packages

## Tentang Notebook Ini

Import adalah hal pertama yang lo tulis di hampir setiap notebook. Kalau lo salah import atau tidak tahu cara install package, satu jam bisa habis buat debugging yang seharusnya 5 menit.

---

## Hierarki Import Python

```
Python Standard Library     ← built-in, tidak perlu install
        ↓
Third-party Packages        ← install via pip (numpy, pandas, dll)
        ↓
Local Modules               ← file .py buatan lo sendiri
```

---

## Alias Standar yang Wajib Dipakai

Di dunia data science ada konvensi alias yang sudah jadi standar global. Jangan kreatif di sini — pakai yang standar supaya kode lo mudah dibaca orang lain:

```python
import numpy as np              # WAJIB np
import pandas as pd             # WAJIB pd
import matplotlib.pyplot as plt # WAJIB plt
import seaborn as sns           # WAJIB sns
import tensorflow as tf         # WAJIB tf
import torch                    # tidak ada alias standar
from sklearn import ...         # tidak ada alias standar
```

Kalau lo lihat kode ML orang lain di GitHub atau Kaggle, mereka **pasti** pakai alias ini.

---

## Perbedaan Cara Import

```python
# 1. Import seluruh module — akses via prefix
import numpy
arr = numpy.array([1, 2, 3])    # verbose, jarang dipakai

# 2. Import dengan alias — standar
import numpy as np
arr = np.array([1, 2, 3])       # clean, recommended

# 3. Import spesifik — langsung pakai nama
from numpy import array, zeros, ones
arr = array([1, 2, 3])          # tanpa prefix

# 4. Import semua (HINDARI ini)
from numpy import *              # ❌ pollutes namespace
array([1, 2, 3])                 # bisa konflik nama
```

### Kapan pakai cara 3 vs cara 2?

Pakai `from x import y` kalau:
- Hanya butuh 1-2 fungsi dari library besar
- Fungsi itu dipakai sangat sering (misal `from pathlib import Path`)
- Ini fungsi dari standard library (`from datetime import datetime`)

Pakai `import x as alias` kalau:
- Library akan dipakai banyak fungsi berbeda
- Library punya alias standar (numpy, pandas, dll)

---

## Cara Install Package di Berbagai Platform

### Jupyter Notebook / JupyterLab (lokal)
```python
!pip install nama-package
```

### Google Colab
```python
!pip install nama-package
# atau lebih modern:
import subprocess
subprocess.run(["pip", "install", "nama-package"])
```

### Terminal (lebih recommended untuk projek serius)
```bash
pip install nama-package
# atau dengan virtual environment aktif:
python -m pip install nama-package
```

---

## Standard Library yang Paling Berguna

| Module | Kegunaan |
|--------|----------|
| `os` | Operasi OS: file, direktori, env vars |
| `sys` | Info Python runtime, path |
| `pathlib` | Manipulasi path (lebih modern dari os.path) |
| `json` | Baca/tulis JSON |
| `csv` | Baca/tulis CSV (sering digantikan pandas) |
| `datetime` | Tanggal dan waktu |
| `random` | Angka random |
| `math` | Fungsi matematika |
| `collections` | Counter, defaultdict, OrderedDict |
| `itertools` | Kombinasi, permutasi, chain |
| `functools` | reduce, partial, lru_cache |
| `re` | Regular expressions |
| `time` | Timing, sleep |
| `subprocess` | Jalankan shell command dari Python |

---

## Virtual Environment — Kenapa Penting?

Virtual environment itu kayak "ruang terpisah" buat setiap project. Tanpanya, semua project pakai versi library yang sama — dan kalau satu project butuh numpy 1.20 sementara yang lain butuh 1.24, bakal bentrok.

```bash
# Buat venv
python -m venv nama_env

# Aktifkan (Windows)
nama_env\Scripts\activate

# Aktifkan (Mac/Linux)
source nama_env/bin/activate

# Install packages di dalam venv
pip install numpy pandas jupyter

# Deaktifkan
deactivate
```

Setelah buat venv dan install jupyter di dalamnya, daftarkan sebagai kernel:
```bash
pip install ipykernel
python -m ipykernel install --user --name=nama_env
```

---

## Gotchas

### 1. Package terinstall tapi tidak bisa diimport
Kemungkinan lo install di Python yang berbeda dari yang dipakai kernel. Cek dengan:
```python
import sys
print(sys.executable)
# Harus sama dengan Python di venv lo
```

### 2. `!pip install` berhasil tapi import masih error
Restart kernel setelah install. Jupyter tidak otomatis reload package yang baru diinstall.

### 3. Konflik versi
```bash
pip install "numpy>=1.20,<2.0"  # pin range versi
```

---

➡️ **Next:** [README_04.md](README_04.md) — Magic Commands
