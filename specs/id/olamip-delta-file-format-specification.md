# Spesifikasi Format Berkas OLAMIP-DELTA

*Hak Cipta © 2025 Ralph Gonzalez – https://olamip.org*

Itifak **OLAMIP-DELTA** mendefinisikan format berkas delta berbasis JSON untuk pembaruan inkremental pada `olamip.json`. Spesifikasi ini ditujukan bagi webmaster dan implementer yang sudah familiar dengan data terstruktur, HTML, dan publikasi bergaya sitemap.

## 1. Ikhtisar

`olamip.json` adalah representasi resmi dan lengkap dari struktur serta konten situs Anda. `olamip-delta.json` adalah berkas pendamping opsional yang menjelaskan perubahan pada snapshot tersebut sejak pembaruan terakhir.

Bersama-sama, keduanya membentuk aliran perubahan yang dapat dipahami mesin:

- `olamip.json`: snapshot lengkap dan terstruktur.
- `olamip-delta.json`: log perubahan inkremental.

---

## 2. Konsep Inti

- **Entry** — item konten pada tingkat daun, misalnya artikel blog, halaman produk, atau halaman dokumentasi.
- **Section / Subsection** — pengelompokan hierarkis, misalnya kategori blog, kategori dokumentasi, koleksi produk, atau grup proyek.
- **Operation** — salah satu dari `added`, `updated`, `removed`.
- **Delta** — kumpulan perubahan berbasis tanggal yang berisi `added`, `updated`, dan `removed`.

Format ini dirancang agar:

- **Utama untuk mesin**, tetapi tetap cukup mudah dibaca manusia untuk diperiksa secara manual.
- **Inkremental**, sehingga sistem AI dapat tetap sinkron tanpa memproses ulang seluruh situs.

---

## 3. Lokasi Berkas dan Penemuan

### 3.1 Lokasi Berkas

`olamip.json` harus dihosting di akar domain Anda:

```text
https://yourdomain.com/olamip.json
```

`olamip-delta.json` harus dihosting di direktori yang sama:

```text
https://yourdomain.com/olamip-delta.json
```

Delta berversi (opsional) berada di direktori yang sama:

```text
/olamip.json
/olamip-delta-YYYY-MM-DD.json
```

### 3.2 Mekanisme Penemuan

Webmaster harus mendeklarasikan lokasi `olamip.json` menggunakan kedua tag berikut di bagian `<head>` situs:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

Sistem AI yang mengetahui `olamip.json` akan otomatis mencari `olamip-delta.json` di direktori yang sama dan menerapkan delta sesuai spesifikasi ini. Tidak perlu menambahkan tag `<link>` atau `<meta>` terpisah untuk `olamip-delta.json`.

---

## 4. Struktur Tingkat Atas

Pada tingkat teratas, `olamip-delta.json` adalah objek JSON dengan field berikut:

```json
{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "window_days": 7,
  "last_updated": "2026-04-28",
  "deltas": [
    {
      "date": "2026-04-27",
      "added": [
        {
          "title": "Contoh entry",
          "url": "https://example.com/page/",
          "summary": "...",
          "content_type": "blog_article"
        }
      ],
      "updated": [
        {
          "url": "https://example.com/page/",
          "summary": "Ringkasan diperbarui"
        }
      ],
      "removed": [
        {
          "url": "https://example.com/removed-page/"
        }
      ]
    }
  ]
}
```

### 4.1 Field Tingkat Atas yang Wajib

- `protocol` (string, wajib)  
  Harus bernilai `"OLAMIP-DELTA"`.  
  Ini membedakan berkas delta dari berkas OLAMIP utama dan dari format delta lainnya.

- `version` (string, wajib)  
  Harus sesuai dengan skema versi yang didokumentasikan, misalnya `"1.0.0"`.  
  Ini memungkinkan implementer mendeteksi perubahan skema.

- `window_days` (integer, opsional)  
  Jumlah hari dalam jendela bergulir; harus bernilai positif (biasanya 7–30).  
  Hanya diperlukan jika menggunakan berkas delta bergulir.

- `last_updated` (string, wajib)  
  Tanggal terakhir berkas delta ini dimodifikasi, dalam format ISO-8601 `YYYY-MM-DD`.  
  Ini membantu crawler menentukan kapan harus mengambil ulang.

- `deltas` (array, wajib)  
  Array non-kosong dari kumpulan perubahan berbasis tanggal.  
  Setiap item adalah **objek delta** dengan `date`, `added`, `updated`, dan `removed`.

