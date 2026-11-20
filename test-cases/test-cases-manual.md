# Test Case Manual — yajaya.store
Acuan teknik: ISO/IEC/IEEE 29119-4:2021. Format dokumen: ISO/IEC/IEEE 29119-3:2021.

## Catatan versi
Bagian A sampai G di bawah ini dibuat untuk website versi 7 Nov 2026 (satu halaman, 16 produk, tanpa login). Website sudah berubah total dan bagian-bagian itu sudah tidak berlaku lagi untuk versi baru, tapi tetap disimpan sebagai jejak. Test case untuk versi terbaru (20 Nov 2026) ada di bagian H sampai L di bawah, dan itulah yang harus dieksekusi sekarang.

Legenda Prioritas: T=Tinggi, S=Sedang, R=Rendah. Severity bug: Critical/Major/Minor/Cosmetic.
Teknik: BB=black-box (EP/BVA/Decision Table/State Transition), EB=experience-based, GB=grey-box.

Legenda Prioritas: T=Tinggi, S=Sedang, R=Rendah. Severity bug: Critical/Major/Minor/Cosmetic.
Teknik: BB=black-box (EP/BVA/Decision Table/State Transition), EB=experience-based, GB=grey-box.

## A. Pencarian produk (BB + EB)
| ID | Judul | Langkah | Hasil yang diharapkan | Teknik | Prioritas | Hasil awal 7 Nov |
|---|---|---|---|---|---|---|
| TC-S01 | Search "hoodie" | Ketik hoodie di search header | 1 produk, heading "Hasil pencarian hoodie 1 Produk" | BB-EP | T | PASS |
| TC-S02 | Search "kaos" | Ketik kaos | Menampilkan produk kaos yang relevan, jumlah konsisten | BB-EP | T | Belum |
| TC-S03 | Search kosong / tidak ada "xyz123" | Ketik xyz123, tunggu | "Produk tidak ditemukan", saran + Reset Filter + Rekomendasi Terlaris | BB-EP/EB | T | PASS (pesan muncul) |
| TC-S04 | Search case-insensitive "HOODIE" | Ketik HOODIE | Hasil sama seperti "hoodie" | BB-BVA/EP | S | Belum |
| TC-S05 | Search parsial "kao" / "sabl" | Ketik parsial | Autocomplete menampilkan saran relevan | BB-EP | S | Belum |
| TC-S06 | Autocomplete klik saran | Ketik hoodie, klik saran dropdown | Grid terfilter ke produk yang dipilih | BB | S | Belum |
| TC-S07 | Reset Filter dari state kosong | Cari xyz123 → klik Reset Filter | Kembali ke 16 produk, teks search terhapus, kategori kembali Semua | EB | T | **FAIL — kandidat BUG-01:** klik 2x tetap "xyz123" + Outerwear aktif, 0 produk |
| TC-S08 | Hapus Semua (chip filter) | Aktifkan filter, klik Hapus Semua | Kembali 16 produk | BB | S | PASS |

