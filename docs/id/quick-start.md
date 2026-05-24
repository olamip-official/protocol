# OLAMIP – Panduan Cepat

*Hak Cipta © 2025 Ralph Gonzalez – https://olamip.org*

Panduan cepat ini menunjukkan cara menerapkan OLAMIP pada sebuah situs web dengan cara yang paling sederhana.

---

## 1. Buat `olamip.json`

Tempatkan file `olamip.json` yang valid di direktori root situs Anda:

`https://yourdomain.com/olamip.json`

Mulailah dengan snapshot kecil dan bersih dari halaman‑halaman terpenting Anda.

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
  "identity": {
    "name": "Example Site",
    "type": "blog",
    "canonical_description": "A website about technology and product updates.",
    "tags": ["technology", "blog"]
  },
  "content": {
    "overview": {
      "summary": "A technology blog covering product news, tutorials, and opinion pieces."
    },
    "sections": [
      {
        "title": "Blog",
        "summary": "Articles and guides.",
        "url": "https://yourdomain.com/blog/",
        "section_type": "blog_category",
        "entries": [
          {
            "title": "Welcome to the Blog",
            "summary": "An introduction to the site’s content and purpose.",
            "url": "https://yourdomain.com/blog/welcome/",
            "content_type": "blog_article",
            "tags": ["intro", "blog"],
            "priority": "high",
            "language": "en"
          }
        ]
      }
    ]
  },
  "metadata": {
    "last_updated": "2026-04-06",
    "language": "en",
    "source_url": "https://yourdomain.com/",
    "copyright": "© 2026 Example Site"
  }
}
```

---

## 2. Tambahkan discovery tag ke HTML Anda

Tambahkan tag berikut ke bagian `<head>` pada homepage dan halaman‑halaman penting:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

---

## 3. Buat ringkasan yang singkat dan jelas

Tulis ringkasan pendek dan faktual yang menjelaskan isi halaman dan mengapa halaman tersebut penting. Hindari bahasa promosi, teks duplikat, dan deskripsi yang kabur.

Contoh yang baik:

- “Panduan pemula untuk fotografi long exposure.”
- “Detail produk dan harga untuk paket enterprise.”

Hindari:

- “Halaman terbaik di internet.”
- “Konten luar biasa yang pasti Anda sukai.”

---

## 4. Gunakan content type yang benar

Pilih `content_type` yang paling spesifik:

- `page`
- `landing_page`
- `legal_page`
- `blog_article`
- `news_article`
- `product`
- `service`
- `doc_page`
- `research_paper`
- `dataset`
- `project`
- `media_item`
- `resource`

---

## 5. Atur struktur menggunakan sections

Gunakan `sections` untuk mengelompokkan konten, dan `entries` untuk halaman individual. Jika perlu, tambahkan `subsections` untuk struktur yang lebih dalam.

Contoh:

- Section: Blog  
- Subsection: Tutorials  
- Entry: How to Use OLAMIP  

---

## 6. Tetapkan prioritas dengan sengaja

Gunakan:

- `high` untuk halaman paling penting  
- `medium` untuk halaman umum  
- `low` untuk halaman niche atau lama  

Jangan menandai semua halaman sebagai `high`.

---

## 7. Gunakan policy untuk mengontrol konten yang boleh dibaca AI

Field `policy` memberi tahu sistem AI apakah mereka boleh mengindeks suatu section, subsection, atau entry.

- `"allow"` → izinkan AI membaca  
- `"forbid"` → larang AI membaca  

Jika `policy` tidak ditentukan, nilainya diwarisi dari parent. Jika tidak ada parent yang menentukan policy, nilai default adalah `"allow"`.

Contoh:

**Melewati seluruh section:**

```json
{
  "title": "Deprecated Guides",
  "summary": "Outdated content, not for AI.",
  "url": "https://yourdomain.com/old-guides/",
  "section_type": "doc_category",
  "policy": "forbid"
}
```

**Melewati satu halaman:**

```json
{
  "title": "Internal Test Page",
  "summary": "Page used for staging only.",
  "url": "https://yourdomain.com/test/",
  "content_type": "page",
  "policy": "forbid"
}
```

---

## 8. Tambahkan metadata bahasa

Gunakan kode bahasa BCP‑47 seperti:

- `en`
- `id`
- `es`
- `fr`
- `de`
- `pt-BR`
- `zh-CN`

Anda dapat menetapkan bahasa pada level file, section, atau entry.

---

## 9. Perbarui file secara rutin

Setiap kali Anda menambah, mengubah, atau menghapus halaman penting, perbarui `olamip.json`.

Jika situs Anda sering berubah, pertimbangkan untuk menambahkan `olamip-delta.json` di direktori yang sama agar AI tetap mengikuti perubahan tanpa memuat ulang seluruh file.

---

## 10. Validasi sebelum dipublikasikan

Pastikan:

- JSON valid  
- URL absolut  
- Field wajib lengkap  
- Ringkasan jelas dan tidak terlalu panjang  
- Tag konsisten dan ternormalisasi  
- File disajikan dari root situs  

---

## Checklist implementasi minimal

- [ ] Buat `olamip.json`  
- [ ] Tempatkan di root situs  
- [ ] Tambahkan discovery tag ke `<head>`  
- [ ] Sertakan sections dan entries untuk konten utama  
- [ ] Tambahkan metadata dan bahasa  
- [ ] Buat ringkasan yang singkat dan akurat  
- [ ] Perbarui file saat situs berubah  
- [ ] Tambahkan `olamip-delta.json` jika ingin pembaruan bertahap  

---

## Rekomendasi titik awal

Jika Anda belum yakin harus mulai dari mana, terapkan OLAMIP hanya pada halaman‑halaman terpenting terlebih dahulu:

- Homepage  
- Indeks blog  
- Artikel terbaik  
- Halaman produk atau layanan utama  
- Halaman dokumentasi penting  

Setelah itu, perluas secara bertahap.
