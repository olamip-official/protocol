# Spesifikasi Format Berkas OLAMIP

*Hak Cipta © 2025 Ralph Gonzalez – https://olamip.org*

## Ikhtisar

Berkas OLAMIP (`/olamip.json`) adalah dokumen JSON terstruktur yang menyediakan ringkasan kurasi dari halaman-halaman paling penting pada situs web Anda. Berkas ini dirancang agar mudah diurai oleh model bahasa besar (LLM), sehingga model dapat memahami, memprioritaskan, dan menggunakan konten Anda dengan jelas, presisi, dan sesuai tujuan.

## Lokasi Berkas

Berkas OLAMIP harus dihosting di akar domain Anda:

`https://yourdomain.com/olamip.json`

## Mendeklarasikan Lokasi Berkas OLAMIP

Untuk memaksimalkan adopsi dan memastikan sistem dapat menemukan berkas OLAMIP Anda secara andal, publikasikan tag `<link>` dan tag `<meta>` di bagian `<head>` situs Anda.

### Penemuan Utama: `<link rel="olamip">`

- Praktik terstandar: crawler dan parser sudah lazim memindai tag `<link>` untuk sumber daya seperti canonical, sitemap, dan alternate.
- Ramah mesin: mendeklarasikan hubungan formal antara halaman dan berkas OLAMIP.
- Interoperabilitas: cocok dengan standar web yang ada, sehingga lebih mudah diadopsi sistem AI tanpa penanganan khusus.

### Penemuan Cadangan: `<meta name="olamip-location">`

- Mudah dibaca manusia: sederhana untuk ditambahkan dan dipahami webmaster.
- Kompatibilitas: beberapa parser dan alat lebih suka memindai tag `<meta>`.
- Redundansi: menjadi cadangan jika suatu crawler belum mendukung `rel="olamip"`.

### Mengapa Keduanya Lebih Kuat Bersama

- Tahan masa depan: seiring bertambahnya adopsi OLAMIP, sistem berbeda mungkin menemukan berkas dengan cara berbeda. Menyertakan keduanya memastikan tidak ada sistem yang tertinggal.
- Tangguh: jika satu metode gagal, metode lain menjadi cadangan.
- Mudah diintegrasikan: pengembang dapat memilih pendekatan yang paling sesuai untuk pipeline mereka tanpa memaksa webmaster menebak.
- Kepercayaan dan kejelasan: sinyal ganda mengurangi ambiguitas dan memperjelas lokasi berkas OLAMIP.

### Implementasi Praktik Terbaik

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

Dengan menyertakan kedua tag ini, Anda membuat berkas OLAMIP Anda dapat ditemukan oleh berbagai crawler, validator, dan sistem AI.

## Struktur Berkas

Berkas OLAMIP harus berupa dokumen JSON yang valid dan dienkode UTF-8, yang berisi:

- `protocol`
- `version`
- `identity`
- `content`
- `metadata`

