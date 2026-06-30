# 📘 README — 01: Introduction to Jupyter Notebook

## Tentang Notebook Ini

Notebook pertama ini adalah orientasi — lo kenalan sama environment Jupyter sebelum nulis kode serius. Kalau lo skip ini dan langsung lompat ke NumPy, lo bakal sering bingung sama hal-hal basic yang seharusnya udah jelas.

---

## Konsep Kunci

### Jupyter vs Script Python Biasa

Script `.py` dijalankan dari atas ke bawah sekaligus. Jupyter berbeda — lo bisa run **per cell**, lihat output-nya, lalu lanjut. Ini yang bikin Jupyter cocok banget buat eksplorasi data dan eksperimen model.

```
Script .py          Jupyter Notebook
─────────────       ────────────────
run semua →         run cell 1 → lihat output
tunggu selesai      run cell 2 → lihat output
lihat output        run cell 3 → lihat output
                    (bisa run ulang cell manapun)
```

### State dan Urutan Eksekusi

Ini bagian yang sering bikin confused di awal:

```python
# Cell A
x = 10

# Cell B  
x = 20

# Cell C
print(x)  # Output: berapa?
```

Jawabannya tergantung **urutan lo run cell-nya**, bukan urutan posisi cell di notebook. Kalau lo run C dulu sebelum A dan B, bakal error karena `x` belum ada.

> **Best practice:** Selalu bisa di-run dari atas ke bawah secara berurutan. Gunakan `Kernel → Restart & Run All` untuk verifikasi.

### Kernel State

Kernel itu seperti sesi Python yang terus jalan. Semua variabel yang pernah lo define tersimpan di sana sampai:
- Lo restart kernel
- Kernel crash
- Lo tutup server Jupyter

---

## Shortcuts yang Paling Sering Dipakai

Dari sekian banyak shortcut, 5 ini yang paling sering dipakai 90% waktu:

| Shortcut | Aksi | Kapan Dipakai |
|----------|------|---------------|
| `Shift+Enter` | Run & next | Lagi ngoding normal |
| `Ctrl+Enter` | Run di tempat | Lagi coba-coba satu cell |
| `Esc → B` | Cell baru di bawah | Mau nambahin kode |
| `Esc → D,D` | Hapus cell | Bersihin cell yang nggak perlu |
| `Esc → M` | Ganti ke Markdown | Mau nulis penjelasan |

---

## Platform Jupyter yang Umum Dipakai

| Platform | Keterangan | Cocok untuk |
|----------|------------|-------------|
| **Jupyter Notebook** | Classic, ringan | Belajar, projek sederhana |
| **JupyterLab** | Modern, multi-panel | Projek kompleks, power user |
| **Google Colab** | Cloud, gratis GPU | Eksperimen ML, berbagi notebook |
| **VS Code + Jupyter Extension** | Terintegrasi IDE | Developer yang kerja di VS Code |
| **Kaggle Notebooks** | Cloud, built-in dataset | Kompetisi ML, eksplorasi dataset |

---

## Gotchas & Hal yang Sering Bikin Bingung

### 1. Output Lama Masih Keliatan
Jupyter menyimpan output dari run sebelumnya di file `.ipynb`. Jadi meski lo belum run ulang, output lama masih keliatan. Run cell lagi untuk update.

### 2. Angka di `[1]`, `[2]`, dst
Angka itu adalah **execution counter** — urutan kapan cell itu terakhir dirun. Kalau lo run cell yang sama 3 kali, angkanya akan terus naik. Bukan berarti cell itu run 3 kali sekarang.

### 3. `[*]` = Lagi Running
Kalau lo lihat `[*]` di sebelah cell, artinya cell itu lagi dieksekusi. Kalau kelamaan, bisa jadi infinite loop atau komputasi berat. Interrupt dengan `I, I` (command mode) atau `Kernel → Interrupt`.

### 4. Kernel Mati = Variabel Hilang
Kalau Jupyter-nya crash atau lo restart, semua variabel di memory hilang. Lo harus run ulang dari awal. Makanya selalu save kode lo di cell, bukan cuma di memory.

---

## Format File .ipynb

File `.ipynb` sebenernya cuma JSON biasa:

```json
{
  "cells": [
    {
      "cell_type": "code",
      "source": ["print('hello')"],
      "outputs": [{"text": "hello\n"}]
    }
  ],
  "metadata": {
    "kernelspec": {"name": "python3"}
  }
}
```

Makanya file `.ipynb` bisa di-track di git, dibuka di GitHub (auto-render), dan di-convert ke format lain.

---

## Resource Tambahan

- [Jupyter Documentation](https://jupyter-notebook.readthedocs.io/)
- [JupyterLab Documentation](https://jupyterlab.readthedocs.io/)
- [Google Colab FAQ](https://research.google.com/colaboratory/faq.html)

---

➡️ **Next:** [README_02.md](README_02.md) — Python Basics