### 4.2 Array `deltas` dan Urutan Tanggal

- `deltas` harus diurutkan berdasarkan `"date"` secara menaik (tertua dulu, terbaru terakhir).
- Setiap objek delta harus memiliki field `"date"` dalam format ISO-8601 `YYYY-MM-DD`.
- `deltas` harus non-kosong.

Urutan ini tidak opsional untuk kesesuaian: sistem AI menganggap delta disusun secara kronologis.

---

## 5. Struktur Objek Delta

Setiap item dalam `deltas` adalah objek delta dengan bentuk berikut:

```json
{
  "date": "2026-04-27",
  "added": [ ... ],
  "updated": [ ... ],
  "removed": [ ... ]
}
```

Untuk kejelasan dan konsistensi yang dapat dipahami mesin, webmaster **HARUS** menyertakan ketiga field operasi pada setiap objek delta, walaupun sebagian kosong.

Field:

- `date` (string) — tanggal ISO-8601 `YYYY-MM-DD`.
- `added` (array) — nol atau lebih item baru.
- `updated` (array) — nol atau lebih item yang dimodifikasi.
- `removed` (array) — nol atau lebih item yang dihapus.

Contoh objek delta hanya dengan penambahan:

```json
{
  "date": "2026-04-27",
  "added": [
    {
      "title": "Halaman Baru",
      "summary": "Halaman baru yang ditambahkan ke situs.",
      "url": "https://example.com/new-page/"
    }
  ],
  "updated": [],
  "removed": []
}
```

Pola ini wajib bahkan jika pada hari itu hanya berlaku satu operasi.

---

## 6. Operasi

### 6.1 `added`

Item `added` adalah **objek penuh** dari tipe yang berlaku (entry, section, atau subsection). Field-nya harus cocok dengan spesifikasi format berkas OLAMIP untuk tipe tersebut.

Untuk sebuah **entry**:

- `title` (string, wajib) — judul yang dapat dibaca manusia.
- `summary` (string, wajib) — deskripsi ringkas.
- `url` (string, wajib) — URL kanonik absolut.
- `content_type` (string, wajib) — dari taksonomi `content_type` OLAMIP.
- `tags` (array, opsional) — daftar tag huruf kecil, satu token.
- `priority` (string, opsional) — `"high"`, `"medium"`, atau `"low"`.
- `policy` (string, opsional) — `"allow"` atau `"forbid"`.
- `language` (string, opsional) — kode bahasa BCP-47.
- `metadata` (object, opsional) — data terstruktur khusus domain.

Untuk **section atau subsection**, objek `added` mengikuti spesifikasi level section OLAMIP, dengan `title`, `summary`, `url`, `section_type`, `policy`, `tags`, `priority`, dan seterusnya.

#### Aturan untuk `added`

- Item `added` harus menyertakan `url`.
- Item `added` harus berupa objek penuh, setidaknya semua field wajib dari spesifikasi OLAMIP utama.
- Tidak boleh ada `url` yang muncul lebih dari sekali dalam objek delta yang sama.

Contoh:

```json
{
  "date": "2026-04-27",
  "added": [
    {
      "title": "Artikel Blog Baru",
      "summary": "Pengenalan singkat untuk fitur baru.",
      "url": "https://example.com/blog/new-feature/",
      "content_type": "blog_article",
      "tags": ["feature", "new"],
      "priority": "medium"
    }
  ],
  "updated": [],
  "removed": []
}
```

### 6.2 `updated`

Item `updated` harus menyertakan `url` dan boleh berupa:

- **Objek parsial** yang hanya berisi field yang berubah, atau
- **Objek penuh** yang berisi semua field.

Pada item `updated`, field yang hilang dianggap **tidak berubah**.

#### Contoh pembaruan parsial

```json
{
  "date": "2026-04-27",
  "updated": [
    {
      "url": "https://example.com/post/to-update/",
      "summary": "Ringkasan diperbarui dengan informasi baru.",
      "tags": ["feature", "update", "docs"]
    }
  ],
  "added": [],
  "removed": []
}
```

#### Contoh penggantian penuh

```json
{
  "date": "2026-04-27",
  "updated": [
    {
      "title": "Judul Post yang Diperbarui",
      "summary": "Ringkasan diperbarui dengan informasi baru.",
      "url": "https://example.com/post/to-update/",
      "content_type": "blog_article",
      "tags": ["feature", "update", "docs"],
      "priority": "medium"
    }
  ],
  "added": [],
  "removed": []
}
```

#### Aturan untuk `updated`