## B. Filter kategori & lanjutan (BB — Decision Table + EP)
| ID | Judul | Langkah | Hasil yang diharapkan | Teknik | Prioritas | Hasil awal |
|---|---|---|---|---|---|---|
| TC-F01 | Kategori Semua | Klik Semua Produk | 16 produk | BB-EP | T | PASS |
| TC-F02 | Kategori Kaos Polos / Outerwear / Jeans / Sablon / Paket | Klik tiap kategori | Jumlah 3 / 10 / 1 / 1 / 1, produk sesuai | BB-EP | T | PASS (jumlah terverifikasi) |
| TC-F03 | Filter harga <Rp50.000 | Filter Lanjutan → <Rp50.000 | 2 produk: 30s Rp40.000, Sablon Rp10.000 | BB-EP | T | PASS |
| TC-F04 | Filter harga Rp50-100rb / >Rp100rb | Pilih tiap rentang | Hanya produk dalam rentang itu | BB-BVA | T | Belum |
| TC-F05 | Filter warna Hitam + Semua | Warna Hitam | Semua produk yang punya Hitam/Black muncul | BB-EP | T | **FAIL — kandidat BUG-02:** hanya 4 produk |
| TC-F06 | Filter warna Hitam + Kaos Polos | Kategori Kaos Polos + Hitam | 3 produk kaos (ketiganya punya Hitam/Black) | BB-EP/GB | T | **FAIL — kandidat BUG-02:** 0 produk |
| TC-F07 | Filter warna Putih + Kaos Polos | Putih + Kaos Polos | Produk kaos yang punya White/Putih | BB-EP/GB | S | Belum — diduga gagal sama seperti Hitam |
| TC-F08 | Filter ukuran S / 5XL | Pilih ukuran | Hanya produk yang punya ukuran itu | BB-EP | S | Belum |
| TC-F09 | Filter tipe Kaos Polos / Sablon DTF | Pilih tipe | Hasil sesuai tipe | BB-EP | S | Belum |
| TC-F10 | Kombinasi filter + chip aktif | Aktifkan warna+ukuran+harga | Chip tampil, jumlah konsisten, Hapus Semua mengembalikan 16 | BB-DT | S | Belum |
| TC-F11 | Sort (negatif) | Cari kontrol sort harga/nama/popularitas | — | BB | R | **TIDAK ADA** — dicatat sebagai keterbatasan fitur, bukan bug |

## C. Quick View modal (BB — BVA + State Transition)
| ID | Judul | Langkah | Hasil yang diharapkan | Teknik | Prioritas | Hasil awal |
|---|---|---|---|---|---|---|
| TC-Q01 | Buka Quick View 30s | Klik QUICK VIEW produk 1 | Modal terbuka: gambar, rating, deskripsi, warna, ukuran, jumlah, Total Harga | BB | T | PASS |
| TC-Q02 | Pilih ukuran XXL (30s) | Pilih XXL | Harga berubah Rp40.000 → Rp45.000 | BB-BVA | T | PASS |
| TC-Q03 | Jumlah 2 (XXL) | Stepper + sampai 2 | Total Harga Rp90.000 | BB-BVA | T | PASS |
| TC-Q04 | Jumlah 0 / negatif | Coba kurangi di bawah 1 | Jumlah minimal 1, tidak 0/negatif | BB-BVA/EB | S | Belum |
| TC-Q05 | Harga coret XXL | Pilih XXL, lihat harga coret | Harga coret > harga saat ini, atau tidak tampil bila sama | BB/EB | S | **FAIL — kandidat BUG-06:** keduanya Rp45.000 |
| TC-Q06 | Panduan Ukuran 30s | Buka Panduan Ukuran | Tabel S 50x70 … 5XL 68x84, konsisten dengan ukuran yang ditawarkan | BB | S | PASS (tabel terverifikasi) |
| TC-Q07 | Spesifikasi Sablon DTF | Buka Quick View Sablon | Spesifikasi sesuai jasa sablon, bukan kaos | EB/GB | T | **FAIL — kandidat BUG-05:** tertulis Cotton Combed 30s 150gr |
| TC-Q08 | Spesifikasi Overshirt | Buka Quick View Overshirt | Bahan sesuai overshirt (bukan Combed 30s), alamat kirim = Bogor | EB/GB | T | **FAIL — kandidat BUG-05:** tertulis Palmerah, Jakarta Barat |
| TC-Q09 | Swatch Cobalt Blue | Buka Quick View Overshirt Cobalt | Swatch berwarna biru cobalt | EB | S | **FAIL — kandidat BUG-08:** tampil abu-abu |
| TC-Q10 | Harga produk terkait | Buka Quick View Overshirt → lihat terkait | Harga terkait = harga kartu produk itu | BB/EB | S | **FAIL — kandidat BUG-07:** Brown Rp200.000 vs kartu Rp209.000 |
| TC-Q11 | Pesan WA dari kartu produk | Klik Pesan WA (jangan kirim) | Teks prefilled hanya varian yang dipilih + harga — periksa href | EB | T | **FAIL (desain) — kandidat BUG:** teks mencantumkan SEMUA ukuran & warna, tanpa harga |
| TC-Q12 | Tutup modal | Klik X / luar modal / Esc | Modal tertutup, halaman kembali ke katalog | BB-ST | S | Belum |

