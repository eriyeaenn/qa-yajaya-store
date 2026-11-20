# Standar dan Jurnal Acuan — QA yajaya.store

Dokumen ini adalah daftar referensi formal laporan QA yajaya.store. Semua standar dan jurnal di bawah ini terbit di rentang **2022–2026**. Terakhir diverifikasi: 20 November 2026.

Cara mengecek setiap referensi: klik DOI atau tautan yang tercantum, lalu cocokkan judul, tahun, dan volume dengan sitasi di bawah. Kalau sebuah tautan tidak bisa dibuka, sitasi tetap bisa dilacak lewat DOI-nya.

## A. Standar ISO/IEEE (daftar formal, 2022–2026 saja)

| Standar | Tahun | Isi singkat | Dipakai di proyek ini untuk |
|---|---|---|---|
| ISO/IEC/IEEE 29119-1 | 2022 | Konsep dan istilah dasar pengujian perangkat lunak (edisi 2) | Istilah dan kerangka kerja pengujian di seluruh laporan |
| ISO/IEC/IEEE 29119-5 | 2024 | Keyword-driven testing | Pola otomatisasi Cypress (keyword-driven, Tahap 6) |
| ISO/IEC 25010 | 2023 | Model kualitas produk SQuaRE, 9 karakteristik kualitas | Menentukan karakteristik yang dinilai: functional suitability, interaction capability, reliability, performance efficiency, security |
| ISO/IEC 25002 | 2024 | Panduan penyusunan model kualitas | Menyusun kriteria penilaian kualitas dari 25010:2023 |
| ISO/IEC 25040 | 2024 | Proses evaluasi kualitas produk | Kerangka verdict akhir dan rekomendasi di test report |
| IEEE 1012 | 2024 | Verification & Validation (V&V) | Pendukung traceability: setiap temuan harus bisa dilacak ke kebutuhan atau perilaku yang diamati |

Sumber katalog resmi: <https://www.iso.org/contents/data/standard/08/12/81291.html>, <https://www.iso.org/standard/79428.html>, <https://webstore.iec.ch/en/publication/90024>, <https://standards.ieee.org/ieee/1012/7324/>.

Teks lengkap standar ISO dan IEEE itu berbayar. Yang dikutip di laporan ini hanya halaman katalog dan abstrak resminya. Jangan lampirkan PDF bajakan di repo ini.

### Catatan penting soal edisi 2021

ISO/IEC/IEEE 29119-2, 29119-3, dan 29119-4 edisi terkininya memang tahun **2021** dan masih berlaku sampai hari ini. Karena aturan tugas ini hanya menerima referensi 2022–2026, ketiga standar itu **tidak dimasukkan ke daftar referensi formal** di atas. Format test plan, test case, dan test report di repo ini mengikuti struktur dokumen yang dijelaskan standar-standar tersebut sebagai konvensi kerja, tetapi sitasi formalnya tidak ditulis sebagai referensi 2022–2026. Keputusan ini dicatat di sini supaya dosen bisa melihatnya secara terbuka, bukan disembunyikan.

Standar lama IEEE 829-2008 disebut di sini hanya sebagai catatan: standar itu sudah digantikan oleh seri ISO/IEC/IEEE 29119 dan tidak dipakai di proyek ini. Begitu juga ISO/IEC 25010:2011 dan IEEE 1012-2016, keduanya sudah digantikan edisi yang tercantum di tabel.

## B. Jurnal inti (2022–2026)

### B.1 Pengujian manual, white-box/black-box, dan UAT

