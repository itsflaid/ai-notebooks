# 📘 README — 02: Python Basics

## Tentang Notebook Ini

Notebook ini cover Python yang paling sering dipakai di dunia data science dan ML. Bukan tutorial Python lengkap — fokusnya ke hal-hal yang bakal lo pakai terus di notebook-notebook berikutnya.

---

## Kenapa Python untuk Data Science?

- **Sintaks bersih** — kode mudah dibaca, mirip pseudocode
- **Ekosistem library** — NumPy, Pandas, Matplotlib, Scikit-learn, TensorFlow semua Python
- **Interaktif** — cocok banget sama Jupyter
- **Komunitas besar** — hampir semua masalah sudah ada solusinya di Stack Overflow

---

## Cheat Sheet Cepat

### Tipe Data

| Tipe | Contoh | Mutable? |
|------|--------|----------|
| `int` | `42` | ❌ |
| `float` | `3.14` | ❌ |
| `str` | `"hello"` | ❌ |
| `bool` | `True/False` | ❌ |
| `list` | `[1, 2, 3]` | ✅ |
| `tuple` | `(1, 2, 3)` | ❌ |
| `dict` | `{"a": 1}` | ✅ |
| `set` | `{1, 2, 3}` | ✅ |

### Kapan Pakai Apa?

- **List** → koleksi data yang urutannya penting dan bisa berubah
- **Tuple** → koordinat, RGB, data yang tidak boleh diubah
- **Dict** → data terstruktur dengan key yang meaningful (mirip JSON)
- **Set** → cek keunikan, operasi himpunan, hapus duplikat

---

## Hal yang Sering Bikin Bingung

### 1. Mutable vs Immutable

```python
# List — mutable (berubah di tempat)
a = [1, 2, 3]
b = a           # b BUKAN copy, b adalah reference yang sama!
b.append(4)
print(a)        # [1, 2, 3, 4] — a ikut berubah!

# Cara yang benar kalau mau copy:
b = a.copy()    # atau: b = a[:]
```

Ini bug klasik yang sering nyebelin. Di NumPy dan Pandas perilakunya mirip — perlu `.copy()` kalau mau data independen.

### 2. Indeks Negatif

```python
data = [10, 20, 30, 40, 50]
print(data[-1])   # 50 (terakhir)
print(data[-2])   # 40 (kedua dari terakhir)
print(data[-3:])  # [30, 40, 50]
```

### 3. None vs 0 vs False vs ""

Semua ini "falsy" di Python:
```python
if not None:   print("None is falsy")
if not 0:      print("0 is falsy")
if not False:  print("False is falsy")
if not "":     print("empty string is falsy")
if not []:     print("empty list is falsy")
```

Penting banget saat cek apakah data ada atau kosong.

### 4. Integer Division

```python
10 / 3    # 3.333...  (float division)
10 // 3   # 3         (integer division, buang desimal)
10 % 3    # 1         (modulo / sisa bagi)
```

---

## Pattern yang Sering Dipakai di ML

### Flatten nested list
```python
nested = [[1, 2], [3, 4], [5, 6]]
flat = [x for sublist in nested for x in sublist]
# [1, 2, 3, 4, 5, 6]
```

### Filter data dengan kondisi
```python
nilai = [45, 78, 92, 55, 88, 34, 95]
lulus = [n for n in nilai if n >= 60]
# [78, 92, 88, 95]
```

### Zip dua list
```python
nama = ["Budi", "Ani", "Ciko"]
nilai = [85, 92, 78]
data = list(zip(nama, nilai))
# [('Budi', 85), ('Ani', 92), ('Ciko', 78)]
```

### Sort dict by value
```python
skor = {"Budi": 85, "Ani": 92, "Ciko": 78}
terurut = sorted(skor.items(), key=lambda x: x[1], reverse=True)
# [('Ani', 92), ('Budi', 85), ('Ciko', 78)]
```

---

## Tentang Function di Jupyter

Di Jupyter, function yang lo define di satu cell bisa dipakai di cell lain — selama cell yang define-nya sudah dirun dulu.

```python
# Cell 1 — define function
def proses_data(data):
    return [x * 2 for x in data]

# Cell 2 — pakai function (harus run cell 1 dulu!)
hasil = proses_data([1, 2, 3])
```

---

➡️ **Next:** [README_03.md](README_03.md) — Imports & Packages