## D. Pricelist, FAQ, navigasi, tema (BB + EB)
| ID | Judul | Langkah | Hasil yang diharapkan | Teknik | Prioritas | Hasil awal |
|---|---|---|---|---|---|---|
| TC-P01 | Pricelist Sablon | Buka #harga | Logo 10rb, A6+ 15rb, A5 20rb, A4 25rb, A3 40rb | BB | T | PASS |
| TC-P02 | Pricelist Kaos Pendek/Panjang | Buka #harga | Harga per ukuran sesuai (Pendek S-XL 40rb … 5XL 60rb; Panjang 50-70rb) | BB | T | PASS |
| TC-P03 | Item pricelist tanpa jalur beli | Cari Kaos Panjang & Sablon A6+ di katalog | Ada produk / tombol beli yang sesuai | EB | S | **FAIL — kandidat BUG-09:** tidak ada di katalog |
| TC-N01 | Anchor #harga dari hero | Klik Lihat Pricelist | Mendarat di "Daftar Harga Terbaik" | BB | T | PASS |
| TC-N02 | Anchor #pricelist dari footer | Klik Daftar Harga di footer | Mendarat di pricelist | BB | T | **FAIL — kandidat BUG-03:** tidak mendarat |
| TC-N03 | Heading tidak tertutup sticky bar | Lompat ke #harga / #faq | Heading terlihat penuh | EB | S | **FAIL — kandidat BUG-04:** heading terpotong sticky bar |
| TC-FQ01 | FAQ accordion | Klik tiap pertanyaan | Hanya satu terbuka, jawaban lengkap | BB-ST | T | PASS (1-4 lengkap) |
| TC-FQ02 | FAQ search "sablon" | Ketik sablon di search FAQ | 5 → 3 item | BB-EP | S | PASS |
| TC-FQ03 | FAQ #5 lengkap | Buka pertanyaan ketebalan kaos | Jawaban lengkap menyebut 30s/24s/20s | BB | S | Belum — jawaban terpotong saat inventaris |
| TC-T01 | Toggle tema | Klik toggle gelap/terang | Tema berubah, tetap bisa dibaca | BB | S | PASS |
| TC-T02 | Popup support | Load halaman → tutup popup | Popup muncul, tombol tutup berfungsi, tombol chat WA tetap ada | BB | S | PASS |

## E. Konten & konsistensi (EB)
| ID | Judul | Langkah | Hasil yang diharapkan | Teknik | Prioritas | Hasil awal |
|---|---|---|---|---|---|---|
| TC-C01 | Testimoni vs katalog | Baca 6 testimoni, cocokkan produk yang dibeli | Produk yang disebut ada di katalog 16 produk | EB | S | **FAIL — kandidat BUG-10:** Maya Putri beli "Kaos Oversize Pendek" (tidak ada) |
| TC-C02 | Penamaan warna konsisten | Bandingkan warna di semua produk | Satu bahasa / mapping konsisten (Hitam=Black) | EB/GB | S | **FAIL — kandidat BUG-10:** campur Indonesia/Inggris |
| TC-C03 | Deskripsi Overshirt | Baca deskripsi Overshirt | Bahasa Indonesia, konsisten dengan situs | EB | R | **FAIL — kandidat BUG-08:** deskripsi Inggris |
| TC-C04 | Klaim footer "checkout" | Baca footer | Tidak mengklaim fitur yang tidak ada | EB | R | **FAIL — kandidat BUG-10:** "Made for premium streetwear checkout" padahal tanpa checkout |
| TC-C05 | Link sosial | Periksa href WA/IG/TikTok | WA 628568703266, IG yajaya.id, TikTok @yajaya.store | BB | T | PASS (href saja, tidak dibuka) |
| TC-C06 | Link Shopee | Cari link toko Shopee | Ada / tidak ada — catat | BB | S | **TIDAK ADA** di situs |