- Item `updated` harus menyertakan `url`.
- `updated` boleh berupa objek parsial (patch hanya pada field yang berubah) atau objek penuh.
- Item `updated` tidak boleh mengubah identitas entry melalui remapping `url`; itu adalah keputusan konfigurasi, bukan urusan berkas delta.

### 6.3 `removed`

Item `removed` bersifat **minimal** dan hanya untuk identitas.

```json
{
  "url": "https://example.com/page/to-remove/"
}
```

#### Aturan untuk `removed`

- Item `removed` harus menyertakan `url` dan **tidak boleh** menyertakan field lain.
- `url` harus berupa URL absolut dan kanonik.
- `removed` berlaku untuk URL tersebut beserta turunan strukturalnya (subsections dan entries), jika ada.
- Tidak boleh ada `url` yang muncul lebih dari sekali dalam objek delta yang sama.

Contoh:

```json
{
  "date": "2026-04-27",
  "removed": [
    {
      "url": "https://example.com/old-page/"
    }
  ],
  "added": [],
  "updated": []
}
```

---

## 7. URL dan Identitas Objek

OLAMIP-DELTA menggunakan URL kanonik sebagai identitas utama untuk entries, sections, dan subsections.

### Aturan identitas

- `url` harus berupa URL absolut, termasuk skema dan domain.
- Tidak boleh ada `url` yang muncul lebih dari sekali dalam objek delta yang sama, baik pada add, update, maupun remove.
- Saat sebuah section dihapus, semua turunannya secara logis juga dihapus.
- Mengubah URL memerlukan **remove** lalu **add**, bukan pembaruan `url` secara langsung.

### Praktik terbaik untuk URL

- Gunakan URL kanonik yang stabil dan jarang berubah.
- Hindari parameter kueri atau fragmen yang bergantung pada sesi.
- Jika halaman berpindah, gunakan redirect situs dan perlakukan perubahan sebagai persoalan otoritas konten di tingkat URL, bukan urusan berkas delta.

---

## 8. Jendela Delta Bergulir (Disarankan)

**Jendela delta bergulir** adalah mode yang direkomendasikan untuk `olamip-delta.json`.

Dalam model ini:

- Berkas delta berisi perubahan dari `window_days` terakhir.
- Sistem AI membaca semua delta di berkas dan menerapkannya secara kronologis.
- Ini mencegah pembaruan terlewat saat crawler terlambat mengambil data.

### Contoh Delta Bergulir (Jendela 7 Hari)

```json
{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "window_days": 7,
  "last_updated": "2026-04-28",
  "deltas": [
    {
      "date": "2026-04-22",
      "added": [
        {
          "title": "Produk Baru A",
          "url": "https://example.com/product/a",
          "summary": "Tambahan baru ke katalog."
        }
      ],
      "updated": [],
      "removed": []
    },
    {
      "date": "2026-04-23",
      "added": [],
      "updated": [
        {
          "url": "https://example.com/product/b",
          "summary": "Deskripsi diperbarui sesuai fitur baru."
        }
      ],
      "removed": []
    },
    {
      "date": "2026-04-24",
      "added": [],
      "updated": [],
      "removed": [
        {
          "url": "https://example.com/product/c"
        }
      ]
    }
  ]
}
```

Sistem AI:

- Membaca semua delta di berkas.
- Menerapkannya dalam urutan kronologis.
- Berakhir sinkron dengan status terbaru.

---

## 9. Delta Berversi (Alternatif)

Sebagian webmaster mungkin lebih suka **delta berversi**, di mana setiap berkas berisi perubahan untuk satu tanggal.

### Contoh Tata Letak

```text
/olamip.json
/olamip-delta-2026-03-01.json
/olamip-delta-2026-03-02.json
/olamip-delta-2026-03-03.json
```

### Bentuk Berkas

Setiap berkas versi memiliki struktur logis yang sama seperti berkas bergulir, tetapi hanya mewakili satu tanggal perubahan:

```json
{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "date": "2026-03-02",
  "added": [
    {
      "title": "Artikel Blog Baru",
      "url": "https://example.com/blog/new-post/",
      "summary": "Ringkasan singkat dari pembaruan."
    }
  ],
  "updated": [],
  "removed": []
}
```

### Aturan Ingest

Sistem AI seharusnya:

- Menemukan semua file `olamip-delta-*.json` di direktori yang sama dengan `olamip.json`.
- Mengurutkannya berdasarkan `date` secara menaik.
- Menerapkan hanya yang berada dalam jendela yang dikonfigurasi, misalnya 7–30 hari terakhir.

