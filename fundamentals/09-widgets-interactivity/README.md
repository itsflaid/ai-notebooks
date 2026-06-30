# 📘 README — 09: Widgets & Interactivity

## Tentang Notebook Ini

ipywidgets bikin notebook lo jadi lebih dari sekedar kode — bisa jadi tool interaktif yang bisa dipakai orang yang bahkan tidak ngerti Python. Berguna banget untuk eksplorasi parameter model, presentasi hasil analisis, atau bikin dashboard sederhana.

---

## Cara Kerja Widgets

```
User geser slider
      ↓
Widget value berubah
      ↓
Callback function dipanggil
      ↓
Output di-update
```

---

## Widget Types Cheat Sheet

### Input Widgets

| Widget | Tipe Value | Contoh Penggunaan |
|--------|-----------|-------------------|
| `IntSlider` | int | Learning rate steps, epoch count |
| `FloatSlider` | float | Learning rate, threshold |
| `IntRangeSlider` | (int, int) | Range umur, range harga |
| `Text` | str | Input nama, path file |
| `Textarea` | str | Multi-line input |
| `Dropdown` | any | Pilih model, pilih dataset |
| `RadioButtons` | any | Pilih satu dari beberapa opsi |
| `Checkbox` | bool | Toggle fitur on/off |
| `SelectMultiple` | tuple | Filter kategori |
| `ColorPicker` | str (hex) | Pilih warna plot |
| `DatePicker` | date | Filter rentang tanggal |

### Output Widgets

| Widget | Kegunaan |
|--------|----------|
| `Output` | Container untuk output (print, plot) |
| `HTML` | Render HTML |
| `Image` | Tampilkan gambar |
| `Label` | Teks non-interaktif |
| `IntProgress` | Progress bar |

### Layout Widgets

| Widget | Layout |
|--------|--------|
| `HBox` | Horizontal (kiri ke kanan) |
| `VBox` | Vertical (atas ke bawah) |
| `GridBox` | Grid layout |
| `Tab` | Tab panel |
| `Accordion` | Collapsible panel |

---

## Pattern: interact vs observe

```python
# INTERACT — cara paling simple, cocok untuk prototyping
@interact(x=(0, 10), y=(0, 10))
def tambah(x=5, y=5):
    print(x + y)

# OBSERVE — lebih kontrol, untuk UI kompleks
slider = widgets.IntSlider(value=5)

def on_change(change):
    print(f"Nilai baru: {change['new']}")

slider.observe(on_change, names='value')
display(slider)
```

Pakai `interact` kalau:
- Prototipe cepat
- UI sederhana, tidak perlu kontrol penuh

Pakai `observe` kalau:
- Perlu trigger dari beberapa widget sekaligus
- Perlu kontrol kapan output di-update
- UI kompleks dengan layout kustom

---

## Output Widget — Penting!

Selalu pakai `Output` widget kalau lo mau kontrol di mana output muncul, terutama dalam callback button:

```python
out = widgets.Output()
btn = widgets.Button(description='Run')

def on_click(b):
    with out:
        clear_output(wait=True)  # wait=True = tidak flicker
        # semua print/plot di sini masuk ke out
        plt.figure()
        plt.plot([1,2,3])
        plt.show()

btn.on_click(on_click)
display(btn, out)
```

Tanpa `Output` widget, output dari callback bisa muncul di tempat yang random.

---

## Gotchas

### 1. Widget tidak muncul di Jupyter Classic
Mungkin perlu enable extension:
```bash
jupyter nbextension enable --py widgetsnbextension
```

### 2. Widget tidak muncul di JupyterLab
```bash
pip install jupyterlab_widgets
```

### 3. clear_output tanpa wait=True bikin flicker
```python
# ❌ Flicker — hapus dulu, render baru
clear_output()
plt.show()

# ✅ Smooth — render dulu, hapus lama setelah selesai
clear_output(wait=True)
plt.show()
```

### 4. interact membuat widget baru setiap dipanggil
Kalau mau widget persist, define di luar function dan pakai `observe`.

---

## Alternatif untuk Dashboard yang Lebih Serius

Kalau lo butuh yang lebih powerful dari ipywidgets:

- **Streamlit** — deploy ke web dengan mudah, tidak perlu Jupyter
- **Gradio** — khusus untuk demo ML model
- **Panel** — dashboard yang bisa deploy ke web
- **Dash (Plotly)** — interactive dashboard production-grade

---

➡️ **Next:** [README_10.md](README_10.md) — Debugging & Profiling