### Struktur tingkat tinggi

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
  "identity": { "...": "..." },
  "content": { "...": "..." },
  "metadata": { "...": "..." }
}
```

## Objek Identity

| Field | Tipe | Wajib | Deskripsi |
|---|---|---:|---|
| `name` | `string` | Ya | Nama situs web atau organisasi. |
| `type` | `string` | Ya | Tipe entitas, seperti `company`, `blog`, atau `ecommerce`. |
| `canonical_description` | `string` | Ya | Deskripsi situs yang dapat dibaca manusia. |
| `tags` | `array<string>` | Tidak | Kata kunci opsional yang menjelaskan domain atau industri. |

## Objek Content

Objek `content` berisi:

- Sebuah `overview`
- Daftar `sections`
- Setiap section dapat memiliki `subsections`
- Setiap section atau subsection dapat memiliki `entries`

Struktur ini mendukung hierarki multi-level.

### Objek Overview

| Field | Tipe | Wajib | Deskripsi |
|---|---|---:|---|
| `summary` | `string` | Ya | Penjelasan singkat tentang tujuan situs web. |

## Spesifikasi Objek Section

Sebuah `Section` mewakili kategori, koleksi, atau pengelompokan konten. Section dapat berisi:

- `entries` (item konten)
- `subsections` (objek Section bersarang)

Ini memungkinkan kedalaman nesting tanpa batas.

### Field Section

| Field | Tipe | Wajib | Deskripsi |
|---|---|---:|---|
| `title` | `string` | Ya | Nama section yang dapat dibaca manusia. |
| `summary` | `string` | Ya | Deskripsi isi section. |
| `url` | `string` | Ya | URL kanonik section. |
| `section_type` | `string` | Ya | Klasifikasi semantik dari taksonomi. |
| `policy` | `string` | Tidak | `"allow"` atau `"forbid"`. Lihat aturan pewarisan di bawah. |
| `tags` | `array<string>` | Tidak | Kata kunci opsional. |
| `priority` | `string` | Tidak | `"high"`, `"medium"`, atau `"low"`. |
| `published` | `string` | Tidak | Tanggal ISO 8601. |
| `language` | `string` | Tidak | Gunakan kode bahasa BCP-47. |
| `entries` | `array<Entry>` | Ya | Array objek Entry. |
| `subsections` | `array<Section>` | Tidak | Array objek Section bersarang. |

### Nilai `section_type` yang Diizinkan

| `section_type` | Arti |
|---|---|
| `blog_category` | Mengelompokkan artikel blog. |
| `news_section` | Mengelompokkan artikel berita. |
| `product_collection` | Mengelompokkan produk atau layanan. |
| `doc_category` | Mengelompokkan halaman dokumentasi. |
| `research_category` | Mengelompokkan artikel penelitian atau dataset. |
| `project_group` | Mengelompokkan proyek portofolio. |
| `content_section` | Cadangan umum. |

## Perilaku Kebijakan dan Pewarisan

Field `policy` mengontrol apakah sistem AI diizinkan untuk meng-ingest konten yang diwakili oleh Section, Subsection, atau Entry. Nilai yang valid adalah `"allow"` dan `"forbid"`. Field ini bersifat opsional pada semua level struktur OLAMIP.

### Perilaku Default

Jika field `policy` dihilangkan pada level tertentu, kebijakan efektif ditentukan melalui pewarisan. Jika tidak ada leluhur yang mendefinisikan kebijakan, kebijakan efektif default adalah `"allow"`.

### Aturan Pewarisan

Sistem AI harus menentukan kebijakan efektif untuk setiap Entry dengan urutan berikut:

1. **Kebijakan di level Entry**
   Jika Entry mendefinisikan `policy`, nilai itu bersifat otoritatif.

2. **Kebijakan di level Subsection**
   Jika Entry tidak memiliki `policy`, sistem AI harus memeriksa Subsection terdekat yang mengandungnya.

3. **Kebijakan di level Section**
   Jika baik Entry maupun Subsection tidak mendefinisikan `policy`, sistem AI harus menggunakan kebijakan pada level Section.

4. **Kebijakan default**
   Jika tidak ada leluhur yang mendefinisikan `policy`, kebijakan efektif adalah `"allow"`.

### Penggunaan yang Dimaksud untuk Webmaster

- Untuk membuat seluruh situs dapat di-ingest oleh sistem AI, hilangkan field `policy` di semua tempat.
- Untuk mengontrol ingest, gunakan `"allow"` dan `"forbid"` secara selektif pada level apa pun dalam hierarki.
- Kebijakan yang diterapkan pada Section atau Subsection otomatis berlaku untuk semua turunannya kecuali dioverride.

### Persyaratan Sistem AI

Sistem AI harus:

- Menerapkan default `"allow"` hanya ketika tidak ada kebijakan eksplisit dalam rantai leluhur.
- Menghormati kebijakan efektif yang ditentukan melalui pewarisan.
- Memperlakukan `"forbid"` sebagai larangan tegas terhadap ingest.
- Memperlakukan `"allow"` sebagai izin untuk meng-ingest konten yang diwakili oleh node tersebut.

## Diagram Hierarki Multi-Level

```text
content
└── sections[]
    ├── Section (Level 1)
    │     ├── entries[]
    │     └── subsections[]
    │           ├── Section (Level 2)
    │           │     ├── entries[]
    │           │     └── subsections[]
    │           │           └── Section (Level 3)
    │           │                 └── entries[]
    │           └── ...
    └── ...
