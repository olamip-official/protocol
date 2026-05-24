# Proses Rilis (Release Process)

*Hak Cipta © 2025 Ralph Gonzalez – https://olamip.org*

Dokumen ini menjelaskan proses standar yang saya gunakan untuk membuat, menguji, dan menerbitkan rilis spesifikasi OLAMIP serta perangkat pendukungnya. Untuk saat ini, semua rilis dikelola dan dimiliki sepenuhnya oleh saya.

---

## 1. Penomoran Versi (Versioning)

Saya mengikuti **Semantic Versioning 2.0.0**: https://semver.org/

- **MAJOR** — ketika saya membuat perubahan yang tidak kompatibel dengan versi sebelumnya.  
- **MINOR** — ketika saya menambahkan fitur atau bagian baru yang tetap kompatibel.  
- **PATCH** — ketika saya memperbaiki bug, memperjelas teks, atau melakukan koreksi kecil tanpa mengubah perilaku.  

Contoh:  
`1.0.0` → `1.0.1` (patch), `1.1.0` (minor), `2.0.0` (major).

---

## 2. Model Branching

Saya menggunakan alur kerja Git yang sederhana:

- `main` — branch stabil yang selalu siap dirilis.  
- Branch fitur atau perbaikan (misalnya `spec/delta-formats`, `fix/tag-rules`) — branch jangka pendek untuk perubahan tertentu.  
- Branch rilis (misalnya `release/v1.1.0`) — dibuat tepat sebelum rilis dipublikasikan.  

---

## 3. Prasyarat Sebelum Rilis

Sebelum memulai proses rilis, saya memastikan bahwa:

- Semua perubahan yang direncanakan telah digabungkan ke `main`.  
- Semua pemeriksaan otomatis (linting, validasi schema, build scripts) lulus.  
- Semua issue atau catatan yang relevan telah ditutup atau didokumentasikan.  
- `CHANGELOG.md` diperbarui dengan catatan yang jelas dan mudah dibaca.  
- Dokumentasi telah diperbarui, termasuk:  
  - `file-format-specification.md`  
  - `olamip-delta-file-format-specification.md`  
  - `docs/introduction.md`  
  - `faq.md`  
- File contoh `olamip.json` dan `olamip-delta.json` masih valid terhadap aturan terbaru.  

---

## 4. Membuat Release Candidate

1. Buat branch rilis dari `main`:

   ```bash
   git checkout main
   git checkout -b release/vX.Y.Z
   ```

2. Perbarui nomor versi (jika digunakan dalam tooling, skrip, atau aset lain).  
3. Perbarui `CHANGELOG.md` dengan header dan catatan versi.  
4. Commit perubahan:

   ```bash
   git add .
   git commit -m "chore(release): prepare v1.1.0"
   ```

5. Push branch:

   ```bash
   git push origin release/vX.Y.Z
   ```

6. Jalankan kembali validasi lokal atau CI dan pastikan semuanya lulus.  

---

## 5. Pengujian Release Candidate

Karena hanya saya yang mengelola rilis, pengujian pribadi saya menjadi gerbang utama:

- Saya memvalidasi bahwa file contoh `olamip.json` dan `olamip-delta.json` masih sesuai dengan schema.  
- Saya memeriksa bahwa semua contoh dalam dokumen spesifikasi masih mengikuti aturan yang dijelaskan.  
- Saya membaca ulang bagian yang diperbarui untuk memastikan tidak ada inkonsistensi.  

Jika saya menemukan masalah besar:

- Saya memperbaikinya di branch `release`.  
- Saya memperbarui `CHANGELOG.md` dan melakukan commit lagi.  
- Saya melakukan validasi ulang sebelum melanjutkan.  

---

## 6. Finalisasi Rilis

Setelah kandidat rilis stabil:

1. Buat tag rilis di Git:

   ```bash
   git tag -a vX.Y.Z -m "Release v1.1.0"
   git push origin vX.Y.Z
   ```

2. Buat rilis di GitHub (atau platform setara):

   - Gunakan tag `vX.Y.Z`.  
   - Tempel bagian relevan dari `CHANGELOG.md` ke deskripsi rilis.  
   - Lampirkan artefak yang dihasilkan (schema, CLI tools, snapshot tarball), jika ada.  

---

## 7. Langkah Setelah Rilis

Setelah rilis dipublikasikan:

- Saya memperbarui catatan internal atau roadmap mengenai apa yang telah dirilis.  
- Saya memindahkan item yang ditunda ke backlog atau catatan rilis berikutnya.  
- Jika ada kanal komunikasi (Twitter, newsletter, GitHub Discussions), saya dapat mengumumkan rilis baru dan menyoroti perubahan penting.  

---

## 8. Tata Kelola dan Pengawasan

Meskipun hanya ada satu maintainer, saya tetap menjaga struktur tata kelola:

- Perubahan apa pun yang memengaruhi schema inti, field wajib, atau versi protokol harus didokumentasikan dengan jelas dalam changelog.  
- Perubahan besar pada proses rilis harus dicatat dalam dokumen keputusan di `governance/decision-records/`.  

---

## 9. Rollback atau Patch

Jika rilis memiliki bug kritis:

- Saya membuat patch sesegera mungkin (misalnya `v1.1.1`).  
- Saya mendokumentasikan perbaikan tersebut di `CHANGELOG.md`.  
- Jika perlu, saya memperbarui dokumentasi atau memberi tahu bahwa versi sebelumnya tidak lagi direkomendasikan.  

---

## 10. Otomatisasi dan Skalabilitas Masa Depan

Saya mengotomatiskan sebanyak mungkin:

- Langkah berulang seperti:  
  - Memeriksa validitas JSON  
  - Memvalidasi file contoh terhadap schema  
  - Menghasilkan atau melinting teks changelog  

Saya menjaga proses tetap sederhana sehingga jika kontributor baru bergabung di masa depan, langkah‑langkahnya dapat dibagikan dengan mudah.

---

Proses rilis ini dikelola dan dipelihara oleh saya, dan dapat diperbarui kapan saja melalui workflow normal ketika saya meningkatkan prosedurnya.
