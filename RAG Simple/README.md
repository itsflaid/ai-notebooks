# Belajar RAG & Jupyter Notebook — Panduan Lengkap

> Ditulis untuk developer yang udah ngerti coding tapi baru masuk ke dunia ML/AI & Jupyter.

---

## Bagian 1 — Jupyter Notebook: Aturan & Cara Kerja

### Apa itu Jupyter Notebook?

Jupyter Notebook adalah lingkungan coding interaktif di mana kamu nulis dan jalanin kode **per blok (cell)**, bukan satu file sekaligus. Output langsung muncul di bawah tiap cell — cocok banget buat eksplorasi data, prototyping, dan edukasi.

File-nya berekstensi `.ipynb` (Interactive Python Notebook), yang sebenernya adalah JSON berisi list cells.

---

### Anatomi Notebook

Notebook terdiri dari dua jenis cell:

| Jenis | Fungsi |
|---|---|
| **Code cell** | Nulis & jalanin kode Python |
| **Markdown cell** | Nulis teks, judul, penjelasan (pakai syntax Markdown) |

Kamu bisa campur keduanya bebas — makanya notebook enak buat dokumentasi sekaligus kode.

---

### Aturan Penting Jupyter Notebook

#### 1. Cell dijalankan secara berurutan (state shared)
Ini paling krusial. Variabel, import, dan fungsi yang lo definisikan di cell sebelumnya **tetap hidup** di memory selama session masih aktif.

```python
# Cell 1
x = 10

# Cell 2 — x masih bisa diakses
print(x)  # ✅ output: 10
```

**Implikasi:** kalau lo jalanin cell secara acak / lompat-lompat, bisa error atau dapat hasil yang salah karena state-nya kekacauan.

**Best practice:** Selalu jalanin dari atas ke bawah (`Runtime > Run all` di Colab). Kalau ada yang aneh, restart kernel dulu.

---

#### 2. Kernel = proses Python yang jalan di belakang
Kernel adalah "mesin" yang menjalankan kode lo. Satu notebook = satu kernel.

- **Restart kernel** = reset semua variabel, import, dll. (kayak restart program)
- **Restart + Run All** = restart lalu jalanin semua cell dari awal

Di Google Colab: `Runtime > Restart session` atau `Runtime > Restart and run all`

---

#### 3. Angka di samping cell = urutan eksekusi
```
[1] → cell ini dijalankan pertama
[2] → cell ini dijalankan kedua
[*] → cell ini sedang berjalan
```

Kalau lo jalanin cell yang sama dua kali, angkanya akan update ke nomor terbaru. Ini petunjuk urutan eksekusi — penting buat debugging.

---

#### 4. Output menempel di cell
Output (print, error, grafik, tabel) langsung muncul di bawah cell yang menghasilkannya dan **tersimpan di file `.ipynb`**. Jadi kalau lo share notebook, orang lain bisa lihat output tanpa jalanin ulang.

---

#### 5. Install library pakai `!` (shell command)
Di dalam notebook, kamu bisa jalanin perintah terminal dengan prefix `!`:

```python
!pip install pandas matplotlib
!ls /content/
```

Di Colab, library yang diinstall akan hilang kalau session berakhir — jadi cell install harus selalu ada di bagian atas notebook.

---

#### 6. Shortcut penting (keyboard)
| Shortcut | Aksi |
|---|---|
| `Shift + Enter` | Jalanin cell & pindah ke bawah |
| `Ctrl + Enter` | Jalanin cell, tetap di cell yang sama |
| `Esc` lalu `A` | Tambah cell baru di atas |
| `Esc` lalu `B` | Tambah cell baru di bawah |
| `Esc` lalu `D D` | Hapus cell |
| `Esc` lalu `M` | Ubah cell jadi Markdown |
| `Esc` lalu `Y` | Ubah cell jadi Code |

---

#### 7. Google Colab vs Jupyter lokal

| | Google Colab | Jupyter Lokal |
|---|---|---|
| Setup | Langsung pakai, gratis | Perlu install Anaconda/pip |
| GPU | Ada (gratis, terbatas) | Tergantung hardware |
| Storage | Google Drive | Lokal |
| Session | Reset tiap ~12 jam idle | Permanen selama laptop nyala |
| Cocok untuk | Belajar, prototyping, kolaborasi | Project yang butuh persistensi |

---

## Bagian 2 — RAG: Konsep dari Dasar

### Masalah yang RAG selesaikan

LLM (seperti GPT, Llama, Claude) punya dua batasan besar:

1. **Knowledge cutoff** — dilatih sampai tanggal tertentu, tidak tahu info terbaru
2. **Tidak tahu dokumen privat lo** — tidak tahu isi laporan, PDF, codebase, dll.

RAG (Retrieval-Augmented Generation) adalah teknik untuk **menghubungkan LLM dengan dokumen eksternal secara real-time**, tanpa perlu melatih ulang model.

---

### Analogi Sederhana

Bayangkan kamu ujian open book:

- **LLM tanpa RAG** = ujian closed book, jawab dari hafalan saja
- **LLM dengan RAG** = ujian open book, boleh buka buku, lalu tulis jawaban berdasarkan yang kamu baca

RAG memberi LLM akses ke "buku" yang relevan sebelum menjawab.

---

### Arsitektur RAG: Dua Fase

#### Fase 1 — Indexing (dilakukan sekali di awal)

```
Dokumen PDF/TXT
      ↓
  Text Splitting (potong jadi chunks kecil)
      ↓
  Embedding (ubah teks jadi vector angka)
      ↓
  Vector Store (simpan semua vector)
```

#### Fase 2 — Retrieval + Generation (dilakukan tiap query)

```
Query User ("Apa itu X?")
      ↓
  Embed query (ubah query jadi vector)
      ↓
  Similarity Search (cari chunks paling mirip di vector store)
      ↓
  Ambil top-K chunks paling relevan
      ↓
  Gabungkan chunks + query → kirim ke LLM
      ↓
  LLM hasilkan jawaban berdasarkan konteks
```

---

### Komponen RAG & Fungsinya

#### 1. Document Loader
Membaca file (PDF, TXT, CSV, web, dll) dan mengubahnya jadi objek `Document` yang punya `page_content` dan `metadata`.

```python
from langchain_community.document_loaders import PyPDFLoader

loader = PyPDFLoader("dokumen.pdf")
pages = loader.load()
# pages[0].page_content → isi teks halaman pertama
# pages[0].metadata → {'source': 'dokumen.pdf', 'page': 0}
```

---

#### 2. Text Splitter
Memotong dokumen panjang jadi **chunks** (potongan kecil). Kenapa perlu dipotong?

- LLM punya batas token (tidak bisa proses dokumen 100 halaman sekaligus)
- Pencarian lebih presisi kalau chunks kecil dan spesifik

```python
from langchain.text_splitter import RecursiveCharacterTextSplitter

splitter = RecursiveCharacterTextSplitter(
    chunk_size=500,    # max karakter per chunk
    chunk_overlap=50   # tumpang tindih antar chunk
)
chunks = splitter.split_documents(pages)
```

**Kenapa ada `chunk_overlap`?**
Supaya informasi yang ada di akhir chunk A dan awal chunk B tidak "terputus" konteksnya.

---

#### 3. Embedding Model
Mengubah teks (string) menjadi **vector** — array angka yang merepresentasikan makna semantik teks tersebut.

Teks yang maknanya mirip → vector yang posisinya berdekatan di ruang vektor.

```python
from langchain_community.embeddings import HuggingFaceEmbeddings

embedder = HuggingFaceEmbeddings(model_name="all-MiniLM-L6-v2")

vector = embedder.embed_query("apa itu machine learning?")
# vector = [0.023, -0.41, 0.88, ...] → 384 angka
```

Model `all-MiniLM-L6-v2` adalah pilihan populer: ringan, gratis, cukup akurat untuk bahasa Inggris.

---

#### 4. Vector Store (ChromaDB)
Database khusus yang menyimpan vectors dan bisa melakukan **similarity search** — mencari vector yang paling "dekat" dengan query vector.

```python
from langchain_community.vectorstores import Chroma

# Simpan semua chunks beserta embeddings-nya
vectorstore = Chroma.from_documents(chunks, embedding=embedder)

# Cari 3 chunks paling relevan untuk query
relevant_chunks = vectorstore.similarity_search("pertanyaan lo", k=3)
```

Di balik layar, ChromaDB pakai algoritma **cosine similarity** untuk mengukur "kedekatan" dua vector.

---

#### 5. LLM (Language Model)
Model bahasa yang generate jawaban akhir. Dalam project ini pakai **Groq** sebagai API provider dengan model **Llama 3 8B**.

```python
from langchain_groq import ChatGroq

llm = ChatGroq(model_name="llama3-8b-8192", temperature=0.2)
```

`temperature` mengontrol kreativitas jawaban:
- `0.0` = deterministik, selalu jawab yang sama
- `1.0` = sangat kreatif / random
- Untuk RAG → gunakan `0.1–0.3` supaya jawaban faktual

---

