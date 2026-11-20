# Test Plan — Pengujian Website yajaya.store
Mengikuti ISO/IEC/IEEE 29119-3:2021 (Test Documentation) dan ISO/IEC/IEEE 29119-2:2021 (Test Processes).
Penilaian kualitas mengikuti ISO/IEC 25010:2023. Evaluasi akhir mengikuti ISO/IEC 25040:2024.

## 1. Pendahuluan
- **Objek uji:** https://yajaya.store, website katalog kaos polos & sablon DTF satuan, UMKM di Parung, Bogor.
- **Jenis usaha:** menengah ke bawah (UMKM, order via WhatsApp, tanpa cart/checkout/pembayaran on-site).
- **Tujuan:** memverifikasi fungsi, kesesuaian konten/harga, dan kualitas interaksi website dari sudut pengguna, serta mendokumentasikan defect yang ditemukan.
- **Riwayat pengujian:** inventaris pertama 7 Nov 2026 (website satu halaman, 16 produk, tanpa login). Website lalu diperbarui pemiliknya dan dicek ulang 20 Nov 2026 (multi-halaman, 19 produk, login Google). Test plan ini direvisi 20 Nov 2026 mengikuti kondisi terbaru. Inventaris lama tetap disimpan di `docs/site-inventory-2026-11-07.md` sebagai jejak, dan inventaris baru ada di `docs/site-inventory-2026-11-20.md`.
- **Dokumen acuan:** lihat `references/standards-journals.md`.

## 2. Ruang lingkup
### 2.1 Yang diuji (test items)
- Halaman Home, Katalog, Sablon, Galeri, Blog, Tentang, Kontak, Login, Register, Dashboard (terbatas, lihat 2.3), dan 19 halaman detail produk.
- Login dan register Google-only: tombol Google, pengalihan halaman login untuk rute yang terkunci, halaman register dan janji fiturnya.
- Katalog 19 produk: pencarian, filter kategori, sortir harga, harga jual dan harga coret, badge diskon.
- Halaman detail produk: pilihan warna dan ukuran, jumlah, tombol Pesan Sekarang, link Beli di Shopee.
- Kalkulator DTF dan tabel pricelist di /sablon.
- Galeri: filter kategori dan lightbox.
- Blog: daftar artikel dan halaman artikel.
- Link keluar: WhatsApp (wa.me/628568703266), Instagram (@yajaya.id), TikTok (@yajaya.store), Shopee (shopee.co.id/yajaya.id) — diperiksa href-nya saja, tidak dikirim atau dibuka percakapannya.
- Konsistensi konten: nama produk, harga di kartu vs halaman detail vs kategori Home, tanggal artikel blog, teks footer, judul halaman.

### 2.2 Yang TIDAK diuji (tidak ada di website)
Cart, checkout, pembayaran on-site, form order/kontak, validasi form email/password, ongkir, lacak pesanan, wishlist, halaman About/Privacy/Terms/Shipping/Returns, newsletter.
Alasan: fitur tersebut tidak ada di situs (bukan gagal diuji). Dicatat sebagai keterbatasan produk, bukan defect, kecuali klaim di situs menyebutkannya (misalnya kartu Polo Shirt yang mengarah ke halaman order).

### 2.3 Yang diuji TERBATAS — butuh login Google asli
Dashboard (/dashboard), form order (/dashboard/pesanan/baru), tracking pesanan, dan download/cetak invoice tidak bisa diuji tanpa akun Google asli, karena login di situs ini hanya lewat Google dan tidak ada akun demo. Statusnya: diblokir, bukan gagal. Kalau pemilik situs menyediakan akun tes, bagian ini diuji terpisah dan test plan diperbarui.

## 3. Pendekatan & teknik (ISO/IEC/IEEE 29119-4:2021)
| Keluarga teknik | Teknik | Dipakai untuk |
|---|---|---|
| Specification-based (black-box) | Equivalence Partitioning, Boundary Value Analysis, Decision Table, State Transition | Search, filter kategori, sortir harga, kalkulator DTF, alur login Google, rute terkunci |
| Structure-based (white-box) | Statement & branch/decision coverage | **TERBATAS / BERSYARAT:** hanya bisa penuh bila ada akses source code/repo. Repo yajaya-web sudah dibaca 17 Nov 2026 (read-only), tapi kodenya sudah berubah lagi sejak itu, jadi temuan white-box lama tidak dipakai untuk versi baru tanpa pengecekan ulang. |
| Experience-based | Error guessing, exploratory testing | Filter dan lightbox galeri, harga kategori Home, pencarian substring, inkonsistensi konten |
| Grey-box (gabungan) | Black-box + pengetahuan struktur client-side (Next.js, data produk di client) | Harga coret dan diskon, data produk terkait, mapping pencarian |

