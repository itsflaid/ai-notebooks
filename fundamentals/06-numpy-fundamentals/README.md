# 📘 README — 06: NumPy Fundamentals

## Tentang Notebook Ini

NumPy bukan sekadar "list yang lebih cepat". Ini adalah fondasi matematika seluruh ekosistem Python data science. Kalau lo ngerti NumPy dengan baik, lo bakal ngerti kenapa TensorFlow dan PyTorch bekerja seperti itu.

---

## Kenapa NumPy Jauh Lebih Cepat dari Python List?

Python list menyimpan referensi ke objek Python arbitrary — setiap elemen bisa berbeda tipe, dan Python harus cek tipe setiap kali operasi dijalankan.

NumPy array menyimpan data homogen (tipe sama) dalam blok memori yang contiguous — tidak ada overhead per-elemen, dan operasi bisa di-vectorize di level C.

```
Python list:  [ptr→obj1, ptr→obj2, ptr→obj3, ...]
               ↓           ↓           ↓
NumPy array:  [10, 20, 30, 40, ...]  ← raw memory, contiguous
```

Hasilnya? NumPy bisa 10x–100x lebih cepat untuk operasi numerik.

---

## Konsep Shape yang Sering Bikin Bingung

```python
# 1D — vektor
a = np.array([1, 2, 3])
# shape: (3,)    ← tuple dengan satu angka

# 2D — matrix
b = np.array([[1,2,3],[4,5,6]])
# shape: (2, 3)  ← 2 baris, 3 kolom

# 3D — tensor (umum di deep learning)
c = np.zeros((4, 28, 28))
# shape: (4, 28, 28) ← 4 gambar, 28×28 pixel
```

Di deep learning, shape biasanya `(batch_size, height, width, channels)` atau `(batch_size, sequence_length, features)`. Paham shape = paham kenapa model lo error.

---

## Perbedaan View vs Copy

```python
a = np.array([1, 2, 3, 4, 5])

# VIEW — berbagi memori dengan original
b = a[1:4]    # slice → view
b[0] = 999
print(a)      # [1, 999, 3, 4, 5] ← a IKUT BERUBAH!

# COPY — data terpisah
c = a[1:4].copy()
c[0] = 0
print(a)      # tidak berubah
```

Ini penting banget buat avoid bug yang susah dicari. Kalau ragu, pakai `.copy()`.

---

## Broadcasting Rules

NumPy bisa operate array beda shape kalau mengikuti rules:

1. Array dengan dimensi lebih sedikit ditambah dimensi 1 di kiri
2. Dimensi dengan size 1 di-"stretch" untuk match

```
Array A: (3, 3)
Array B: (3,)   → broadcast jadi (1, 3) → (3, 3)

Array A: (3, 1)
Array B: (1, 4) → hasil: (3, 4)
```

Contoh praktis — normalisasi kolom:
```python
matrix = np.random.randn(100, 5)  # 100 data, 5 fitur
mean = matrix.mean(axis=0)        # shape (5,)
std = matrix.std(axis=0)          # shape (5,)

# Broadcasting: (100,5) - (5,) / (5,) → bekerja!
normalized = (matrix - mean) / std
```

---

## Pattern Umum di ML

### One-hot encoding manual
```python
labels = np.array([0, 2, 1, 0, 2])  # kelas 0, 1, 2
n_classes = 3
one_hot = np.eye(n_classes)[labels]
# [[1,0,0], [0,0,1], [0,1,0], [1,0,0], [0,0,1]]
```

### Train-test split manual
```python
data = np.random.randn(1000, 10)
labels = np.random.randint(0, 2, 1000)

# Shuffle
idx = np.random.permutation(len(data))
split = int(0.8 * len(data))

train_idx, test_idx = idx[:split], idx[split:]
X_train, X_test = data[train_idx], data[test_idx]
y_train, y_test = labels[train_idx], labels[test_idx]
```

### Normalisasi data
```python
# Min-max normalization ke [0, 1]
x_min = data.min(axis=0)
x_max = data.max(axis=0)
normalized = (data - x_min) / (x_max - x_min)

# Z-score normalization
mean = data.mean(axis=0)
std = data.std(axis=0)
standardized = (data - mean) / std
```

---

## Gotchas

### 1. Integer division
```python
np.array([1, 2, 3]) / 2    # [0.5, 1.0, 1.5] — float
np.array([1, 2, 3]) // 2   # [0, 1, 1] — integer
```

### 2. Perbandingan float
```python
0.1 + 0.2 == 0.3     # False! (floating point precision)
np.isclose(0.1 + 0.2, 0.3)  # True — pakai ini
```

### 3. Shape (n,) beda dari (n, 1)
```python
a = np.array([1, 2, 3])      # shape (3,)
b = np.array([[1], [2], [3]])  # shape (3, 1)

# Operasi bisa jalan tapi hasilnya beda!
# Selalu explicit tentang shape yang lo mau
```

---

➡️ **Next:** [README_07.md](README_07.md) — Pandas Fundamentals