#### 6. Prompt Template
Template prompt yang menginstruksikan LLM untuk menjawab hanya berdasarkan konteks yang diberikan.

```
Kamu adalah asisten yang menjawab berdasarkan dokumen.
Gunakan HANYA informasi dari konteks ini:

{context}   ← diisi chunks yang relevan

Pertanyaan: {question}   ← diisi query user

Jawaban:
```

Prompt engineering sangat berpengaruh ke kualitas jawaban RAG.

---

#### 7. RetrievalQA Chain
LangChain menyediakan abstraksi `RetrievalQA` yang menggabungkan semua komponen di atas:

```
Query → Retriever → ambil chunks → masukkan ke prompt → LLM → jawaban
```

```python
from langchain.chains import RetrievalQA

chain = RetrievalQA.from_chain_type(
    llm=llm,
    retriever=vectorstore.as_retriever(search_kwargs={"k": 3}),
    chain_type="stuff",   # "stuff" = gabungkan chunks jadi 1 prompt
)
```

---

### Jenis-jenis Chain Type

| Chain Type | Cara Kerja | Kapan Pakai |
|---|---|---|
| `stuff` | Gabungkan semua chunks jadi 1 prompt | Dokumen pendek, chunks sedikit |
| `map_reduce` | Proses tiap chunk terpisah, lalu gabung | Dokumen panjang |
| `refine` | Iteratif, refine jawaban per chunk | Jawaban butuh detail tinggi |
| `map_rerank` | Proses tiap chunk, pilih jawaban terbaik | Perlu ranking jawaban |

Untuk belajar → `stuff` sudah cukup.

---

### Konsep Lanjutan yang Perlu Dipelajari Selanjutnya

Setelah paham basic RAG ini, step berikutnya:

1. **Hybrid Search** — kombinasi vector search + keyword search (BM25) untuk hasil lebih akurat
2. **Reranker** — filter ulang hasil retrieval dengan model khusus sebelum dikirim ke LLM
3. **Metadata Filtering** — filter chunks berdasarkan metadata (misal: hanya chunk dari halaman tertentu)
4. **Conversational RAG** — RAG dengan memory percakapan (chatbot yang ingat history)
5. **Multi-document RAG** — load dan query dari banyak dokumen sekaligus
6. **Evaluation** — cara mengukur kualitas RAG (RAGAS framework)

---

## Bagian 3 — Panduan Testing Notebook

### Langkah-langkah Menjalankan di Google Colab

1. Buka [colab.research.google.com](https://colab.research.google.com)
2. Klik `File > Upload notebook` → pilih file `rag_groq.ipynb`
3. Jalankan **Cell 1** terlebih dahulu (install library) — tunggu hingga selesai (~2 menit)
4. **Cell 2** — ganti `"gsk_xxxx_your_key_here"` dengan API key Groq lo yang asli
5. **Cell 3** — klik Run, akan muncul tombol upload → pilih PDF kamu
6. Jalankan **Cell 4 sampai 6** secara berurutan
7. **Cell 7** — ganti variabel `pertanyaan` sesuai isi dokumen lo, lalu jalankan
8. Opsional: jalankan **Cell 8** untuk mode tanya-jawab interaktif

### Tips Memilih PDF untuk Testing

- Pilih PDF yang isinya teks (bukan scan/gambar) — PDF scan butuh OCR dulu
- Dokumen 5–20 halaman ideal untuk percobaan pertama
- Contoh yang bagus: skripsi, laporan, artikel, dokumentasi teknis

### Troubleshooting Umum

| Error | Penyebab | Solusi |
|---|---|---|
| `AuthenticationError` | API key salah / expired | Cek ulang key di console.groq.com |
| `ModuleNotFoundError` | Library belum diinstall | Jalanin Cell 1 dulu |
| `Error loading PDF` | PDF terproteksi / scan | Coba PDF lain yang tidak diproteksi |
| Jawaban tidak relevan | Chunks terlalu besar/kecil | Coba ubah `chunk_size` ke 300 atau 700 |
| Model overloaded | Groq free tier rate limit | Tunggu beberapa detik, coba lagi |

---

## Penutup

RAG adalah fondasi dari hampir semua aplikasi AI yang bekerja dengan dokumen — dari chatbot customer service, document Q&A, sampai code search engine. Memahami RAG dari level notebook seperti ini adalah cara terbaik sebelum mengimplementasinya di production app (misalnya di Next.js backend atau CLI tool).

Setelah lo nyaman dengan notebook ini, step logis berikutnya adalah memindahkan logic RAG ini ke dalam API endpoint atau backend service.
