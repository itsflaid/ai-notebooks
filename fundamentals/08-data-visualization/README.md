# 📘 README — 08: Data Visualization

## Tentang Notebook Ini

Visualisasi bukan cuma buat bikin laporan keliatan bagus. Ini adalah skill analitis — lo tidak bisa benar-benar "ngerti" data tanpa melihatnya. Histogram bisa langsung kasih tau distribusi yang butuh 10 menit kalau lo cuma baca angka.

---

## Kapan Pakai Plot Apa

| Tipe Plot | Dipakai Untuk | Contoh |
|-----------|---------------|--------|
| **Line** | Tren waktu, fungsi kontinu | Penjualan bulanan, fungsi sin |
| **Bar** | Perbandingan kategori | Jumlah per kota, penjualan per produk |
| **Histogram** | Distribusi satu variabel | Distribusi nilai, distribusi umur |
| **Scatter** | Hubungan dua variabel | Jam belajar vs nilai, tinggi vs berat |
| **Box Plot** | Distribusi + outlier | IPK per jurusan |
| **Heatmap** | Korelasi, matrix data | Correlation matrix fitur ML |
| **Pie** | Proporsi (pakai sparingly) | Market share |

---

## Anatomi Figure Matplotlib

```
Figure (canvas keseluruhan)
└── Axes (satu plot/panel)
    ├── Title
    ├── X-axis Label
    ├── Y-axis Label
    ├── Ticks & Tick Labels
    ├── Legend
    └── Plot elements (lines, bars, dll)
```

```python
fig, ax = plt.subplots(figsize=(10, 6))
# fig = Figure (seluruh kanvas)
# ax  = Axes (area plot)
# Selalu lebih prefer fig, ax approach daripada plt.xxx()
```

---

## Cheat Sheet Styling

```python
# Warna
ax.plot(x, y, color='#2196F3')    # hex
ax.plot(x, y, color='blue')       # named
ax.plot(x, y, color=(0.1, 0.5, 0.9))  # RGB tuple

# Line style
ax.plot(x, y, linestyle='-')      # solid (default)
ax.plot(x, y, linestyle='--')     # dashed
ax.plot(x, y, linestyle=':')      # dotted
ax.plot(x, y, linestyle='-.')     # dash-dot

# Marker
ax.plot(x, y, marker='o')   # circle
ax.plot(x, y, marker='s')   # square
ax.plot(x, y, marker='^')   # triangle
ax.plot(x, y, marker='*')   # star

# Width & size
ax.plot(x, y, linewidth=2)
ax.scatter(x, y, s=100)     # marker size

# Transparency
ax.plot(x, y, alpha=0.7)
```

---

## Styles yang Tersedia

```python
print(plt.style.available)   # lihat semua style

# Rekomendasi yang bagus:
plt.style.use('seaborn-v0_8-whitegrid')   # clean, profesional
plt.style.use('ggplot')                    # mirip R ggplot
plt.style.use('dark_background')           # dark mode
plt.style.use('fivethirtyeight')           # journalism style
```

---

## Subplot Patterns

```python
# 1x2 (side by side)
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))

# 2x2 grid
fig, axes = plt.subplots(2, 2, figsize=(12, 8))
axes[0, 0].plot(...)  # baris 0, kolom 0
axes[1, 1].plot(...)  # baris 1, kolom 1

# Ukuran berbeda
from matplotlib.gridspec import GridSpec
gs = GridSpec(2, 3)
ax1 = fig.add_subplot(gs[0, :])   # baris pertama, span semua kolom
ax2 = fig.add_subplot(gs[1, 0])   # baris kedua, kolom 0
ax3 = fig.add_subplot(gs[1, 1:])  # baris kedua, kolom 1-2
```

---

## Simpan dengan Kualitas Baik

```python
# PNG untuk web/presentasi
fig.savefig('plot.png', dpi=150, bbox_inches='tight', transparent=False)

# PDF/SVG untuk print/laporan (vector, tidak pixelated)
fig.savefig('plot.pdf', bbox_inches='tight')
fig.savefig('plot.svg', bbox_inches='tight')

# dpi — dots per inch
# 72  = screen resolution (default)
# 150 = presentasi
# 300 = print quality
```

---

## Gotchas

### plt.show() di Jupyter
Dengan `%matplotlib inline`, plot otomatis keliatan. `plt.show()` tetap bagus dipakai untuk explicitly "commit" satu figure sebelum mulai figure baru.

### Figure tidak muncul
```python
# Pastikan cell ini sudah dirun sebelum plotting
%matplotlib inline
```

### Axis yang tidak kelihatan full
```python
plt.tight_layout()  # otomatis adjust spacing
# atau
plt.subplots_adjust(left=0.1, right=0.9, top=0.9, bottom=0.1)
```

---

➡️ **Next:** [README_09.md](README_09.md) — Widgets & Interactivity
