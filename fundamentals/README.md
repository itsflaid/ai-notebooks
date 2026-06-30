# 📓 Jupyter Notebook Fundamentals

Koleksi notebook untuk belajar Jupyter Notebook dari nol sampai siap dipakai buat proyek AI/ML sungguhan. Setiap notebook berdiri sendiri tapi direkomendasikan dibaca berurutan.

---

## 🗺️ Learning Path

```
[01] Introduction → [02] Python Basics → [03] Imports & Packages
      ↓
[04] Magic Commands → [05] File Operations → [06] NumPy
      ↓
[07] Pandas → [08] Visualization → [09] Widgets & Interactivity
      ↓
[10] Debugging & Profiling → [11] Best Practices
```

---

## 📚 Daftar Notebook

| No | Notebook | Topik | Level |
|----|----------|-------|-------|
| 01 | [Introduction](01_introduction.ipynb) | Interface, cell types, shortcuts | 🟢 Pemula |
| 02 | [Python Basics](02_python_basics.ipynb) | Variabel, loop, function, data types | 🟢 Pemula |
| 03 | [Imports & Packages](03_imports_packages.ipynb) | Import, alias, pip install | 🟢 Pemula |
| 04 | [Magic Commands](04_magic_commands.ipynb) | %timeit, %%bash, %who, %run | 🟡 Menengah |
| 05 | [File Operations](05_file_operations.ipynb) | Upload, download, baca/tulis file | 🟡 Menengah |
| 06 | [NumPy Fundamentals](06_numpy_fundamentals.ipynb) | Array, indexing, operasi matematika | 🟡 Menengah |
| 07 | [Pandas Fundamentals](07_pandas_fundamentals.ipynb) | DataFrame, CSV, filtering, cleaning | 🟡 Menengah |
| 08 | [Data Visualization](08_data_visualization.ipynb) | Matplotlib, plot types, styling | 🟡 Menengah |
| 09 | [Widgets & Interactivity](09_widgets_interactivity.ipynb) | ipywidgets, slider, interact | 🟠 Lanjutan |
| 10 | [Debugging & Profiling](10_debugging_profiling.ipynb) | %debug, %pdb, memory profiling | 🟠 Lanjutan |
| 11 | [Best Practices](11_best_practices.ipynb) | Struktur, reproducibility, tips | 🟠 Lanjutan |

---

## ⚡ Quick Start

### Jalankan di Jupyter Notebook / JupyterLab
```bash
# Install jupyter dulu kalau belum
pip install notebook

# Jalankan dari folder ini
jupyter notebook
```

### Jalankan di Google Colab
Upload file `.ipynb` ke [colab.research.google.com](https://colab.research.google.com) atau buka langsung dari GitHub.

### Jalankan di VS Code
Install ekstensi **Jupyter** dari Microsoft, lalu buka file `.ipynb` langsung.

---

## 📦 Dependencies

```bash
pip install numpy pandas matplotlib ipywidgets
```

Untuk notebook tertentu mungkin ada tambahan — cek bagian **Requirements** di README masing-masing.

---

## 💡 Tips Belajar

- **Jangan cuma baca** — selalu run setiap cell dan coba modifikasi kodenya
- **Urut itu penting** — notebook 01-05 adalah fondasi, skip dengan risiko sendiri
- **Error itu normal** — notebook 10 ada khusus buat ngajarin cara debug

---

## 📁 Struktur File

```
fundamentals/
├── README_INDEX.md          ← lo lagi baca ini
├── 01_introduction.ipynb
├── README_01.md
├── 02_python_basics.ipynb
├── README_02.md
├── ... (dan seterusnya)
├── 11_best_practices.ipynb
└── README_11.md
```

Setiap notebook punya README pasangannya yang berisi penjelasan konsep lebih dalam, contoh use case, dan gotchas yang sering bikin pusing.
