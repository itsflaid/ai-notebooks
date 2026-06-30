# 📘 README — 10: Debugging & Profiling

## Tentang Notebook Ini

Debugging adalah skill yang jarang diajarkan tapi habis 60% waktu programmer. Belajar debug dengan efisien = belajar nyelesaikan masalah lebih cepat. Profiling adalah cara lo tau *di mana* kode lo lambat sebelum lo coba optimasi.

---

## Mental Model: Cara Debug yang Efisien

```
Error muncul
     ↓
1. Baca traceback dari BAWAH ke atas
     ↓
2. Temukan baris yang error
     ↓
3. Cek tipe dan value variabel di sana
     ↓
4. Isolasi: apakah input yang salah atau logikanya?
     ↓
5. Fix dan verifikasi
```

Jangan panik dan langsung Google sebelum baca traceback-nya dulu. 80% error jelas dari pesan errornya.

---

## Baca Traceback

```
Traceback (most recent call last):
  File "notebook.py", line 15, in <module>     ← titik awal
    hasil = load_dan_proses('data.csv')
  File "notebook.py", line 10, in load_dan_proses   ← call berikutnya
    return proses_data(data)
  File "notebook.py", line 5, in proses_data    ← di mana error terjadi
    return [item['nilai'] for item in data]
KeyError: 'nilai'                               ← ini yang lo baca PERTAMA
```

Baca dari bawah:
1. `KeyError: 'nilai'` → ada dict yang tidak punya key `'nilai'`
2. `File "...", line 5, in proses_data` → ini di mana tepatnya
3. Naik ke atas → siapa yang panggil `proses_data`

---

## Debug Toolkit

| Tool | Kapan Pakai |
|------|-------------|
| `print()` | Cara paling simple, selalu works |
| `assert` | Validasi asumsi, bisa di-disable production |
| `%debug` | Setelah error terjadi, buka interactive pdb |
| `%pdb on` | Auto-pdb setiap error |
| `breakpoint()` | Set breakpoint di kode Python 3.7+ |
| `logging` | Debug yang proper, bisa di-control level-nya |

---

## Profiling: Rule #1

> **"Premature optimization is the root of all evil"** — Donald Knuth

Jangan optimasi sebelum lo tahu mana yang lambat. Otak lo tidak bisa prediksi bottleneck dengan benar. Selalu profile dulu, baru optimasi di bagian yang terbukti lambat.

```
Kode jalan → Profile → Temukan bottleneck → Optimasi → Profile lagi
```

---

## Interpretasi %prun Output

```
ncalls  tottime  percall  cumtime  percall  filename:lineno(function)
   1    0.000    0.000    1.234    1.234    notebook.py:1(<module>)
   1    0.500    0.500    1.200    1.200    notebook.py:5(fungsi_berat)
1000    0.700    0.001    0.700    0.001    notebook.py:10(sub_fungsi)
```

- `ncalls` → berapa kali dipanggil
- `tottime` → total waktu di fungsi ini (exclude sub-calls)
- `cumtime` → total waktu termasuk sub-calls — **ini yang paling penting**
- Sort by `cumtime` untuk cari bottleneck

---

## Optimasi Pattern Umum

```python
# ❌ LAMBAT — Python loop untuk numerik
hasil = []
for x in data:
    hasil.append(x ** 2)

# ✅ CEPAT — NumPy vectorized
hasil = data ** 2  # kalau data adalah numpy array

# ❌ LAMBAT — string concatenation di loop
s = ""
for word in words:
    s += word + " "

# ✅ CEPAT — join
s = " ".join(words)

# ❌ LAMBAT — repeated DataFrame indexing
for i in range(len(df)):
    val = df.iloc[i]['kolom']

# ✅ CEPAT — itertuples atau vectorized
for row in df.itertuples():
    val = row.kolom
# atau lebih baik lagi: df['kolom'].apply(func)
# atau: df['kolom'] * 2 (kalau bisa vectorized)
```

---

## Common Bugs Cheat Sheet

| Bug | Gejala | Fix |
|-----|--------|-----|
| Mutable default arg | Function "ingat" nilai dari call sebelumnya | Pakai `None` sebagai default |
| Copy vs reference | Modifikasi variable A ikut ubah B | Pakai `.copy()` atau `deepcopy()` |
| Float precision | `0.1 + 0.2 != 0.3` | Pakai `math.isclose()` |
| Off-by-one | Loop kurang/lebih 1 iterasi | Cek `range()` dan slice |
| Silent exception | `except:` tanpa nama | Selalu specify exception type |
| Mutate during iteration | Skip elemen di loop | Buat list baru, jangan modify yang sedang di-iterate |

---

➡️ **Next:** [README_11.md](README_11.md) — Best Practices
