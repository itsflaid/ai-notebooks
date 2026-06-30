# 📘 README — 11: Best Practices

## Tentang Notebook Ini

Notebook terakhir. Ini tentang perbedaan antara notebook yang "jalan di komputer gue" vs notebook yang bisa dibuka siapapun, kapanpun, dan langsung jalan. Bedanya satu kebiasaan kecil yang konsisten dilakukan setiap notebook.

---

## Reproducibility — Kenapa Penting?

Bayangkan lo build model ML, dapat accuracy 95%, lalu 3 bulan kemudian lo coba run ulang dan hasilnya 87%. Tidak ada yang berubah... atau ada?

Reproducibility problems bisa datang dari:
- Random seed tidak di-set → data shuffled beda
- Versi library berubah → behavior berubah
- Data source berubah → distribusi data beda
- Order eksekusi cell berbeda → variabel punya nilai berbeda

---

## The Most Dangerous Jupyter Anti-Pattern

```python
# Cell A (dijalankan ke-3)
x = 10

# Cell B (dijalankan ke-1)
y = x + 5   # ERROR saat dijalankan fresh, tapi "jalan" di session lo

# Cell C (dijalankan ke-2)
x = 999
```

Kalau lo jalankan notebook ini dalam urutan B → C → A → B:
- Di session lo: Cell B pertama error, tapi setelah semua jalan, B menggunakan `x = 10`
- Di orang lain (fresh): Cell B langsung error karena `x` belum ada

**Solusi:** Selalu `Kernel → Restart & Run All` sebelum anggap notebook lo selesai.

---

## Naming Conventions

```python
# Constants — UPPER_SNAKE_CASE
RANDOM_SEED = 42
DATA_DIR = Path('data')
MAX_EPOCHS = 100

# Variables dan functions — lower_snake_case
train_data = load_data()
model_accuracy = evaluate_model(model)

def proses_data(df, kolom):
    ...

# Classes — PascalCase
class DataProcessor:
    ...
```

---

## Struktur Direktori Project

```
project/
├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_preprocessing.ipynb
│   └── 03_modeling.ipynb
├── data/
│   ├── raw/           ← data asli, jangan diubah
│   ├── processed/     ← hasil preprocessing
│   └── external/      ← data dari sumber luar
├── models/            ← saved models
├── output/            ← gambar, laporan
├── src/               ← Python modules (.py files)
│   ├── utils.py
│   └── preprocessing.py
├── requirements.txt
└── README.md
```

Pisahkan kode reusable ke file `.py` di `src/`, import dari notebook.

---

## Notebook vs Script

| Gunakan Notebook | Gunakan Script (.py) |
|-----------------|---------------------|
| EDA dan eksplorasi | Production code |
| Visualisasi | Reusable functions |
| Presentasi hasil | CI/CD pipeline |
| Belajar dan eksperimen | Unit tests |
| Laporan interaktif | Scheduled jobs |

Notebooks bukan untuk production. Kalau lo butuh code dijalankan otomatis atau dijadikan API, pindahkan ke `.py`.

---

## Tools yang Berguna

### nbstripout — Auto-clear output saat git commit
```bash
pip install nbstripout
nbstripout --install  # install sebagai git filter
```
Setelah ini, setiap `git commit` akan otomatis clear output dari file `.ipynb`.

### nbformat — Validasi notebook
```python
import nbformat
with open('notebook.ipynb') as f:
    nb = nbformat.read(f, as_version=4)
nbformat.validate(nb)  # raise error kalau format tidak valid
```

### jupytext — Sinkronisasi notebook dan script
```bash
pip install jupytext
jupytext --to py:percent notebook.ipynb  # convert ke .py
```

### watermark — Record environment info
```python
%pip install watermark
%load_ext watermark
%watermark -v -p numpy,pandas,sklearn,tensorflow
```

---

## Checklist Cepat (Laminating-worthy)

```
SEBELUM MULAI:
☐ Set random seed
☐ Import semua di atas
☐ Define constants

SELAMA CODING:
☐ Satu cell = satu ide
☐ Komen "kenapa", bukan "apa"
☐ Function kompleks ada docstring
☐ Cek output setiap step

SEBELUM SHARE/PUSH:
☐ Restart & Run All — pastikan jalan clean
☐ Verifikasi output masuk akal
☐ Catat versi library
☐ Clear output sensitif
☐ Plot ada label dan judul
```

---

## 🎓 Selesai!

Lo sudah cover seluruh fundamental Jupyter Notebook. Dari sini, setiap waktu yang lo investasikan di proyek nyata akan 10x lebih produktif karena lo tau cara kerja tool-nya.

**Next:** folder `machine-learning/` untuk mulai build model AI pertama lo.

---

*Dibuat dengan ❤️ untuk semua yang frustrasi sama `fix_final_v3_bener.ipynb`*