Pendekatan ini sangat berguna untuk:

- Situs berita dengan pembaruan harian yang sering.
- Katalog e-commerce dengan volume tinggi.
- Platform dokumentasi dengan siklus rilis cepat.

---

## 10. Perubahan Struktural: Sections dan Subsections

OLAMIP-DELTA menerapkan pola delta yang sama untuk **sections** dan **subsections** seperti pada **entries**. Ini memastikan sistem AI memahami bukan hanya halaman mana yang ada, tetapi juga bagaimana susunannya.

### 10.1 Menambahkan Section atau Subsection

Saat menambahkan section atau subsection, sertakan objek **section penuh** di `added`.

Contoh:

```json
{
  "date": "2026-05-01",
  "added": [
    {
      "title": "Tutorial",
      "summary": "Panduan langkah demi langkah untuk pemula dan pengguna tingkat lanjut.",
      "url": "https://example.com/tutorials/",
      "section_type": "blog_category",
      "policy": "allow",
      "tags": ["tutorials", "beginner", "advanced"],
      "priority": "medium"
    }
  ],
  "updated": [],
  "removed": []
}
```

#### Aturan

- Objek `added` untuk section harus menyertakan `title`, `summary`, `url`, dan `section_type`.
- Gunakan `section_type` untuk mencerminkan peran semantik, misalnya `blog_category`, `doc_category`, `product_collection`, atau `research_category`.
- `priority` dan `policy` membantu sistem AI memahami kepentingan dan aturan ingest untuk seluruh subtree.

### 10.2 Memperbarui Section atau Subsection

Sertakan `url` plus field apa pun yang berubah.

Contoh:

```json
{
  "date": "2026-05-02",
  "updated": [
    {
      "url": "https://example.com/blog/tutorials/",
      "title": "Tutorial Langkah demi Langkah",
      "summary": "Panduan untuk pemula dan pengguna tingkat lanjut, dengan contoh kode dan praktik terbaik.",
      "tags": ["tutorials", "beginner", "advanced", "code-examples"]
    }
  ],
  "added": [],
  "removed": []
}
```

#### Aturan

- Section yang diperbarui harus menyertakan `url`.
- `updated` boleh menyertakan subset field lain.
- Field yang hilang dianggap tidak berubah.

### 10.3 Menghapus Section atau Subsection

Sertakan hanya `url` di `removed`.

Contoh:

```json
{
  "date": "2026-05-03",
  "removed": [
    {
      "url": "https://example.com/blog/retired/"
    }
  ],
  "added": [],
  "updated": []
}
```

#### Aturan

- Item `removed` untuk section harus menyertakan `url` dan tidak boleh menyertakan field lain.
- URL harus menjadi URL kanonik dari section atau subsection yang dihapus.
- Saat sebuah section dihapus, seluruh turunannya juga dianggap dihapus.

---

## 11. Kesesuaian

Implementer dan webmaster sebaiknya memperlakukan hal berikut sebagai aturan kesesuaian.

### 11.1 Aturan MUST

Berkas OLAMIP-DELTA yang sesuai **HARUS**:

- Mendeklarasikan `protocol: "OLAMIP-DELTA"`.
- Menetapkan `version` ke nilai yang sesuai dengan skema versi yang didokumentasikan.
- Menyertakan array `deltas` yang tidak kosong.
- Setiap objek delta harus memiliki `date` dalam format ISO-8601 `YYYY-MM-DD`.
- Mengurutkan `deltas` berdasarkan `date` secara menaik.
- Menyertakan ketiga field operasi (`added`, `updated`, `removed`) di setiap objek delta, bahkan jika kosong.
- Memastikan `url` pada semua operasi adalah URL absolut dan kanonik.
- Memastikan tidak ada `url` yang muncul lebih dari sekali dalam objek delta yang sama.
- Memastikan `added` dan `updated` menyertakan `url`.
- Memastikan `removed` menyertakan `url` dan tidak ada field lain.
- Memperlakukan field yang hilang pada `updated` sebagai tidak berubah.
- Memperlakukan `removed` sebagai penghapusan URL yang diberikan beserta turunannya, misalnya subsections dan entries, dari indeks.

### 11.2 Aturan SHOULD

Implementasi yang sesuai **SEBAIKNYA**:

