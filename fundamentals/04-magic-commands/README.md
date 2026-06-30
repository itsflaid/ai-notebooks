# 📘 README — 04: Magic Commands

## Tentang Notebook Ini

Magic commands adalah fitur yang bikin Jupyter beda dari sekedar "Python di browser". Ini bukan Python — ini ekstensi IPython yang cuma jalan di Jupyter environment.

---

## Line Magic vs Cell Magic

```
% → Line Magic   → berlaku untuk SATU BARIS
%% → Cell Magic  → berlaku untuk SELURUH CELL
```

Contoh perbedaannya:

```python
# Line magic — timing satu ekspresi
%timeit sum(range(1000))

# Cell magic — timing seluruh cell
%%timeit
x = list(range(1000))
result = sum(x)
```

---

## Cheat Sheet Magic Commands

### Timing & Performance

| Command | Kegunaan |
|---------|----------|
| `%time` | Ukur waktu satu eksekusi |
| `%timeit` | Ukur waktu rata-rata dari banyak eksekusi |
| `%%time` | Timing seluruh cell, satu run |
| `%%timeit` | Timing seluruh cell, banyak run |

Kapan pakai `%time` vs `%timeit`?
- `%time` → cepat, tapi hasilnya bisa bervariasi karena noise sistem
- `%timeit` → lebih akurat karena run berkali-kali dan ambil rata-rata. Pakai ini untuk benchmark serius.

### Inspeksi

| Command | Kegunaan |
|---------|----------|
| `%who` | List nama variabel yang ada |
| `%whos` | Detail variabel: tipe, size, value |
| `%who int` | Filter hanya variabel bertipe int |
| `%reset` | Hapus semua variabel |
| `%reset -f` | Hapus semua tanpa konfirmasi |

### Filesystem

| Command | Kegunaan |
|---------|----------|
| `%pwd` | Print working directory |
| `%ls` | List files |
| `%cd path` | Pindah direktori |
| `%mkdir dir` | Buat direktori |
| `%cp src dst` | Copy file |
| `%mv src dst` | Move/rename file |
| `%rm file` | Hapus file |

### File & Code

| Command | Kegunaan |
|---------|----------|
| `%run file.py` | Jalankan file Python |
| `%load file.py` | Load kode dari file ke cell |
| `%save file 1-5` | Simpan cell ke file |
| `%history` | Lihat history command |
| `%history -l 10` | 10 command terakhir |

### Display & Config

| Command | Kegunaan |
|---------|----------|
| `%matplotlib inline` | Plot tampil di dalam notebook |
| `%matplotlib notebook` | Plot interaktif |
| `%config ...` | Konfigurasi IPython |
| `%env VAR` | Lihat env variable |
| `%env VAR=value` | Set env variable |

### Cell Magic

| Command | Kegunaan |
|---------|----------|
| `%%bash` | Jalankan cell sebagai bash script |
| `%%html` | Render cell sebagai HTML |
| `%%latex` | Render cell sebagai LaTeX |
| `%%writefile file` | Tulis isi cell ke file |
| `%%capture` | Capture output cell ke variabel |
| `%%time` | Timing seluruh cell |
| `%%timeit` | Benchmark seluruh cell |

---

## Tips & Trik

### Capture output untuk diproses lebih lanjut

```python
%%capture output
print("ini tidak akan keliatan langsung")
x = 42

# Setelah cell dirun, akses output-nya:
print(output.stdout)
```

### Gunakan `!` untuk satu baris shell command

```python
# ! = satu command shell
!ls -la
!python --version
!pip show numpy

# Simpan output shell ke variabel Python
files = !ls *.py
print(files)  # list of strings
```

Bedanya `!` vs `%%bash`:
- `!` → untuk satu baris command
- `%%bash` → untuk multi-baris script bash

### %autoreload — workflow yang lebih smooth

```python
%load_ext autoreload
%autoreload 2

# Sekarang kalau lo edit file utils.py di luar notebook,
# perubahan otomatis ke-load tanpa restart kernel
import utils
```

Ini sangat berguna saat lo develop modul Python sambil testing di notebook.

---

## Yang Tidak Bisa Dilakukan Magic Commands

- Tidak bisa dipakai di dalam function atau class
- Tidak bisa di-assign ke variabel secara langsung
- Tidak bisa dikombinasikan dalam satu ekspresi

---

## Cek Magic Commands Kustom

Jupyter juga support magic commands dari extension. Beberapa yang populer:
- `%sql` — dari library `ipython-sql`, bisa query SQL langsung
- `%tensorboard` — dari TensorBoard
- `%watermark` — tampilkan info library versions

---

➡️ **Next:** [README_05.md](README_05.md) — File Operations