## F. White-box — BERSYARAT (tanpa akses kode: TIDAK DAPAT DIEKSEKUSI)
| ID | Judul | Keterangan |
|---|---|---|
| TC-W01 | Statement coverage — fungsi filter/search | Butuh source code (Next.js). Status: BLOCKED sampai repo dikonfirmasi. |
| TC-W02 | Branch coverage — logika harga Quick View (ukuran → harga, jumlah → total) | Butuh source code. Dugaan bug BUG-06/07 kemungkinan dari branch harga coret / data terkait — tidak bisa dibuktikan tanpa kode. |
| TC-W03 | Path coverage — alur Quick View → WA | Butuh source code. |
| Jika tidak ada akses kode, ganti dengan: grey-box — periksa data produk yang dikirim ke client (JSON/props Next.js), mapping warna, dan data harga terkait — dan tulis keterbatasan ini di laporan. |

## G. Performa & aksesibilitas (ringkas, ISO 25010)
| ID | Judul | Harapan |
|---|---|---|
| TC-PF01 | Waktu render grid setelah search/filter | Skeleton 2-3 dtk tercatat (kandidat BUG-11, Minor). Ukur ulang di mobile + desktop. |
| TC-PF02 | Load homepage | HTTP 200, <5 dtk di desktop (terukur 1,79 dtk via curl 7 Nov). |
| TC-AX01 | Responsif mobile | BELUM DIUJI — wajib diuji terpisah (viewport 360px, 768px). |

Total versi lama: 40+ test case manual (A 8 + B 11 + C 12 + D 11 + E 6 + F 3 + G 3). Bagian ini sudah tidak dieksekusi lagi untuk versi baru.

## H. Login & register Google-only — versi 20 Nov 2026 (BB + EB)
| ID | Judul | Langkah | Hasil yang diharapkan | Teknik | Prioritas | Hasil awal 20 Nov |
|---|---|---|---|---|---|---|
| TC-L01 | Halaman login | Buka /login | Judul "Masuk ke Akun Anda", tombol "Lanjutkan dengan Google" | BB | T | PASS |
| TC-L02 | Halaman register | Buka /register | Judul "Daftar Akun Baru", tombol "Daftar Cepat dengan Google", 3 keuntungan tertulis | BB | T | PASS |
| TC-L03 | Tidak ada kolom email/password | Periksa /login dan /register | Tidak ada kolom isian, tidak ada lupa password | BB | T | PASS — memang tidak ada kolomnya |
| TC-L04 | Tidak ada akun demo | Cari "demo" di /login dan /register | Tidak ada akun demo yang ditampilkan | BB | T | PASS — tidak ada |
| TC-L05 | Rute terkunci /dashboard | Buka /dashboard tanpa login | Dilempar ke /login setelah spinner "Memuat portal dashboard…" | BB-ST | T | PASS |
| TC-L06 | Rute terkunci /dashboard/pesanan/baru | Buka /dashboard/pesanan/baru tanpa login | Pesan "Sesi tidak aktif. Silakan masuk untuk mengakses halaman ini." lalu ke /login | BB-ST | T | PASS |
| TC-L07 | Login Google (butuh akun asli) | Klik "Lanjutkan dengan Google" | Dialihkan ke Google, setelah berhasil kembali ke situs sebagai pengguna login | BB-ST | T | BLOCKED — butuh akun Google asli |
| TC-L08 | Dashboard setelah login | Login, buka /dashboard | Dashboard tampil, ada tracking pesanan dan invoice | BB | T | BLOCKED — butuh akun Google asli |
| TC-L09 | Beda admin dan customer | Bandingkan dashboard admin dan customer | Tampilan dan menu berbeda sesuai peran | BB | S | BLOCKED — butuh akun Google asli |

