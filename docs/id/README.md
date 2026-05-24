<p align="center">
  <img src="assets/banner.jpg" alt="Spanduk OLAMIP" width="100%">
</p>

<p align="center">
  <img src="assets/logo.png" alt="Logo OLAMIP" width="140">
</p>

<h1 align="center">Protokol OLAMIP</h1>

<p align="center">
  <strong>Open Language‑Aligned Machine‑Interpretable Protocol</strong><br>
  Standar multibahasa, terstruktur, dan dapat dibaca mesin untuk situs web yang ramah AI.
</p>

<p align="center">
  <a href="https://olamip.org/">Situs Web</a> •
  <a href="https://olamip.org/file-format-specification/">Spesifikasi Format File</a> •
  <a href="https://olamip.org/delta-updates/">OLAMIP‑DELTA</a> •
  <a href="https://olamip.org/frequently-asked-questions/">FAQ</a>
</p>

---

# 📘 Ikhtisar

OLAMIP adalah protokol terbuka dan multibahasa yang memungkinkan situs web menjelaskan struktur, makna, dan tujuannya dalam format yang **dapat dibaca mesin**. OLAMIP menyediakan representasi JSON yang bersih sehingga model bahasa besar (LLM) dapat memprosesnya dengan andal, tanpa perlu menafsirkan HTML, CSS, atau JavaScript. [web:50][web:58]

Repositori ini berisi:

- **Spesifikasi resmi OLAMIP**.
- **Protokol pembaruan inkremental OLAMIP‑DELTA**.
- **Dokumentasi multibahasa dan materi tata kelola**.
- **Contoh dan templat**.

---

# 🌐 Indeks Bahasa

Dokumentasi OLAMIP tersedia dalam berbagai bahasa. Setiap kategori (Specs, Docs, Governance) memiliki folder paralel:

```text
/specs/<lang>
/docs/<lang>
/governance/<lang>
```

### Bahasa yang Didukung

| Bahasa | Kode | Specs | Docs | Governance |
|---|---|---|---|---|
| Inggris | en | [/specs/en](specs/en/) | [/docs/en](docs/en/) | [/governance/en](governance/en/) |
| Spanyol | es | [/specs/es](specs/es/) | [/docs/es](docs/es/) | [/governance/es](governance/es/) |
| Rusia | ru | [/specs/ru](specs/ru/) | [/docs/ru](docs/ru/) | [/governance/ru](governance/ru/) |
| Jepang | ja | [/specs/ja](specs/ja/) | [/docs/ja](docs/ja/) | [/governance/ja](governance/ja/) |
| Tionghoa (Sederhana) | zh‑CN | [/specs/zh-CN](specs/zh-CN/) | [/docs/zh-CN](docs/zh-CN/) | [/governance/zh-CN](governance/zh-CN/) |
| Portugis | pt | [/specs/pt](specs/pt/) | [/docs/pt](docs/pt/) | [/governance/pt](governance/pt/) |
| Hindi | hi | [/specs/hi](specs/hi/) | [/docs/hi](docs/hi/) | [/governance/hi](governance/hi/) |
| Bengali | bn | [/specs/bn](specs/bn/) | [/docs/bn](docs/bn/) | [/governance/bn](governance/bn/) |

> Bahasa lain ditambahkan secara berkala.

---

# 📑 Struktur Repositori

```text
/README.md              → Beranda utama repositori
/CHANGELOG.md           → Catatan perubahan global
/LICENSE                → Lisensi
/assets/                → Logo, banner, diagram

/specs/                 → Spesifikasi resmi OLAMIP & OLAMIP‑DELTA
    /en/ /es/ /ru/ /ja/ /zh-CN/ /pt/ /hi/ /bn/

/docs/                  → Dokumentasi, panduan, penjelasan
    /en/ /es/ /ru/ /ja/ /zh-CN/ /pt/ /hi/ /bn/

/governance/            → Tata kelola, versi, proses
    /en/ /es/ /ru/ /ja/ /zh-CN/ /pt/ /hi/ /bn/

/examples/              → Contoh file OLAMIP dan templat
```

---

# 📜 Spesifikasi

### **Spesifikasi Inti**
- Inggris: [/specs/en](specs/en/).
- Versi situs web: [https://olamip.org/file-format-specification/](https://olamip.org/file-format-specification/).

### **OLAMIP‑DELTA**
- Inggris: [/specs/en](specs/en/).
- Versi situs web: [https://olamip.org/delta-updates/](https://olamip.org/delta-updates/).

---

# 🧭 Tata Kelola

Tata kelola, versi, dan evolusi protokol:

- Inggris: [/governance/en](governance/en/).
- Hindi: [/governance/hi](governance/hi/).
- Bengali: [/governance/bn](governance/bn/).
- Semua bahasa: [/governance](governance/).

---

# 🧪 Contoh

Contoh file dan templat OLAMIP:

- [/examples](examples/).

Mencakup:

- Contoh `olamip.json`.
- Contoh `olamip-delta.json`.
- Templat section/entry.
- Pola praktik terbaik.

---

# 📄 Lisensi

Proyek ini dirilis di bawah lisensi terbuka. Lihat file `/LICENSE` dan folder `/governance` untuk detailnya.

---

<p align="center">
  <strong>OLAMIP — Membuat Web Dapat Dibaca Mesin.</strong><br>
  <a href="https://olamip.org/">https://olamip.org/</a>
</p>