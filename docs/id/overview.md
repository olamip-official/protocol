# Ikhtisar OLAMIP

*Hak Cipta © 2025 Ralph Gonzalez – https://olamip.org*

## Open Language‑Aligned Machine‑Interpretable Protocol

OLAMIP adalah standar terbuka yang memungkinkan situs web berkomunikasi dengan jelas dan sengaja dengan sistem kecerdasan buatan. Alih‑alih memaksa large language models (LLM) menebak makna dari HTML, CSS, dan JavaScript yang berisik, OLAMIP menyediakan representasi bersih, terstruktur, dan dapat ditafsirkan mesin dari konten terpenting situs Anda.

Dokumen ini memberikan pengenalan tingkat tinggi tentang tujuan, filosofi, format file, dan model pembaruan OLAMIP.

---

## 1. Mengapa OLAMIP Dibuat

Situs modern dibangun untuk browser, bukan untuk AI. Akibatnya, LLM sering kesulitan dengan:

- HTML yang penuh elemen tata letak  
- Konten yang dirender oleh JavaScript  
- Menu navigasi, iklan, dan elemen berulang  
- Halaman duplikat atau tidak relevan  

LLM *sudah* merayapi situs Anda, tetapi sering salah menafsirkannya. OLAMIP menyelesaikan masalah ini dengan memberikan sistem AI apa yang mereka butuhkan:

- File JSON ringan (`/olamip.json`)  
- Ringkasan bersih yang dikurasi manusia  
- Metadata terstruktur  
- URL kanonis  
- Hierarki jelas dari bagian, subbagian, dan entri  

Dengan mengadopsi OLAMIP, Anda mengendalikan bagaimana AI memahami dan merepresentasikan konten Anda.

---

## 2. Filosofi Inti: Ramah Manusia + Ramah Mesin

OLAMIP dirancang sebagai jembatan komunikasi antara manusia dan AI. Setiap bagian dari protokol harus:

| Prinsip | Ramah Manusia | Ramah Mesin |
|---------|----------------|--------------|
| Format File | Mudah ditulis dan divalidasi | JSON ketat dan dapat diprediksi |
| Nama Field | Intuitif dan jelas | Huruf kecil, sesuai skema |
| Ringkasan | Bahasa jelas, selaras dengan brand | Ringkas, faktual, kaya semantik |
| Sistem Prioritas | Label sederhana (`"high"`, `"medium"`, `"low"`) | Dapat dipetakan ke bobot peringkat |
| Dokumentasi | Panduan bahasa sederhana | Skema JSON, aturan validator |
| Peralatan | Generator, plugin CMS | CLI tools, parser, test suite |

- **Untuk manusia:** OLAMIP harus mudah diadopsi tanpa keahlian teknis mendalam.  
- **Untuk mesin:** OLAMIP harus terstruktur, dapat diprediksi, dan tidak ambigu.

---

## 3. File OLAMIP (`/olamip.json`)

File OLAMIP adalah dokumen JSON terstruktur yang dihosting di:

`https://yourdomain.com/olamip.json`

File ini berisi:

- `protocol` — harus `"OLAMIP"`  
- `version` — versi protokol  
- `identity` — identitas situs  
- `content` — hierarki konten terstruktur  
- `metadata` — bahasa, tanggal pembaruan, dan bidang global lainnya  

### 3.1 Tag Penemuan (Discovery Tags)

Agar sistem AI dapat menemukan file OLAMIP dengan andal, sertakan:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

Menggunakan keduanya memberikan redundansi, kompatibilitas, dan kesiapan masa depan.

---

## 4. Struktur File

### 4.1 Objek Identity

Menjelaskan situs atau organisasi:

- `name` — wajib  
- `type` — wajib  
- `canonical_description` — wajib  
- `tags` — opsional  

### 4.2 Objek Content

Berisi:

- `overview`  
- `sections`  
- `subsections` (opsional)  
- `entries` (unit konten paling detail)  

Mendukung kedalaman hierarki tak terbatas.

### 4.3 Sections

Section mengelompokkan konten terkait. Field wajib:

- `title`  
- `summary`  
- `url`  
- `section_type`  

Field opsional:

- `policy` (`"allow"` atau `"forbid"`)  
- `tags`  
- `priority`  
- `published`  
- `language`  
- `subsections`  
- `entries`  

**Pewarisan kebijakan:**  
Jika tidak ditentukan, kebijakan diwarisi dari induk. Default adalah `"allow"`.

### 4.4 Entries

Entries mewakili item konten individual seperti:

- Artikel blog  
- Berita  
- Produk  
- Halaman dokumentasi  
- Makalah penelitian  
- Media  

Field wajib:

- `title`  
- `summary`  
- `url`  
- `content_type`  

Field opsional:

- `policy`, `tags`, `priority`, `published`, `language`, `metadata`  

URL wajib karena mengidentifikasi konten secara unik dan memungkinkan AI memverifikasi, menghapus duplikasi, dan melakukan cross‑reference.

---

## 5. Metadata dan Dukungan Bahasa

Objek metadata dapat berisi:

```json
{
  "last_updated": "2026-01-21",
  "language": "en",
  "source_url": "https://www.yourwebsite.com/",
  "copyright": "© 2026 Example"
}
```

### Dukungan Multibahasa

Bahasa dapat ditentukan pada:

- tingkat file  
- tingkat section  
- tingkat entry  

Gunakan kode BCP‑47 seperti `en`, `id`, `es`, `pt-BR`, `zh-CN`.

Ini membantu AI:

- memilih tokenizer yang tepat  
- menghindari pencampuran bahasa  
- meningkatkan akurasi pencarian  
- mengurangi halusinasi  

---

## 6. Tag dan Prioritas

### 6.1 Tag

Tag memberikan petunjuk semantik ringan. Tag harus:

- huruf kecil  
- satu kata  
- ASCII  
- menggunakan tanda hubung untuk konsep multi‑kata  
- konsisten  

Contoh:

| Konsep | Tag Valid |
|--------|-----------|
| Los Angeles | `los-angeles` |
| Machine Learning | `machine-learning` |
| Data Science | `data-science` |

### 6.2 Prioritas

Nilai:

- `high` — konten utama  
- `medium` — default  
- `low` — konten niche atau lama  

Gunakan `high` secara hemat (5–10% entri).

---

## 7. Pembaruan Delta (`olamip-delta.json`)

Situs web terus berubah. OLAMIP mendukung pembaruan bertahap melalui file pendamping opsional:

`/olamip-delta.json`

Berisi hanya:

- entri baru  
- entri yang diperbarui  
- URL yang dihapus  

### 7.1 Mengapa Delta Penting

- Sistem AI tetap mutakhir tanpa memproses ulang seluruh file.  
- Katalog besar tetap mudah dikelola.  
- Pembaruan menyebar dengan cepat.  
- Tidak ada perubahan yang terlewat.  

### 7.2 Jendela Delta Bergulir (Direkomendasikan)

Simpan semua delta 7–30 hari terakhir dalam satu file.  
Sistem AI menerapkannya secara kronologis.

### 7.3 File Delta Berversi (Alternatif)

Publikasikan file terpisah:

`/olamip-delta-2026-03-01.json`  
`/olamip-delta-2026-03-02.json`  
...

Ideal untuk situs volume tinggi (berita, e‑commerce, dokumentasi).

### 7.4 Penemuan

Hanya file OLAMIP utama yang memerlukan tag `<link>` dan `<meta>`.  
Sistem AI otomatis mencari file delta di direktori yang sama.

---

## 8. OLAMIP vs Sitemap Tradisional

- Sitemap XML: “Ini halaman saya.”  
- OLAMIP: “Ini makna situs saya.”  

Sitemap hanya mencantumkan URL.  
OLAMIP menyediakan:

- ringkasan  
- jenis konten  
- hierarki section  
- tag  
- metadata  
- prioritas  
- kebijakan ingest  

Bersama schema.org, OLAMIP memberi AI pemahaman lengkap dan terkurasi tentang situs Anda.

---

## 9. FAQ (Ringkasan)

### Apakah OLAMIP mengungkapkan informasi pribadi?

Tidak. Hanya merangkum konten publik.

### Bisakah pesaing menggunakan file saya?

Tidak lebih dari mereka menggunakan halaman publik Anda.

### Seberapa besar file OLAMIP?

1–10 MB umum.  
Bahkan 3.000+ entri tetap normal dan aman.

### Seberapa sering harus diperbarui?

Setiap kali konten berubah — perlakukan seperti sitemap untuk AI.

### Apakah file utama harus diperbarui jika menggunakan delta?

Ya. `olamip.json` selalu menjadi sumber utama.

### Apakah LLM benar‑benar membaca file ini?

Ya — itulah tujuan protokol.  
Anda dapat menguji dengan:

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```

---

## 10. Ringkasan

OLAMIP mengubah situs Anda menjadi peta pengetahuan yang dapat ditafsirkan mesin:

- `olamip.json` — snapshot lengkap dan terstruktur  
- `olamip-delta.json` — log perubahan bertahap  
- section, subsection, entry — hierarki jelas  
- ringkasan, tag, prioritas — makna terkurasi manusia  
- discovery tags — ingest yang andal  
- dukungan multibahasa — siap secara global  

Dengan mengadopsi OLAMIP, Anda memastikan sistem AI memahami konten Anda sebagaimana Anda maksudkan — secara akurat, konsisten, dan dalam skala besar.