```

Struktur ini mendukung:

- News → Politics → Opinion → Articles
- Docs → API → Authentication → Pages
- Store → Clothing → Men → Jackets → Products
- Research → Physics → Quantum → Papers

## Spesifikasi Objek Entry

`Entry` adalah unit konten paling granular. Contohnya meliputi:

- Artikel blog
- Artikel berita
- Halaman produk
- Halaman dokumentasi
- Artikel penelitian
- Proyek portofolio
- Halaman legal
- Sumber daya yang dapat diunduh

### Field Entry

| Field | Tipe | Wajib | Deskripsi |
|---|---|---:|---|
| `title` | `string` | Ya | Judul yang dapat dibaca manusia. |
| `summary` | `string` | Ya | Deskripsi singkat dari konten. |
| `url` | `string` | Ya | URL kanonik absolut. |
| `content_type` | `string` | Ya | Klasifikasi semantik dari taksonomi. |
| `policy` | `string` | Tidak | `"allow"` atau `"forbid"`. Sama seperti pada Section/Subsection. |
| `tags` | `array<string>` | Tidak | Kata kunci opsional. |
| `priority` | `string` | Tidak | `"high"`, `"medium"`, atau `"low"`. |
| `published` | `string` | Tidak | Tanggal publikasi ISO 8601. |
| `language` | `string` | Tidak | Gunakan kode bahasa BCP-47. |
| `metadata` | `object` | Tidak | Informasi terstruktur khusus domain atau halaman. |

### Mengapa URL Wajib

Field `url` sangat penting karena menjadi pengenal kanonik untuk konten. Walaupun ringkasan menyampaikan makna konten, URL mengikat makna itu ke lokasi spesifik dan terverifikasi di web. Sistem AI menggunakan URL untuk deduplikasi, retrieval, validasi, dan cross-referencing dengan schema.org, sitemap, dan crawler.

### Nilai `content_type` yang Diizinkan

#### Halaman Umum

| `content_type` | Arti |
|---|---|
| `page` | Halaman konten standar. |
| `landing_page` | Halaman pemasaran atau kampanye. |
| `legal_page` | Ketentuan, privasi, penafian. |

#### Blog

| `content_type` | Arti |
|---|---|
| `blog_article` | Artikel blog. |

#### Berita

| `content_type` | Arti |
|---|---|
| `news_article` | Artikel berita. |

#### E-commerce

| `content_type` | Arti |
|---|---|
| `product` | Halaman produk. |
| `service` | Penawaran layanan. |

#### Dokumentasi

| `content_type` | Arti |
|---|---|
| `doc_page` | Halaman dokumentasi atau bantuan. |

#### Penelitian

| `content_type` | Arti |
|---|---|
| `research_paper` | Artikel ilmiah atau akademik. |
| `dataset` | Dataset penelitian. |

#### Portofolio

| `content_type` | Arti |
|---|---|
| `project` | Proyek portofolio atau studi kasus. |

#### Media / Sumber Daya

| `content_type` | Arti |
|---|---|
| `media_item` | Video, audio, galeri, dan lainnya. |
| `resource` | Materi unduhan atau referensi. |

## Array dan Field Multi-Nilai

Beberapa field OLAMIP dirancang untuk menampung lebih dari satu nilai. Ketika sebuah field berisi banyak elemen, seperti `tags`, atau daftar kustom apa pun yang didefinisikan di dalam field opsional `metadata` pada objek Entry, maka field tersebut harus direpresentasikan sebagai array JSON.

Array ditulis dengan tanda kurung siku (`[ ]`) dan berisi urutan nilai string yang dipisahkan koma. Penggunaan array memastikan sistem AI dapat menafsirkan data multi-nilai secara andal tanpa ambiguitas. Setiap elemen di dalam array harus berupa string mandiri, dan urutan elemen harus tetap konsisten jika urutan itu memiliki makna semantik.

Field `metadata` digunakan untuk menyimpan informasi terstruktur khusus domain atau khusus halaman yang melampaui field inti OLAMIP. Field ini memungkinkan penerbit menambahkan detail machine-interpretable yang relevan untuk industri atau tipe konten mereka, sehingga sistem AI mendapatkan sinyal konteks yang lebih kaya tanpa mengubah protokol inti.

## Objek Metadata

Objek metadata yang umum dapat terlihat seperti ini:

```json
{
  "last_updated": "2026-01-21",
  "language": "en",
  "source_url": "https://www.yourwebsite.com/",
  "copyright": "© 2026 Example"
}
```

## Mengapa Prioritas Kategoris adalah Pilihan Terbaik

| Manfaat | Penjelasan |
|---|---|
| Kejelasan & konsistensi | `"high"/"medium"/"low"` mudah dipahami secara universal. |
| Lebih sederhana bagi penerbit | Tidak memerlukan skor numerik. |
| Lebih mudah divalidasi | Alat dapat mendeteksi penyalahgunaan. |
| Fleksibel untuk pipeline LLM | Model dapat memetakan kategori ke bobot internal. |

### Panduan Field Priority

| Nilai | Arti |
|---|---|
| `"high"` | Konten unggulan dan sangat penting. Gunakan dengan hemat. |
| `"medium"` | Default untuk sebagian besar konten. |
| `"low"` | Konten niche, usang, atau bernilai rendah. |

### Praktik Terbaik

| Rekomendasi | Alasan |
|---|---|
| Batasi `"high"` hingga 5–10% | Menjaga sinyal tetap bermakna. |
| Gunakan `"medium"` sebagai default | Menjamin konsistensi. |
| Gunakan `"low"` untuk konten niche/legacy | Mengurangi noise. |
| Tinjau prioritas secara berkala | Menjaga file tetap akurat. |

### Mengapa Ini Penting

LLM dapat menggunakan `priority` untuk:

- Mengalokasikan perhatian lebih besar selama pelatihan.
- Memeringkat halaman untuk tugas retrieval.
- Menyaring konten yang kurang relevan.

Jika setiap halaman diberi label `"high"`, sinyal menjadi tidak bermakna, dan konten paling berharga Anda justru tenggelam dalam noise.

## Dukungan Multibahasa

Untuk mendukung situs web multibahasa secara penuh, definisikan `language` pada:

- Level file, sebagai default global, di dalam `metadata`.
- Level section, sebagai override opsional.
- Level entry, sebagai override opsional.

Ini penting untuk:

- Blog multibahasa.
- Media berita internasional.
- Situs penelitian dengan makalah dalam banyak bahasa.
- Toko e-commerce dengan halaman produk yang dilokalkan.

## Format Bahasa

Gunakan kode bahasa BCP-47, standar global yang digunakan oleh:

- schema.org.
- Atribut `lang` HTML.
- W3C.
- Mesin pencari.
- Pipeline LLM utama.

### Contoh

| Bahasa | Kode |
|---|---|
| English | `en` |
| Spanish | `es` |
| French | `fr` |
| German | `de` |
| Portuguese (Brazil) | `pt-BR` |
| Chinese (Simplified) | `zh-CN` |
| Arabic | `ar` |

## Mengapa Ini Penting bagi Sistem AI

LLM menggunakan metadata bahasa untuk:

- Memilih tokenizer yang tepat.
- Menerapkan model peringkasan yang sesuai.
- Menghindari pencampuran bahasa dalam embedding.
- Meningkatkan akurasi retrieval.
- Mengurangi halusinasi dalam konteks multibahasa.
- Mendukung pencarian dan penerjemahan lintas bahasa.

Tanpa field bahasa yang eksplisit, sistem AI harus menebak, dan sering kali tebakan itu salah.

## Aturan Validasi Umum

| Aturan | Persyaratan |
|---|---|
| JSON valid | Tidak boleh ada trailing comma atau struktur yang rusak. |
| Field wajib | Section dan entry harus menyertakan field wajib. |
| URL kanonik | Harus absolut dan stabil. |
| Panjang summary | Di bawah 500 karakter. |
| Tags | String satu kata, huruf kecil. |
| Subsections | Harus mengikuti skema Section. |

## Tujuan Tags

Tags menyediakan sinyal ringkas yang dapat dipahami mesin untuk membantu sistem AI memahami hubungan topikal di seluruh situs Anda. Tags mendukung clustering, retrieval, dan penyelarasan semantik, tetapi hanya jika mengikuti format yang konsisten dan dapat diprediksi.

Tags memberi sistem AI petunjuk semantik ringan tentang tema, topik, atau kategori yang terkait dengan sebuah entry. Tags bukan pengganti summary atau hierarki section; sebaliknya, tags adalah sinyal tambahan yang membantu model untuk:

- Mengelompokkan konten terkait.
- Menyimpulkan domain topik.
- Meningkatkan akurasi retrieval.
- Mengurangi ambiguitas antara halaman dengan judul serupa.
- Mendukung penalaran lintas halaman.

Tags bersifat opsional tetapi sangat disarankan untuk situs dengan inventaris konten besar atau topik yang saling tumpang tindih.

## Format Wajib untuk Tags

Tags harus selalu mengikuti pola normalisasi yang ketat agar konsisten antar-penerbit dan mudah ditafsirkan oleh sistem AI.

- Huruf kecil.
- String satu kata.
- Hanya karakter ASCII.
- Tanpa spasi.
- Tanpa tanda baca kecuali tanda hubung.
- Tanpa camelCase atau snake_case.

Normalisasi ini memastikan tags tetap menjadi token yang jelas, mudah diindeks, disematkan, dan dibandingkan.

## Tag Multi-Kata

Beberapa konsep secara alami terdiri dari beberapa kata. Untuk mempertahankan makna sambil tetap memenuhi syarat satu-token, tag multi-kata harus diubah menjadi bentuk dengan tanda hubung.

### Aturan pemformatan

- Ubah semua huruf menjadi huruf kecil.
- Ganti spasi dengan tanda hubung.
- Hapus tanda baca kecuali itu bagian dari istilah standar.
- Hindari underscore, camelCase, atau penggabungan tanpa pemisah.
- Jaga agar tag tetap ringkas dan bermakna secara semantik.

### Contoh

| Konsep | Tag Valid | Tag Tidak Valid |
|---|---|---|
| Los Angeles | `los-angeles` | `Los Angeles`, `los_angeles`, `losangeles` |
| Machine Learning | `machine-learning` | `MachineLearning`, `machine_learning` |
| Data Science | `data-science` | `data science`, `datascience` |
| Customer Support | `customer-support` | `customerSupport`, `customer support` |

Hyphenation mempertahankan batas semantik sambil menjaga tag tetap ramah mesin dan mudah dibaca manusia.

## Pertimbangan Multibahasa untuk Tags

Metadata bahasa menentukan bagaimana tags ditafsirkan. Tags sendiri tidak harus diterjemahkan, tetapi harus tetap konsisten dengan konteks bahasa pada entry.

### Aturan untuk situs multibahasa

- Tags sebaiknya ditulis dalam bahasa yang sama dengan entry, kecuali konsepnya sudah distandardisasi secara global (misalnya `javascript`).
- Jika konsep yang sama muncul dalam beberapa bahasa, setiap versi bahasa harus menggunakan tag lokalnya sendiri.
- Aturan hyphenation tetap sama di semua bahasa.
- Hindari mencampur bahasa dalam satu tag.
- Gunakan kode bahasa BCP-47 pada level file, section, atau entry untuk memperjelas interpretasi.

### Contoh

| Bahasa Entry | Konsep | Tag Valid |
|---|---|---|
| `en` | climate change | `climate-change` |
| `es` | cambio climático | `cambio-climatico` |
| `de` | künstliche intelligenz | `kunstliche-intelligenz` |

Tags tetap merupakan token yang dinormalisasi, tetapi bentuk linguistiknya harus sesuai dengan konten yang dijelaskan.

## Interaksi Tags dengan Sistem AI

Tags membantu sistem AI untuk:

- Mengelompokkan entry terkait.
- Membedakan topik yang mirip.
- Meningkatkan presisi retrieval.
- Mendukung pemetaan lintas bahasa.
- Mengurangi risiko halusinasi dengan memperkuat konteks.

Tags bukan sinyal peringkat, tetapi meningkatkan keterpahaman dan mengurangi ambiguitas, terutama pada situs besar atau multibahasa.

## Versioning Berkas OLAMIP

| Pedoman | Tujuan |
|---|---|
| Parser mengabaikan field yang tidak dikenal. | Menjamin kompatibilitas ke depan. |
| Penerbit memvalidasi terhadap schema terbaru. | Menjamin ketepatan. |

## Penyelarasan Semantik

OLAMIP melengkapi HTML, sitemap, dan metadata terstruktur lainnya dengan memberikan lapisan semantik yang lebih jelas dan lebih ramah AI. Jika HTML adalah presentasi, OLAMIP adalah interpretasi kuratorial dari situs web Anda.

## Contoh Struktur Berkas

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
  "identity": {
    "name": "Contoh Situs",
    "type": "blog",
    "canonical_description": "Situs contoh untuk demonstrasi OLAMIP.",
    "tags": ["technology", "documentation"]
  },
  "content": {
    "overview": {
      "summary": "Situs ini berisi dokumentasi, artikel, dan sumber daya terkait teknologi."
    },
    "sections": [
      {
        "title": "Dokumentasi",
        "summary": "Panduan teknis dan referensi.",
        "url": "https://yourdomain.com/docs/",
        "section_type": "doc_category",
        "policy": "allow",
        "tags": ["docs", "reference"],
        "priority": "high",
        "published": "2026-01-21",
        "language": "id",
        "entries": [
          {
            "title": "Memulai Cepat",
            "summary": "Panduan singkat untuk memulai.",
            "url": "https://yourdomain.com/docs/getting-started",
            "content_type": "doc_page",
            "policy": "allow",
            "tags": ["getting-started", "guide"],
            "priority": "high",
            "published": "2026-01-21",
            "language": "id",
            "metadata": {
              "reading_time": "5 min"
            }
          }
        ]
      }
    ]
  },
  "metadata": {
    "last_updated": "2026-01-21",
    "language": "id",
    "source_url": "https://yourdomain.com/olamip.json",
    "copyright": "© 2026 Example"
  }
}
```

## Catatan Implementasi

- Semua URL harus absolut.
- Semua string harus UTF-8.
- Hindari field ambigu atau berlebihan.
- Gunakan summary yang ringkas dan akurat.
- Pertahankan konsistensi penamaan di seluruh berkas.

## Kesimpulan

OLAMIP memberikan cara yang jelas, terstruktur, dan dapat dipahami mesin untuk mendeskripsikan konten web. Dengan mengikuti spesifikasi ini, webmaster memungkinkan sistem AI memahami hierarki situs, memprioritaskan halaman penting, menghormati kebijakan ingest, dan bekerja dengan lebih akurat, transparan, serta andal.