# 📘 README — 07: Pandas Fundamentals

## Tentang Notebook Ini

Pandas adalah Swiss Army Knife-nya data analyst dan ML engineer. Hampir tidak ada project data yang tidak pakai ini. Kalau lo bisa Pandas dengan baik, tahap data preparation yang biasanya makan 80% waktu proyek ML bisa jauh lebih smooth.

---

## DataFrame vs Series

```
Series  = satu kolom dengan index
           index   value
           0       85
           1       92
           2       78

DataFrame = tabel dengan banyak kolom
           nama    umur    nilai
           0  Budi  25      85
           1  Ani   23      92
           2  Ciko  27      78
```

DataFrame terdiri dari beberapa Series yang berbagi index yang sama.

---

## loc vs iloc — Kapan Pakai Mana?

```python
# loc — gunakan LABEL
df.loc[0]              # baris dengan label/index 0
df.loc['Budi']         # baris dengan label 'Budi' (kalau index string)
df.loc[0, 'nama']      # baris 0, kolom 'nama'
df.loc[0:3, 'nama':'nilai']  # INCLUSIVE di kedua sisi

# iloc — gunakan INTEGER POSITION
df.iloc[0]             # baris pertama (posisi 0)
df.iloc[-1]            # baris terakhir
df.iloc[0, 1]          # baris 0, kolom 1 (posisi)
df.iloc[0:3, 0:2]      # EXCLUSIVE di akhir (seperti Python slice)
```

**Gotcha:** `loc[0:3]` = baris 0, 1, 2, **3** (inclusive)
`iloc[0:3]` = baris 0, 1, **2** (exclusive)

---

## Method Chaining — Pattern yang Clean

```python
hasil = (
    df
    .dropna(subset=['nilai'])
    .query('umur > 22')
    .assign(grade=lambda x: pd.cut(x['nilai'], bins=[0, 70, 80, 90, 100],
                                    labels=['C', 'B', 'A', 'A+']))
    .groupby('grade')['nilai']
    .agg(['count', 'mean'])
    .round(2)
)
```

Method chaining membuat kode lebih readable dan mudah di-debug per step.

---

## apply vs vectorized operations

```python
# ❌ LAMBAT — apply per baris
df['nilai_baru'] = df['nilai'].apply(lambda x: x * 2)

# ✅ CEPAT — vectorized
df['nilai_baru'] = df['nilai'] * 2

# apply tetap diperlukan untuk logika kompleks
df['kategori'] = df['nilai'].apply(
    lambda x: 'Tinggi' if x > 85 else 'Rendah'
)

# Atau pakai np.where (lebih cepat dari apply)
df['kategori'] = np.where(df['nilai'] > 85, 'Tinggi', 'Rendah')

# pd.cut untuk binning
df['grade'] = pd.cut(df['nilai'], 
                      bins=[0, 60, 70, 80, 90, 100],
                      labels=['E', 'D', 'C', 'B', 'A'])
```

---

## Missing Values — Strategy

| Strategy | Kapan Dipakai |
|----------|---------------|
| `dropna()` | Data banyak, baris yang missing sedikit |
| `fillna(mean)` | Numerik, distribusi normal |
| `fillna(median)` | Numerik, ada outlier |
| `fillna(mode)` | Kategorik |
| `fillna(method='ffill')` | Time series — isi dengan nilai sebelumnya |
| `fillna(method='bfill')` | Time series — isi dengan nilai berikutnya |
| Imputer dari sklearn | Butuh yang lebih sophisticated |

---

## Merge Types

```
INNER:  hanya yang ada di KEDUA tabel
LEFT:   semua dari kiri, match dari kanan (NaN kalau tidak ada)
RIGHT:  semua dari kanan, match dari kiri
OUTER:  semua dari keduanya, NaN di mana tidak match
```

---

## Tips Performa

```python
# Gunakan kategori untuk kolom string repetitif
df['kota'] = df['kota'].astype('category')  # hemat memori

# Baca CSV dengan dtype yang benar dari awal
df = pd.read_csv('data.csv', dtype={'id': 'int32', 'nilai': 'float32'})

# query() lebih cepat dari boolean indexing untuk DataFrame besar
df.query('umur > 25 and nilai > 80')  # lebih cepat dari df[(df.umur>25) & ...]
```

---

➡️ **Next:** [README_08.md](README_08.md) — Data Visualization
