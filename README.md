# QA Testing — Website yajaya.store

Laporan tugas Testing & QA: pengujian website katalog kaos polos dan sablon DTF satuan milik Yajaya Store, Parung, Bogor. Pengujian dilakukan murni dari sudut pandang pelanggan (external / customer perspective): semua yang diuji adalah antarmuka pengguna, alur bisnis, perilaku HTTP dan performa, serta validasi input dari luar. Tidak ada white-box testing dan tidak ada akses ke source code backend.

Standar acuan: ISO/IEC/IEEE 29119 dan ISO/IEC 25010:2023. Semua standar dan jurnal yang dipakai terbit 2022–2026, lengkapnya di [references/standards-journals.md](references/standards-journals.md).

## Daftar Isi

- [1. Profil Objek Uji](#1-profil-objek-uji)
- [2. Perubahan Versi: 7 November → 20 November 2026](#2-perubahan-versi-7-november--20-november-2026)
- [3. Sudut Pandang Pengujian](#3-sudut-pandang-pengujian)
- [4. Alasan Pemilihan Metode](#4-alasan-pemilihan-metode)
- [5. Status Proyek](#5-status-proyek)
- [6. Struktur Repository](#6-struktur-repository)
- [7. Menjalankan Test Otomatis](#7-menjalankan-test-otomatis)

## 1. Profil Objek Uji

**yajaya.store** (<https://www.yajaya.store/>) adalah website milik Yajaya Store, toko kaos polos dan sablon DTF satuan di Jl. H. Mawi No. 12, Waru, Parung, Kabupaten Bogor. Toko ini nyata dan masih aktif: alamat, nomor WhatsApp (0856-8703-266), dan jam buka (Senin–Minggu, 09.00–23.00 WIB) tercantum di website, dan katalognya berisi produk yang benar-benar dijual, lengkap dengan harga dan jumlah terjual.

Website ini memenuhi kriteria tugas karena skalanya menengah ke bawah:

- **Bisnisnya UMKM, bukan portal besar.** Satu toko, satu lokasi, produknya puluhan bukan ribuan. Katalog per 20 November 2026 berisi 19 produk: kaos polos, kaos distro (Persija, Iron Maiden, Testament, Denzbagus), work jacket, overshirt, hoodie, celana denim, dan jasa sablon DTF.
- **Fiturnya terbatas dan masih bisa diuji satu per satu dalam waktu tugas.** Tidak ada marketplace, tidak ada sistem kasir yang kompleks. Order masih lewat WhatsApp, pembayaran dikonfirmasi lewat chat, dan tidak ada checkout atau pembayaran di website.
- **Dibuat untuk usaha nyata, bukan tugas kuliah atau demo.** Website ini adalah etalase dan kanal order sungguhan untuk toko yang beroperasi. Itu penting: defect yang ditemukan di sini berdampak langsung ke pembeli dan ke pemilik toko, bukan sekadar nilai di atas kertas.
- **Cukup kompleks untuk diuji dengan serius.** Ada multi-halaman, login, pencarian, filter, sortir, kalkulator harga sablon, galeri, dan blog. Cukup luas untuk black-box testing, UAT, regression testing, dan otomatisasi, tetapi tidak sebesar e-commerce umum yang tidak mungkin selesai dalam satu semester.

## 2. Perubahan Versi: 7 November → 20 November 2026

Website ini berubah total di antara dua pengecekan kami, dan perubahan itu justru menjadi bahan regression testing.

### Versi 7 November 2026 (baseline)

- Website **satu halaman**: semua konten ada di satu halaman panjang dengan navigasi anchor (`#produk`, `#harga`, `#testimoni`, `#faq`).
- **16 produk**, tanpa halaman detail produk. Produk hanya bisa dilihat lewat kartu dan modal Quick View.
- **Tidak ada login atau register.** Tidak ada akun pengguna, tidak ada dashboard.
- Order hanya lewat WhatsApp. Tidak ada sortir produk, tidak ada blog, tidak ada galeri.
- Inventaris lengkapnya ada di [docs/site-inventory-2026-11-07.md](docs/site-inventory-2026-11-07.md).

### Versi 20 November 2026 (versi yang diuji sekarang)

- Website **multi-halaman**: Home, Katalog, Sablon, Galeri, Blog, Tentang, Kontak, Login, Register, Dashboard, 19 halaman detail produk, dan 4 halaman artikel blog.
- **19 produk** dengan halaman detail masing-masing (pilihan warna dan ukuran, tombol Pesan Sekarang, link Beli di Shopee).
- **Login dan register sudah ada, tetapi hanya lewat Google.** Tidak ada kolom email atau password, tidak ada akun demo. Dashboard, tracking pesanan, dan invoice ada di balik login dan belum bisa diuji tanpa akun Google asli.
- Ada **sortir harga**, **kalkulator DTF** di halaman Sablon, **galeri** dengan filter dan lightbox, dan **blog**.
- Order tetap lewat WhatsApp. Masih tidak ada keranjang, checkout, atau pembayaran di website.
- Inventaris lengkapnya ada di [docs/site-inventory-2026-11-20.md](docs/site-inventory-2026-11-20.md).

### Peran perubahan ini dalam Regression Testing

Setiap fitur yang sudah ada di versi 7 November harus dicek ulang di versi 20 November: apakah masih berjalan, berubah perilaku, atau hilang sama sekali. Hasilnya:

- **Masih berjalan:** pencarian produk, filter kategori, sortir harga (baru), dan order lewat WhatsApp.
- **Berubah perilaku:** Quick View diganti halaman detail produk; filter warna pindah dari katalog ke halaman detail; pricelist hanya tersisa tabel DTF di halaman Sablon.
- **Hilang:** FAQ, tombol reset filter, dan bagian pricelist di Home.
- **Bug lama yang ikut hilang:** sebagian besar bug versi 7 November sudah tidak relevan karena fiturnya dihapus (misalnya filter warna dan Quick View). Daftar lengkap status bug lama ada di [bug-reports/bug-reports.md](bug-reports/bug-reports.md).
- **Bug baru yang muncul:** 10 bug baru di versi 20 November, antara lain filter dan lightbox galeri yang rusak, kategori Polo Shirt yang mengarah ke halaman login, dan harga kategori di Home yang tidak cocok dengan produk mana pun.

Checklist regression lengkapnya ada di [test-cases/test-cases-manual.md](test-cases/test-cases-manual.md).

## 3. Sudut Pandang Pengujian

Pengujian di proyek ini dilakukan sebagai **tester dari sisi pelanggan**. Artinya:

- Yang diuji adalah apa yang dilihat dan dilakukan pembeli: membuka halaman, mencari produk, memilih warna dan ukuran, menghitung harga sablon, mengklik tombol, dan mencoba login.
- Yang **tidak** diuji adalah kode sumber, database, API internal, dan unit test di level kode. White-box testing dan code-level unit testing dinyatakan **out-of-scope** di [test-plan/test-plan.md](test-plan/test-plan.md).
- File [docs/white-box-findings-2026-11-07.md](docs/white-box-findings-2026-11-07.md) adalah catatan historis dari pembacaan kode tanggal 7 November 2026. File itu disimpan sebagai arsip, bukan sebagai bagian dari pengujian berjalan, karena sudut pandang proyek ini sudah berubah menjadi murni eksternal.

Konsekuensinya, defect yang dilaporkan di sini selalu disertai langkah reproduksi dari antarmuka pengguna, bukan dari baris kode. Kalau sebuah masalah hanya bisa dibuktikan dengan membaca kode, masalah itu tidak dilaporkan sebagai bug di laporan ini.

## 4. Alasan Pemilihan Metode

### Black-Box Testing

Metode utama. Alasannya sederhana: sebagai tester dari sisi pelanggan, satu-satunya yang bisa kami amati adalah perilaku website dari luar. Black-box testing memakai teknik Equivalence Partitioning, Boundary Value Analysis, dan Decision Table untuk menguji login, navigasi, katalog, filter, pencarian, detail produk, dan order — tanpa perlu tahu cara kerja di dalamnya. Jurnal Chrisna dkk. (2024, Sinta 2) menunjukkan kombinasi black-box dengan white-box dan otomatisasi efektif untuk sistem web; di proyek ini porsi white-box-nya diganti dengan pengujian perilaku HTTP dan validasi input eksternal, karena akses kode tidak tersedia.

### UAT / Beta Testing

UAT menguji apakah website ini bisa dipakai oleh pembeli sungguhan untuk menyelesaikan tujuannya: menemukan produk, memahami harga, dan memesan. Skenario UAT di proyek ini adalah alur bisnis end-to-end dari sudut pandang pelanggan nyata, ditambah kuesioner kepuasan skala Likert. Safira dkk. (2023) memakai UAT sebagai penutup pengujian sistem web dan mencapai 94,67% — pola yang sama kami pakai di sini: UAT adalah penentu akhir apakah website layak dipakai, bukan sekadar apakah fiturnya berjalan.

### Regression Testing

Wajib di proyek ini karena website berubah total antara 7 dan 20 November 2026. Regression testing memastikan fitur lama tidak rusak oleh perubahan: pencarian, filter, order WhatsApp, dan konsistensi harga dicek ulang setelah update. Tanpa regression testing, laporan ini akan mencampuradukkan bug lama yang sudah hilang dengan bug baru yang muncul setelah update.

### Automation E2E — Cypress

Otomatisasi dipakai untuk skenario yang diulang terus: login, navigasi antar halaman, dan alur katalog. Cypress dipilih karena dirancang khusus untuk pengujian end-to-end di browser, dengan sinkronisasi otomatis dan debugging yang mudah — cocok untuk tim kecil yang menguji alur pelanggan dari luar. García dkk. (2024) membandingkan Cypress dengan Selenium, Puppeteer, dan Playwright; Gosik & Miłosz (2025) membandingkan langsung Cypress dan Playwright dan mencatat trade-off keduanya. Playwright lebih cepat dan lebih hemat memori, tetapi Cypress dipilih di sini karena skenarionya lebih mudah ditulis dan dirawat untuk alur pelanggan. Catatan: skrip Cypress di repo ini masih berupa draf dan belum dieksekusi di lingkungan final — lihat [automation/cypress/](automation/cypress/) (direncanakan, Tahap 6).

### Performance Testing — Apache JMeter

Performance testing mengukur apakah website tetap responsif saat banyak pengunjung datang bersamaan, memakai skenario load testing (50–100 user), stress testing (200–500 user), dan spike testing (lonjakan 10 ke 200 user), dengan metrik response time, throughput, dan error rate. Indrianto (2023) memakai pola yang sama — 50 dan 100 sampel dengan ramp-up 10 detik — dan Annisa dkk. (2026) menunjukkan website yang diuji stabil sampai 200 user bersamaan, lalu menurun di atas 300 user. Target SLA di proyek ini: response time rata-rata di bawah 2 detik. Rencana lengkapnya ada di [automation/jmeter/](automation/jmeter/) (direncanakan, Tahap 6).

### Security Testing — DAST dengan OWASP ZAP

Karena tidak ada akses kode, pengujian keamanan dilakukan dari luar dengan Dynamic Application Security Testing (DAST): OWASP ZAP memindai aplikasi yang sedang berjalan, memeriksa header keamanan HTTP, dan menguji input form login dengan payload dasar seperti SQL Injection (`' OR '1'='1`) dan XSS. DAST menemukan masalah konfigurasi, autentikasi, dan penanganan request yang hanya muncul saat aplikasi berjalan — hal yang tidak bisa ditemukan dari membaca spesifikasi. Panduan lengkapnya ada di [automation/security-dast/](automation/security-dast/) (direncanakan, Tahap 6).

### Kenapa manual dan otomatis dipakai bersamaan

Kurmaku dkk. (2022) me-meta-analisis 21 studi dan menyimpulkan testing otomatis lebih hemat biaya, coverage, dan waktu, tetapi testing manual sama baiknya, bahkan sedikit lebih baik, dalam menemukan fault — dan bedanya tidak signifikan secara statistik. Karena itu proyek ini memakai keduanya: manual untuk menemukan masalah yang butuh penilaian manusia (tampilan, konsistensi konten, alur bisnis), otomatis untuk mengulang skenario yang sama secara konsisten.

## 5. Status Proyek

- **Inventaris versi 7 November 2026:** selesai. Website satu halaman, 16 produk, tanpa login.
- **Inventaris versi 20 November 2026:** selesai dan sudah dicek dua kali (pagi dan sore 20 November, hasilnya sama). Website multi-halaman, 19 produk, login Google-only.
- **Test plan:** sudah direvisi mengikuti inventaris 20 November 2026.
- **Test case manual:** sudah direvisi. Bagian lama (versi 7 November) ditandai tidak berlaku; 35 test case baru untuk versi 20 November sudah ditulis, sebagian sudah dieksekusi saat inventarisasi.
- **Bug report:** 13 bug versi lama (sebagian besar sudah tidak relevan) dan 10 bug baru versi 20 November (BUG-14 sampai BUG-23).
- **Test otomatis:** HTTP smoke test sudah jalan (5 dari 5 lolos, 7 November 2026) dan perlu dijalankan ulang untuk versi baru. Playwright sempat dipakai, tetapi instalasi browsernya gagal 3x karena download dari CDN putus di sekitar 90 persen; otomatisasi E2E pindah ke Cypress (Tahap 6).
- **Dashboard:** belum bisa diuji — butuh akun Google asli, tidak ada akun demo.
- **Wawancara:** belum ada narasumber. Panduan wawancara sudah siap; transkrip akan diisi setelah wawancara nyata (Tahap 7).
- **Push ke GitHub:** revisi 20 November 2026 sudah naik (12 commit). Tahap audit ini (Tahap 1) masih lokal dan belum di-push — ini tugas kelompok, push berikutnya menunggu perintah.

## 6. Struktur Repository

```
qa-yajaya-store/
├── README.md
├── docs/
│   ├── site-inventory-2026-11-07.md        # Inventaris baseline (satu halaman, 16 produk)
│   ├── site-inventory-2026-11-20.md        # Inventaris versi terbaru (multi-halaman, 19 produk, login)
│   ├── repo-structure-2026-11-07.md        # Struktur repo source (7 Nov 2026, historis)
│   ├── white-box-findings-2026-11-07.md    # Temuan white-box 7 Nov 2026 (arsip, bukan pengujian berjalan)
│   ├── draft-jenis-dan-pendekatan-testing.md # Draf bagian laporan (14 Nov 2026, sudah kedaluwarsa)
│   └── pacing-plan.md                      # Rencana batch push (internal)
├── test-plan/
│   └── test-plan.md                        # Test plan (revisi 20 Nov 2026)
├── test-cases/
│   ├── test-cases-manual.md                # Test case manual (versi lama + 35 test case baru)
│   └── test-execution-log.md               # Log eksekusi test case
├── bug-reports/
│   └── bug-reports.md                      # Bug report (BUG-01 s.d. BUG-23)
├── references/
│   └── standards-journals.md               # Standar ISO/IEEE & jurnal 2022–2026
├── automation/
│   ├── README.md                           # Penjelasan test otomatis (Playwright/HTTP, versi lama)
│   ├── http-smoke.js                       # Test HTTP smoke (Node)
│   ├── package.json
│   ├── playwright.config.js
│   ├── smoke.spec.js
│   ├── results-http-smoke-2026-11-07.md
│   ├── results-http-smoke-2026-11-10.md
│   ├── cypress/                            # (direncanakan, Tahap 6)
│   ├── jmeter/                             # (direncanakan, Tahap 6)
│   └── security-dast/                      # (direncanakan, Tahap 6)
├── interview/
│   └── interview-guide.md                  # Panduan wawancara QA/tester ≥3 tahun
└── test-report/                             # (direncanakan, Tahap 5)
    └── test-report.md
```

Tautan langsung ke file-file utama:

- [Inventaris 7 Nov 2026](docs/site-inventory-2026-11-07.md) · [Inventaris 20 Nov 2026](docs/site-inventory-2026-11-20.md)
- [Test Plan](test-plan/test-plan.md)
- [Test Case Manual](test-cases/test-cases-manual.md) · [Log Eksekusi](test-cases/test-execution-log.md)
- [Bug Reports](bug-reports/bug-reports.md)
- [Standar & Jurnal](references/standards-journals.md)
- [Panduan Wawancara](interview/interview-guide.md)
- [Automation README](automation/README.md)

## 7. Menjalankan Test Otomatis

**HTTP smoke test (sudah jalan, perlu diulang untuk versi baru):**

```bash
cd automation
node http-smoke.js
```

**Playwright (arsip, instalasi browser gagal di mesin ini):**

```bash
cd automation
npm install
npx playwright install chromium
npm test
```

**Cypress, JMeter, dan OWASP ZAP (Tahap 6):** panduan instalasi dan eksekusi ada di folder masing-masing setelah Tahap 6 selesai.