**Manual + otomatis (alasan jurnal):** Kurmaku dkk. (ACM/ISEC 2022) — meta-analisis 21 studi: otomatis lebih hemat biaya/coverage/waktu, manual sama/lebih baik dalam deteksi fault, bedanya tidak signifikan. Jadi dipakai keduanya, bukan salah satu. Gupta & Bathla (IJSRST 2022): keduanya komplementer.
**Tool otomatis:** Playwright (bukan Selenium) — Melyawati dkk. (2024): Playwright 4,9 menit vs Selenium 8,3 menit untuk 12 test case yang sama. Pola keyword-driven mengikuti ISO/IEC/IEEE 29119-5:2024. Catatan: instalasi browser Playwright gagal 3x di mesin ini tanggal 7 Nov 2026 karena download dari CDN putus di sekitar 90 persen. Test otomatis level HTTP pakai Node sudah jalan (5 dari 5 lolos) dan perlu dijalankan ulang untuk versi baru, termasuk /sitemap.xml dan /robots.txt.

## 4. Karakteristik kualitas yang dinilai (ISO/IEC 25010:2023)
Prioritas: Functional Suitability, Interaction Capability, Reliability, Performance Efficiency, Security (hanya sebatas login Google dan link yang tampil — tidak ada pembayaran on-site untuk diuji pentest).
Tidak diprioritaskan: Maintainability, Flexibility, Safety (butuh akses kode/infrastruktur).

## 5. Kriteria masuk / keluar (29119-2)
- Masuk: situs dapat diakses (HTTP 200), inventaris versi terbaru selesai (20 Nov 2026).
- Keluar: semua test case manual prioritas Tinggi dijalankan, semua test otomatis smoke pass/fail terdokumentasi, bug report berisi langkah reproduksi + bukti, Test Completion Report berisi verdict.
- Verdict akhir memakai kerangka ISO/IEC 25040:2024 — bukan "lulus/gagal" mutlak, tapi rekomendasi per karakteristik + daftar perbaikan prioritas.

## 6. Risiko & asumsi
1. Tanpa akses source code versi terbaru, white-box tidak bisa diukur coverage-nya. Repo https://github.com/opikbtk/yajaya-web dicek terakhir 17 Nov 2026 — kodenya sudah berubah lagi sejak itu (login sekarang Google-only, bukan localStorage lagi).
2. Dashboard tidak bisa diuji tanpa akun Google asli. Ini keterbatasan terbesar di versi baru.
3. Website masih bisa berubah lagi selama pemiliknya mengoprek — inventaris 20 Nov 2026 adalah patokan, dan setiap perubahan setelahnya harus dicek ulang sebelum laporan final.
4. Statistik 5.0 dari 29 ulasan Google dan jumlah terjual di kartu produk adalah klaim halaman, belum terverifikasi dari sumber independen.
5. Percakapan WA, pengiriman, dan pembayaran berada di luar situs — tidak diuji.
6. Pengujian baru hanya desktop; mobile/responsif masih harus diuji terpisah.

## 7. Jadwal (draf, sesuaikan deadline dosen)
1. Inventaris & revisi test plan — 20 Nov 2026 (selesai)
2. Test case manual baru (login/auth, galeri, blog, sortir, kalkulator) + eksekusi — setelah test plan ini
3. Test otomatis Playwright (smoke + regresi) + HTTP smoke ulang — setelah test case manual
4. Wawancara QA/tester berpengalaman ≥3 tahun — paralel, panduan di `interview/`
5. Bug report + Test Completion Report + push GitHub 1 direktori — akhir, setelah user bilang lanjut (aturan tugas kelompok: tidak ada push tiba-tiba)

## 8. Deliverable (struktur GitHub, 1 direktori)
```
qa-yajaya-store/
├── README.md
├── docs/site-inventory-2026-11-07.md
├── docs/site-inventory-2026-11-20.md
├── test-plan/test-plan.md
├── test-cases/test-cases-manual.md
├── automation/ (Playwright + HTTP smoke)
├── bug-reports/bug-reports.md
├── interview/interview-guide.md + interview-transcript.md
└── references/standards-journals.md
```