**1. Safira, dkk. (2023).** Artikel tentang pengujian sistem web dengan kombinasi white-box, black-box, dan UAT. *Jurnal Teknologi Informasi dan Ilmu Komputer (JTIIK)*, 10(5), 983–992. DOI: [10.25126/jtiik.2023106891](https://doi.org/10.25126/jtiik.2023106891).

- Relevansi: menguji sistem web dengan kombinasi white-box, black-box, dan UAT, dan UAT-nya mencapai 94,67%. Polanya mirip dengan rencana pengujian kami: black-box sebagai pendekatan utama, UAT sebagai penutup dari sudut pandang pengguna.
- Verifikasi: DOI aktif dan mengarah ke artikel JTIIK 2023. Peringkat Sinta **belum terverifikasi** — jangan dikutip sebagai Sinta 2 sebelum dicek di portal Sinta.

**2. Chrisna, dkk. (2024).** Artikel tentang pengujian sistem web dengan white-box, black-box, dan otomatisasi Postman/Katalon. *Jurnal Teknik Informatika (JUTIF)*, 5(2), 551–560. DOI: [10.52436/1.jutif.2024.5.2.1673](https://doi.org/10.52436/1.jutif.2024.5.2.1673).

- Relevansi: **Sinta 2, sudah terverifikasi** di situs resmi jurnal. Memakai white-box dan black-box sekaligus, ditambah otomatisasi, dan hanya 1 dari 23 test case yang gagal. Ini alasan paling kuat kenapa kami membedakan pemakaian black-box dan white-box per level pengujian — dan kenapa, di proyek ini, white-box dinyatakan out-of-scope (lihat test plan), karena pengujian dilakukan murni dari sudut pandang pelanggan tanpa akses kode.
- Verifikasi: DOI aktif, peringkat Sinta 2 terverifikasi.

**3. Caesar, dkk. (2025).** Artikel tentang pengujian sistem POS berbasis web dengan ISO 29119 dan ISO 25010. *Journal of Information System and Business Technology (JISBT)*, 1(3), 19–25. Tautan: <https://journal.jci.co.id/jisbt/article/download/274/93>.

- Relevansi: paling mirip dengan tugas ini. Mereka menguji sistem POS berbasis web memakai proses ISO/IEC 29119 dan evaluasi ISO/IEC 25010, dengan 25 test case dan 6 bug, termasuk satu yang Critical. Jurnal ini dari Universitas Pamulang, kampus yang sama dengan penulis laporan ini.
- Verifikasi: tautan aktif dan mengarah ke PDF artikel JISBT 2025. Jurnal ini masih baru (volume 1), jadi peringkat Sintanya belum ditemukan — jangan diklaim.

### B.2 Manual vs otomatis, dan pemilihan tool otomatisasi

**4. Melyawati, dkk. (2024).** Artikel tentang perbandingan Playwright dan Selenium. *Jurnal Ilmu Komputer dan Informatika (CNAHPC)*, 6(3), 1309–1320. DOI: [10.47709/cnahpc.v6i3.4362](https://doi.org/10.47709/cnahpc.v6i3.4362).

- Relevansi: membandingkan Playwright dan Selenium dengan angka yang terukur — Playwright 4,9 menit, Selenium 8,3 menit, untuk 12 test case yang sama. Dipakai sebagai pembanding tool otomatisasi di laporan ini, meskipun tool E2E yang akhirnya dipilih adalah Cypress (lihat nomor 6 dan 7).
- Verifikasi: DOI aktif dan mengarah ke artikel 2024.

**5. Kurmaku, dkk. (2022).** "Meta-analisis 21 studi tentang manual vs automated testing". *Proceedings of the ACM/ISEC 2022*. DOI: [10.1145/3511430.3511433](https://doi.org/10.1145/3511430.3511433).

- Relevansi: meta-analisis 21 studi. Kesimpulannya, testing otomatis lebih hemat dari segi biaya, coverage, dan waktu, tetapi testing manual hasilnya sama bagusnya, bahkan sedikit lebih baik dalam menemukan fault, dan bedanya tidak signifikan secara statistik. Ini alasan kami memakai manual dan otomatis bersamaan, bukan memilih salah satu.
- Verifikasi: DOI aktif dan mengarah ke prosiding ACM 2022.

**6. García, B., del Álamo, J. M., Leotta, M., & Ricca, F. (2024).** "Exploring browser automation: A comparative study of Selenium, Cypress, Puppeteer, and Playwright". *Proceedings of QUATIC 2024, Communications in Computer and Information Science*, vol. 2178, Springer, pp. 137–152. DOI: [10.1007/978-3-031-70245-7_10](https://doi.org/10.1007/978-3-031-70245-7_10).

- Relevansi: studi banding empat tool otomatisasi browser, termasuk Cypress. Dipakai sebagai alasan pemilihan Cypress untuk E2E di proyek ini: Cypress dirancang khusus untuk pengujian end-to-end di browser, dengan sinkronisasi otomatis dan debugging yang lebih mudah dibanding Selenium, sehingga cocok untuk menguji alur pelanggan (login, navigasi, katalog, order) dari luar, tanpa menyentuh kode.
- Verifikasi: DOI aktif dan mengarah ke bab prosiding Springer QUATIC 2024.

**7. Gosik, P., & Miłosz, M. (2025).** "Comparative analysis of Cypress and Playwright frameworks in end-to-end testing for applications based on Angular". *Journal of Computer Sciences Institute*, 36, 320–327. DOI: [10.35784/jcsi.7662](https://doi.org/10.35784/jcsi.7662).

- Relevansi: membandingkan langsung Cypress dan Playwright pada pengujian E2E, termasuk kecepatan eksekusi, konsumsi memori, dan dukungan multi-browser. Hasilnya dipakai untuk menjelaskan trade-off pemilihan Cypress di proyek ini: Playwright lebih cepat dan lebih hemat memori, tetapi Cypress dipilih karena kemudahan penulisan dan debugging skenario alur pelanggan untuk tim kecil.
- Verifikasi: DOI aktif dan mengarah ke artikel JCSI 2025.

### B.3 Performance testing dengan Apache JMeter

**8. Indrianto, I. (2023).** "Performance Testing on Web Information System Using Apache JMeter and BlazeMeter". *Jurnal Ilmiah Ilmu Terapan Universitas Jambi*, 7(2), 138–149. DOI: [10.22437/jiituj.v7i2.28440](https://doi.org/10.22437/jiituj.v7i2.28440).

- Relevansi: menguji sistem informasi web dengan Apache JMeter dan BlazeMeter, memakai 50 dan 100 sampel dengan ramp-up 10 detik. Skenario load testing di proyek ini (50–100 user, ramp-up 10 detik) mengikuti pola yang sama, dan metrik yang dipakai juga sama: response time, throughput, dan error rate.
- Verifikasi: DOI aktif dan mengarah ke artikel 2023.

**9. Annisa, A., dkk. (2026).** "Performance Evaluation of Web Applications Using JMeter Load Testing for Server Capacity and Response Efficiency". *Journal of Applied Informatics and Computing*, 10(1), 585–590. DOI: [10.30871/jaic.v10i1.11840](https://doi.org/10.30871/jaic.v10i1.11840).

- Relevansi: load testing JMeter dengan skenario 50 sampai 5000 virtual user. Hasilnya, website yang diuji stabil sampai 200 user bersamaan, lalu response time naik tajam dan error mulai muncul di atas 300 user. Ini dipakai sebagai pembanding untuk menentukan ambang stress testing di proyek ini (200–500 user) dan target SLA response time.
- Verifikasi: DOI aktif dan mengarah ke artikel JAIC 2026.

### B.4 Security testing eksternal (DAST) dengan OWASP ZAP

**10. "Methods of Automated Web Application Security Testing Using OWASP Tools" (2026).** *SWorldJournal*, Issue 36, Part 2. DOI: [10.30888/2663-5712.2026-36-02-089](https://doi.org/10.30888/2663-5712.2026-36-02-089).

- Relevansi: membahas metode pengujian keamanan web otomatis memakai tool OWASP, termasuk DAST. DAST menguji aplikasi yang sedang berjalan dari luar — persis seperti sudut pandang pelanggan di proyek ini — dan efektif menemukan masalah konfigurasi, autentikasi, dan penanganan request, tetapi tidak bisa menunjukkan baris kode penyebabnya. Hasil penelitiannya: pemakaian tool otomatis mengurangi waktu pengujian 35–50% dan menaikkan tingkat deteksi kerentanan sampai 90%.
- Verifikasi: DOI aktif dan mengarah ke artikel SWorldJournal 2026. Sebagai panduan teknis tambahan (bukan jurnal), dipakai juga OWASP DevSecOps Guideline bagian DAST: <https://owasp.github.io/DevSecOpsGuideline/2-Process/2-4-Test/2-4-2-Dynamic-Application-Security-Testing/>.

## C. Jurnal pendukung

- **Hafiz, dkk. (2023).** *Jurnal RESTI*, Sinta 2 terverifikasi. DOI: [10.29207/resti.v7i3.4870](https://doi.org/10.29207/resti.v7i3.4870). Pendukung untuk pengujian sistem informasi web.
- **Gupta, A., & Bathla, G. (2022).** "Manual vs automated testing". *International Journal of Scientific Research in Science and Technology (IJSRST)*. DOI: [10.32628/IJSRST229657](https://doi.org/10.32628/IJSRST229657). Pendukung alasan memakai manual dan otomatis bersamaan.
- **Kumar, A., & Sagar, B. (2024).** "Grey-box testing". *International Journal of Intelligent Systems and Applications in Engineering (IJISAE)*. Tautan: <https://ijisae.org/index.php/IJISAE/article/view/7130>. Pendukung untuk pengujian berbasis pengetahuan sebagian tentang struktur aplikasi.
- **Jyolsna, J., & Anuar, S. (2022).** "Modern web automation with Cypress.io". *Open International Journal of Informatics*, 10(2), 182–196. DOI: [10.11113/oiji2022.10n2.229](https://doi.org/10.11113/oiji2022.10n2.229). Pendukung tambahan untuk pemilihan Cypress.

## D. Catatan akhir

1. **Tidak ditemukan jurnal Sinta 1** untuk topik ini di rentang 2022–2026. Yang terverifikasi kuat itu Sinta 2 (JUTIF 2024 dan RESTI 2023). Jangan klaim ada referensi Sinta 1 di laporan ini.
2. **ISO 25010 dan ISO 29119 itu standar, bukan jurnal.** Di laporan, keduanya ditulis di bagian standar, bukan di bagian jurnal.
3. **Semua referensi di atas terbit 2022–2026.** Referensi di luar rentang itu sengaja tidak dimasukkan, kecuali IEEE 829-2008 yang hanya disebut sekilas di bagian A sebagai standar lama yang sudah digantikan.