- Mengadopsi pola **jendela delta bergulir** sebagai default, kecuali alur kerja situs lebih cocok untuk delta berversi.
- Menjaga `last_updated` tetap akurat agar crawler dan validator mengetahui kapan file diperbarui.
- Menggunakan URL kanonik yang stabil dan jarang berubah agar tidak menimbulkan ambiguitas saat delta diterapkan.
- Menjaga `window_days` dalam rentang umum 7–30 hari, disesuaikan dengan frekuensi publikasi situs.
- Untuk delta berversi, menyimpan cukup file dalam jendela yang dikonfigurasi, misalnya 7–30 hari terakhir, agar pembaruan tidak terlewat ketika crawler terlambat.

### 11.3 Aturan MAY

Implementasi yang sesuai **BOLEH**:

- Menggunakan format delta berversi (`olamip-delta-YYYY-MM-DD.json`) alih-alih satu file bergulir, jika lebih cocok dengan pipeline deployment.
- Menerbitkan `olamip-delta.json` lebih sering atau lebih jarang daripada `olamip.json`, selama hubungan antara snapshot dan delta tetap jelas dan konsisten.
- Menambahkan metadata diagnostik atau logging di bawah kunci terpisah di luar field protokol, misalnya objek `diagnostics`, selama tidak bertentangan dengan field wajib atau mengubah makna delta.

---

## 12. Praktik Terbaik

Untuk memastikan kompatibilitas jangka panjang yang kuat dengan sistem AI, webmaster sebaiknya mengikuti praktik terbaik berikut saat menghasilkan `olamip-delta.json`.

### 12.1 Otomatiskan pembuatan delta

- Hubungkan pembuatan delta ke CMS atau pipeline deployment Anda.
- Setiap kali konten dipublikasikan, diperbarui, atau dihapus, perbarui `olamip-delta.json` sesuai perubahan tersebut.
- Validasi skema berkas sebelum deployment untuk menghindari kesalahan sintaks atau field yang tidak valid.

### 12.2 Jadikan `olamip.json` sebagai sumber otoritatif

- Pertahankan `olamip.json` yang valid dan mutakhir.
- Sistem AI baru dapat memulai dari file lengkap, lalu menerapkan delta berikutnya.
- Jangan memperlakukan `olamip-delta.json` sebagai pengganti snapshot utama.

### 12.3 Gunakan URL kanonik yang stabil

- Gunakan URL absolut dengan skema dan domain.
- Hindari pola URL yang sering berubah kecuali mekanisme situs, misalnya redirect, cukup andal untuk menjaga makna kanonik.
- Jika URL halaman benar-benar berubah, perlakukan sebagai “remove-then-add”, bukan pembaruan `url` langsung di delta.

### 12.4 Jaga konsistensi ringkasan dan metadata

- Ringkasan dalam item `added` dan `updated` harus konsisten dengan konten aktual.
- Perubahan redaksional kecil yang tidak mengubah makna masih boleh dikirim sebagai delta, tetapi sebaiknya diminimalkan agar tidak menimbulkan churn berlebih.
- Tag harus dinormalisasi, huruf kecil, satu kata, dan hyphenated untuk istilah majemuk, serta harus mencerminkan konsep yang stabil.

---

## 13. Sumber Terkait

Untuk memahami ekosistem OLAMIP secara lengkap, webmaster juga sebaiknya membaca:

- **[Spesifikasi Format Berkas OLAMIP](https://olamip.org/file-format-specification/)**  
  Menjelaskan struktur `olamip.json` dan mendefinisikan field untuk entry, section, dan subsection yang menjadi dasar format delta.

- **[Tanya Jawab OLAMIP](https://olamip.org/frequently-asked-questions/)**  
  Menjelaskan pertanyaan seputar deployment, penemuan, dan operasional bagi webmaster.

- **[Spesifikasi Format Berkas OLAMIP-DELTA](https://olamip.org/delta-updates/)**  
  Skema JSON formal untuk `olamip-delta.json`, yang dapat digunakan untuk memvalidasi dan melinting berkas delta secara otomatis.

- **[Mengapa OLAMIP Lebih Unggul daripada LLMs.txt](https://olamip.org/why-olamip-is-a-superior-standard-to-llms-txt/)**  
  Menjelaskan keunggulan OLAMIP dan OLAMIP-DELTA dibanding format metadata yang lebih sederhana dan tidak terstruktur.

---

Dokumen ini melengkapi spesifikasi format berkas OLAMIP-DELTA. Webmaster dan implementer dapat menggunakan dokumen ini sebagai referensi untuk menghasilkan, memvalidasi, dan mengonsumsi berkas `olamip-delta.json` di lingkungan produksi.