## I. Katalog & halaman detail produk — versi 20 Nov 2026 (BB + EB)
| ID | Judul | Langkah | Hasil yang diharapkan | Teknik | Prioritas | Hasil awal 20 Nov |
|---|---|---|---|---|---|---|
| TC-K01 | Jumlah produk | Buka /katalog | 19 produk | BB-EP | T | PASS |
| TC-K02 | Harga dan diskon | Periksa semua kartu | Harga jual, harga coret, dan persen diskon konsisten (dihitung ulang) | BB/GB | T | PASS — persen diskon cocok semua |
| TC-K03 | Search "denim" | Ketik denim di search katalog | Hanya Celana Denim | BB-EP | T | PASS |
| TC-K04 | Search "polo" | Ketik polo di search katalog | Tidak ada kaos polo; kalau menampilkan Kaos Polos, itu karena substring | BB-EP/EB | S | **FAIL — kandidat BUG-18:** hasilnya 2 produk Kaos Polos |
| TC-K05 | Filter kategori Hoodie | Klik kategori Hoodie | 1 produk | BB-EP | T | PASS |
| TC-K06 | Sortir harga rendah ke tinggi | Pilih "Harga: Rendah ke Tinggi" | Urutan Rp10.000, Rp40.000, Rp50.000, Rp100.000… | BB | S | PASS |
| TC-K07 | Sortir Populer/Terbaru/Terlaris | Klik tiap tombol sortir | Urutan berubah sesuai kriteria | BB | S | Belum |
| TC-K08 | Detail kaos-30s | Buka /produk/kaos-30s | 26 warna, ukuran S–XXXL, Rp40.000, rating 4.9, 842 penilaian, 1.2rb terjual | BB | T | PASS |
| TC-K09 | Detail hoodie | Buka /produk/hoodie | 5 warna, ukuran M–XXXL (tanpa S), Rp125.000, rating 4.9, 238 penilaian, 610 terjual | BB | T | PASS |
| TC-K10 | Tombol Pesan Sekarang | Periksa href tombol di halaman produk | Mengarah ke wa.me/628568703266 (jangan dikirim) | BB | T | PASS |
| TC-K11 | Link Beli di Shopee | Periksa link di halaman produk | Mengarah ke shopee.co.id/yajaya.id | BB | S | PASS |
| TC-K12 | Gambar produk | Periksa gambar Hoodie dan Sablon DTF | Gambar sesuai bentuk produk, bukan ilustrasi generik | EB | S | **FAIL — kandidat BUG-19:** gambar Hoodie kaos lengan pendek berkerudung |

