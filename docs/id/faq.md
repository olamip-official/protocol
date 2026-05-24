# Pertanyaan yang Sering Diajukan (FAQ)

*Hak Cipta © 2025 Ralph Gonzalez – https://olamip.org*

## Apa itu OLAMIP?

OLAMIP adalah singkatan dari **Open Language‑Aligned Machine‑Interpretable Protocol**.  
Ini adalah format terbuka dan sederhana yang memungkinkan situs web mempublikasikan ringkasan terstruktur dari kontennya sehingga model bahasa besar (LLM) dapat memahami dan mempelajarinya dengan lebih efektif.

## Mengapa saya harus menggunakan OLAMIP?

Karena LLM sudah merayapi situs Anda, tetapi mereka kesulitan menafsirkannya. Dengan OLAMIP, Anda dapat:

- Mengajarkan AI secara tepat tentang isi halaman Anda.  
- Menyoroti konten Anda yang paling berharga.  
- Meningkatkan keterlihatan dalam alat dan pencarian berbasis AI.  
- Mengurangi kesalahpahaman dan halusinasi.

Ini adalah perbedaan antara AI yang *menebak* dan AI yang *memahami*.

## Apakah OLAMIP mengungkapkan informasi pribadi?

Tidak. OLAMIP hanya merangkum konten yang sudah bersifat publik di situs Anda.  
Ini tidak mengungkapkan apa pun yang tidak terlihat dalam HTML, metadata, atau markup SEO Anda.

## Bisakah pesaing menggunakan file OLAMIP saya?

Tidak lebih dari mereka sudah menggunakan halaman publik, metadata, atau sitemap Anda.  
OLAMIP sebenarnya memberi Anda **lebih banyak kontrol** atas bagaimana sistem AI menafsirkan konten Anda — bukan kurang.

## Seperti apa tampilan file `olamip.json`?

`olamip.json` adalah dokumen terstruktur dalam format JSON yang menggambarkan konten, hierarki, dan metadata situs Anda dengan cara yang mudah ditafsirkan oleh sistem AI.  
Untuk melihat struktur teknis lengkapnya, kunjungi halaman [Spesifikasi Format File](https://olamip.org/file-format-specification) atau lihat contoh nyata melalui [studi kasus TimeLAX.com](https://olamip.org/how-olamip-transforms-ai-discovery-a-timelax-com-use-case/).

## Di mana saya harus menempatkan file tersebut?

Di root domain Anda:

```
https://yourdomain.com/olamip.json
```

Anda juga dapat menambahkan tag penemuan di beranda Anda:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## Apa fungsi bidang “priority”?

Bidang ini memberi sinyal seberapa penting suatu halaman bagi LLM. Gunakan dengan bijak:

- `high` → konten inti dan sangat penting.  
- `medium` → nilai default untuk sebagian besar halaman.  
- `low` → konten khusus, usang, atau bernilai rendah.

Jika semuanya ditandai `high`, maka tidak ada yang benar‑benar `high`.

## Mengapa saya perlu menyertakan URL jika ringkasan sudah disediakan?

Ringkasan memberi tahu AI *apa arti* halaman, tetapi URL memberi tahu AI *halaman apa* itu.  
Ringkasan bersifat deskriptif, tetapi bukan pengenal unik.  
URL adalah satu‑satunya referensi kanonis yang stabil ke halaman sebenarnya.

Sistem AI menggunakan URL untuk:

- Mengambil dan memverifikasi konten lengkap.  
- Menghindari pengindeksan duplikat.  
- Mendeteksi ketika halaman berpindah atau berubah.  
- Menghubungkan data OLAMIP ke schema.org, sitemap, dan crawler.  
- Mengutip halaman saat menghasilkan jawaban.

Tanpa URL, dua halaman berbeda dengan ringkasan serupa akan sulit dibedakan.

## Apa fungsi bidang “policy”, dan apakah saya perlu mengaturnya?

Bidang `policy` mengontrol apakah sistem AI diizinkan mengindeks suatu bagian, subbagian, atau entri.  
Nilai yang valid adalah `"allow"` dan `"forbid"`.

Jika Anda menghilangkan bidang ini, OLAMIP akan mewarisi kebijakan dari induk terdekat.  
Jika tidak ada induk yang menetapkan kebijakan, nilai default adalah `"allow"`.

Sebagian besar situs dapat mengabaikan bidang ini sepenuhnya.  
Gunakan `"forbid"` hanya jika Anda ingin mengecualikan halaman atau bagian tertentu dari pengindeksan AI.

## Apa perbedaan antara OLAMIP dan sitemap tradisional?

Sitemap adalah indeks navigasi, sedangkan OLAMIP adalah **peta pengetahuan yang dapat ditafsirkan mesin**.

### Sitemap XML

- Mendaftar URL.  
- Memberikan petunjuk crawling.  
- Membantu mesin pencari menemukan halaman.  
- Tidak memiliki makna semantik.  
- Tidak memiliki hierarki konten selain struktur URL.

### OLAMIP

- Mendefinisikan jenis konten (`page`, `project`, `doc_page`, dll.).  
- Mendefinisikan jenis bagian (`project_group`, `doc_category`, dll.).  
- Menyediakan deskripsi kanonis.  
- Menyediakan ringkasan yang dioptimalkan untuk LLM.  
- Menyediakan tag dan metadata.  
- Menetapkan hierarki eksplisit.  
- Memberikan representasi terstruktur dari seluruh situs kepada sistem AI.

Singkatnya:

- **Sitemap XML = “Ini halaman saya.”**  
- **OLAMIP = “Ini makna situs saya.”**

Itulah sebabnya sistem AI dapat menggunakan OLAMIP jauh lebih efektif daripada sitemap tradisional.

## Apakah file OLAMIP dengan lebih dari 3.000 URL terlalu besar?

Tidak sama sekali.  
File dengan 3.000 entri masih dalam batas aman.  
Bahkan dengan ringkasan panjang, ukuran totalnya sekitar 6 MB — sangat kecil untuk server, browser, dan sistem AI modern.

## Berapa ukuran tipikal entri OLAMIP?

| Panjang Ringkasan | Perkiraan Ukuran |
|---|---|
| Pendek | 300–600 byte |
| Panjang | 1–2 KB |

## Apa yang membuat OLAMIP tetap skalabel meskipun ukuran file besar?

- Struktur hierarkis mengurangi pengulangan.  
- Ringkasan ringkas (biasanya < 500 karakter).  
- Kompresi efisien (Gzip atau Brotli mengurangi ukuran 70–90%).

## Seberapa besar file OLAMIP sebelum menimbulkan masalah kinerja?

| Ukuran File | Perkiraan URL | Dampak |
|---|---:|---|
| 1–10 MB | 500–5.000 | Sangat baik. Normal. |
| 10–25 MB | 5.000–12.000 | Masih baik. Sedikit lebih lambat. |
| 25–50 MB | 12.000–25.000 | Besar tapi dapat dikelola. |
| 50–100 MB | 25.000–50.000 | Berat. Beberapa sistem mungkin melambat. |
| 100+ MB | 50.000+ | Terlalu besar untuk banyak pipeline LLM. |

Sebagian besar webmaster dapat menangani file OLAMIP dengan puluhan ribu entri tanpa masalah.

## Seberapa sering saya harus memperbaruinya?

Setiap kali Anda menerbitkan konten baru atau memperbarui halaman yang ada.  
Perlakukannya seperti sitemap — tetapi untuk AI.

## Apakah saya perlu memperbarui file utama jika menggunakan `olamip-delta.json`?

Ya.  
File utama `olamip.json` harus selalu mencerminkan keadaan terbaru situs Anda.  
File opsional `olamip-delta.json` hanya berisi perubahan terbaru.

## Apakah saya perlu menambahkan tag `<link>` untuk `olamip-delta.json`?

Tidak.  
Hanya file utama `olamip.json` yang perlu direferensikan di bagian `<head>` situs Anda.  
Setelah sistem AI mengetahui lokasi file utama, mereka akan secara otomatis mencari file delta di direktori yang sama.

## Apakah LLM benar‑benar akan membaca ini?

Ya — itulah tujuannya.  
Seiring meningkatnya adopsi, LLM akan memprioritaskan sumber terstruktur seperti OLAMIP.

## Bagaimana cara menguji apakah bot AI dapat mengambil file `olamip.json` saya?

Coba perintah berikut untuk OpenAI, Anthropic, dan Perplexity:

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```