## J. Sablon, galeri, blog — versi 20 Nov 2026 (BB + EB)
| ID | Judul | Langkah | Hasil yang diharapkan | Teknik | Prioritas | Hasil awal 20 Nov |
|---|---|---|---|---|---|---|
| TC-SB01 | Kalkulator DTF Logo x3 | /sablon, jumlah 3, ukuran Logo | Total Rp30.000 | BB-BVA | T | PASS |
| TC-SB02 | Kalkulator DTF A4 x3 | /sablon, jumlah 3, ukuran A4 | Total Rp75.000 | BB-BVA | T | PASS |
| TC-SB03 | Tabel pricelist DTF | Periksa tabel di /sablon | Logo 10rb, A6+ 15rb, A5 20rb, A4 25rb, A3 40rb | BB | T | PASS |
| TC-G01 | Filter galeri Komunitas | /galeri, klik Komunitas | Grid hanya menampilkan item Komunitas | BB-EP | T | **FAIL — kandidat BUG-14:** malah membuka lightbox |
| TC-G02 | Filter galeri Sekolah | /galeri, klik Sekolah | Grid hanya menampilkan item Sekolah | BB-EP | T | **FAIL — kandidat BUG-14:** membuka lightbox Merchandise Event |
| TC-G03 | Lightbox galeri | Buka lightbox Sablon Plastisol | Overlay menutupi halaman, caption sesuai gambar | BB/EB | T | **FAIL — kandidat BUG-15:** overlay tembus, caption salah |
| TC-G04 | Tombol reset filter galeri | Cari tombol Semua di /galeri | Ada tombol reset ke semua item | BB | S | **FAIL — kandidat BUG-14:** tidak ada tombol reset |
| TC-B01 | Daftar blog | Buka /blog | 4 artikel, ada estimasi waktu baca | BB | S | PASS |
| TC-B02 | Tanggal artikel blog | Buka satu artikel | Tanggal terbit ditampilkan dan masih relevan | EB | S | **FAIL — kandidat BUG-20:** artikel dari 2024 |

## K. Navigasi & konten — versi 20 Nov 2026 (BB + EB)
| ID | Judul | Langkah | Hasil yang diharapkan | Teknik | Prioritas | Hasil awal 20 Nov |
|---|---|---|---|---|---|---|
| TC-NV01 | Kategori Polo Shirt di Home | Klik kartu Polo Shirt di Home | Mengarah ke katalog atau produk polo | BB | T | **FAIL — kandidat BUG-16:** mengarah ke /dashboard/pesanan/baru lalu /login, produk polo tidak ada |
| TC-NV02 | Harga kategori Hoodie & Jacket | Baca kartu di Home | "Mulai Rp155.000" cocok dengan produk termurah kategori itu | EB | S | **FAIL — kandidat BUG-17:** hoodie Rp125.000, jaket termurah Rp300.000 |
| TC-NV03 | Judul halaman katalog | Periksa title tag /katalog | Nama toko cukup sekali | EB | R | **FAIL — kandidat BUG-21:** "… | YAJAYA STORE | YAJAYA STORE" |
| TC-NV04 | Footer | Baca footer di semua halaman | "© 2026 YAJAYA STORE" | EB | R | **FAIL — kandidat BUG-22:** tertulis "©2026YAJAYA STORE" |
| TC-NV05 | Peta di /kontak | Buka /kontak | Peta lokasi toko tampil | BB | S | **FAIL (kandidat) — BUG-23:** iframe tampil kosong |
| TC-NV06 | Rute /cart, /checkout, /admin | Buka ketiganya | 404 yang jelas, atau dialihkan ke halaman yang ada | BB | S | PASS — ketiganya 404 Next.js |
| TC-NV07 | Sitemap | Buka /sitemap.xml | Ada, memuat 7 halaman utama, 19 produk, 4 blog | BB | S | PASS — lastmod 20 Nov 2026 |

## L. Performa & aksesibilitas — versi 20 Nov 2026 (ringkas, ISO 25010)
| ID | Judul | Harapan |
|---|---|---|
| TC-PF03 | Load hero Home | Foto hero tampil cepat, tidak kosong gelap lama (kandidat BUG-11 lanjutan) |
| TC-PF04 | Load homepage | HTTP 200, <5 dtk di desktop |
| TC-AX02 | Responsif mobile | BELUM DIUJI — wajib diuji terpisah (viewport 360px, 768px) |

Total versi baru: 35 test case (H 9 + I 12 + J 9 + K 7 + L 3, sebagian sudah dieksekusi saat inventaris 20 Nov 2026). Jalankan ulang formal, isi Hasil Aktual + Status (Pass/Fail/Blocked) + ID bug bila Fail.
