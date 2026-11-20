# Bug Reports — yajaya.store
Format mengikuti ISO/IEC/IEEE 29119-3:2021 (Test Incident Report). Setiap bug perlu reproduksi ulang formal + screenshot/bukti sebelum final.

## Status setelah pengecekan ulang 20 Nov 2026
Website berubah total sejak 7 Nov 2026 (sekarang multi-halaman, 19 produk, ada login Google). Nasib bug lama:
- **Sudah tidak relevan karena fiturnya dihapus:** BUG-01 (reset filter), BUG-02 (filter warna), BUG-12 (filter tipe), BUG-05, BUG-06, BUG-07, BUG-08 (semuanya soal Quick View, fitur ini sudah tidak ada), BUG-09 (pricelist lama), BUG-03 dan BUG-04 (anchor dan FAQ, keduanya sudah tidak ada). Bug-bug ini tetap dicatat di bawah sebagai jejak versi lama, jangan dihapus, tapi jangan ditulis di laporan akhir seolah masih berlaku.
- **Sudah beres:** BUG-13 sebagian — sitemap.xml sekarang ada (lastmod 20 Nov 2026). robots.txt belum dicek ulang.
- **Masih perlu dicek ulang:** BUG-10 (konsistensi konten, sebagian: testimoni dan klaim footer belum dicek ulang di versi baru) dan BUG-11 (render lambat, hero sekarang yang lambat).
- **Bug baru versi 20 Nov 2026:** BUG-14 sampai BUG-23 di bawah.

## BUG-01 — Reset Filter tidak berfungsi di state pencarian kosong | Major
- Test case: TC-S07. Langkah: cari "xyz123" → klik Reset Filter 2x.
- Aktual: teks "xyz123" tetap ada, kategori Outerwear tetap aktif, tetap 0 produk.
- Harapan: kembali ke 16 produk, teks terhapus, kategori Semua.
- Catatan: "Hapus Semua" di chip filter berfungsi — jadi reset di state kosong saja yang rusak.
- ROOT CAUSE (white-box 7 Nov 13:21, Products.tsx): `handleClearAll` hanya mereset 4 state lokal (warna/ukuran/harga/tipe). searchQuery & activeCategory milik parent (app/page.tsx) dan tidak ada setter-nya di props, jadi tidak bisa direset dari komponen ini. Tombol reset juga hanya tampil bila salah satu dari 4 filter lokal aktif.

## BUG-02 — Filter warna tidak berfungsi untuk Kaos Polos (semua warna = 0) | Major
- Test case: TC-F05, TC-F06, TC-F07. Langkah: Warna Hitam / Putih + Kaos Polos.
- Aktual: 0 produk untuk warna apa pun di Kaos Polos (padahal 30s Quick View menawarkan Hitam, Navy, Maroon, Abu-abu, Putih dan kartunya klaim 17 WARNA). Hitam + Semua Produk hanya 4 produk; Putih + Semua Produk = 1 (Paket Kaos + Sablon, padahal gambarnya cream/beige bukan putih).
- ROOT CAUSE (white-box 7 Nov 13:21, Products.tsx + data/products.ts): filter memakai `p.colors.includes(selectedColor)` — exact & case-sensitive — sedangkan data warna campur ID/EN (kaos pakai "Black"/"Black Solid"/"White", filter menawarkan "Hitam"/"Putih"). Hanya id 4, 5, 9, 12 yang mengandung "Hitam". "Abu-abu"/"Cream"/"Sage"/"Mustard" tidak cocok apa pun; "Navy" melewatkan "Navy Blue". Tidak ada normalisasi/mapping di mana pun.
- Catatan tambahan: warna single-select; "Hapus Semua" tidak mereset kategori.

## BUG-03 — Link footer #pricelist mati | Minor
- Test case: TC-N02. Footer "Daftar Harga" → #pricelist tidak mendarat. Hero "Lihat Pricelist" → #harga berfungsi.

## BUG-04 — Sticky bar menutupi heading anchor | Minor
- Test case: TC-N03. Heading "Daftar Harga Terbaik" & "Pertanyaan Umum" terpotong saat lompat anchor.

## BUG-05 — Spesifikasi Quick View generik & alamat kirim salah | Major
- Test case: TC-Q07, TC-Q08. Sablon DTF & Overshirt tertulis "Cotton Combed 30s, 150 gram" dan "Dikirim dari Palmerah, Jakarta Barat".
- Harapan: spesifikasi sesuai produk; alamat kirim = Bogor (sesuai footer).
- ROOT CAUSE (white-box 7 Nov 13:21, ProductModal.tsx): interface Product TIDAK PUNYA field spec. Modal menurunkan spesifikasi dari kategori saja: non-sablon selalu "100% Premium Cotton Combed 30s" + "150 gram", pengiriman selalu hardcode "Dikirim dari Palmerah, Jakarta Barat". Ukuran jeans/DTF jatuh ke label "Standar Asia".

## BUG-06 — Harga coret XXL sama dengan harga saat ini | Minor
- Test case: TC-Q05. Kaos 30s ukuran XXL: harga & harga coret sama-sama Rp45.000.
- ROOT CAUSE (white-box 7 Nov 13:21, ProductModal.tsx): harga coret selalu `product.originalPrice` yang statis (id 1: Rp45.000), sedangkan harga jual mengikuti sizeChart. Di XXL keduanya Rp45.000; di 3XL/4XL/5XL coret (45rb) malah LEBIH RENDAH dari harga jual (50-60rb).

## BUG-07 — Harga Work Jacket tidak konsisten di tiga tempat | Minor
- Test case: TC-Q10. Work Jacket - Brown Rp200.000 di Quick View Overshirt vs Rp209.000 di kartu.
- Tambahan 7 Nov 13:17: catatan di modal Work Jacket sendiri "Harga Size M-XL Rp200.000, XXL Rp210.000" tapi Total ukuran M = Rp209.000.
- ROOT CAUSE (white-box 7 Nov 13:21, ProductModal.tsx): produk terkait = `products.filter(p => p.category === product.category && p.id !== product.id).slice(0, 3)`, tampil hanya `formatPrice(p.price)` (harga dasar) — tanpa originalPrice, tanpa harga per ukuran.
- TERKONFIRMASI LIVE 7 Nov 13:21-13:23 WIB: Work Jacket yang sama tampil di TIGA harga berbeda — Rp209.000 di kartu & Quick View sendiri (ukuran M, coret Rp319.000, HEMAT Rp110.000), Rp200.000 di catatan ukuran modal ("M-XL: Rp200.000, XXL: Rp210.000") dan di tile produk terkait, Rp210.000 di catatan ukuran untuk XXL. Dicek di Black & Brown, pola sama. Overshirt terkait juga menampilkan Brown Rp200.000 (harga sendiri Overshirt Rp189.000/coret Rp250.000 konsisten).
- Penjelasan dari data: `data/products.ts` menyebut Work Jacket price 200000 / originalPrice 299000 — cocok dengan harga terkait & catatan ukuran, TIDAK cocok dengan kartu live Rp209.000/Rp319.000. Artinya ada sumber harga kedua (kemungkinan `priceLabel` / sizeChart / versi deploy) yang membuat kartu & Quick View menampilkan 209rb. Open item untuk white-box lanjutan: lacak dari mana angka 209.000/319.000 berasal.

## BUG-08 — Overshirt: deskripsi Inggris + swatch Cobalt abu-abu | Cosmetic/Minor
- Test case: TC-Q09, TC-C03. Deskripsi Inggris tidak konsisten; swatch Cobalt Blue tampil abu-abu.

## BUG-09 — Item pricelist tanpa jalur beli | Minor
- Test case: TC-P03. Kaos Polos Panjang & Sablon A6+ hanya ada di pricelist, tidak ada produk/tombol beli di katalog.

## BUG-10 — Konten tidak konsisten | Minor/Cosmetic
- Test case: TC-C01 s.d. TC-C04. Testimoni "Kaos Oversize Pendek" tidak ada di katalog; penamaan warna campur ID/EN; footer "Made for premium streetwear checkout" padahal tanpa checkout.

## BUG-11 — Render grid lambat (skeleton 2-3 dtk) | Minor
- Test case: TC-PF01. Skeleton muncul ~2-3 dtk setiap ganti search/filter. Perlu ukur ulang di mobile.

## BUG-13 — Tidak ada robots.txt & sitemap.xml | Minor (kandidat, dari test otomatis HTTP)
- Sumber: `node http-smoke.js`, 7 Nov 2026. /robots.txt → 404, /sitemap.xml → 404 (favicon.ico → 200).
- Dampak: mesin pencari tidak punya peta situs; discoverability produk lewat search lebih lemah. Bukan bug fungsional.

## BUG-12 — Filter Tipe menghilangkan Paket Kaos + Sablon | Major
- Sumber: Products.tsx, 7 Nov 13:21. Filter "Tipe Cetak": "polos" = kategori kaos-polos/outerwear/celana-jeans, "sablon" = sablon. Kategori "paket" tidak cocok keduanya → Paket Kaos + Sablon hilang di kedua pilihan Tipe.
- TERKONFIRMASI LIVE 7 Nov 13:21-13:23 WIB: tanpa filter 16 produk (Paket ada, Rp65.000/coret Rp75.000). Tipe "Kaos Polos" → 14 produk, Paket & Sablon DTF hilang. Tipe "Sablon DTF" → 1 produk (hanya Sablon DTF Satuan), Paket hilang. Reset "Hapus Semua" → kembali 16 produk, Paket muncul lagi.
- ROOT CAUSE (white-box): mapping tipe ke kategori tidak mencakup "paket" di pilihan mana pun.

## Bug baru — versi 20 Nov 2026

## BUG-14 — Filter galeri tidak memfilter, malah membuka lightbox | Major
- Sumber: cek ulang 20 Nov 2026, pagi (3x) dan sore (2x). Langkah: buka /galeri, klik filter Komunitas / Sekolah / Perusahaan.
- Aktual: grid tidak berubah, tetap menampilkan 8 item. Klik malah membuka lightbox item lain.
- Harapan: grid hanya menampilkan item sesuai kategori yang dipilih.
- Catatan: baris tombol filter kadang tidak kelihatan saat halaman di-render, ada duplikat tombol Event dan Sablon, dan tidak ada tombol reset Semua.

## BUG-15 — Lightbox galeri rusak: overlay tembus + caption salah | Major
- Sumber: cek ulang 20 Nov 2026. Langkah: buka lightbox dari /galeri.
- Aktual: overlay semi transparan, halaman di belakang tembus, judul tumpang tindih. Lightbox gambar Sablon Plastisol captionnya "Kaos komunitas custom design"; lightbox gambar Merchandise Event captionnya "Seragam sekolah custom"; lightbox dari filter Perusahaan menampilkan gambar DTF Printing dengan caption "Seragam polo shirt perusahaan".
- Harapan: overlay menutupi halaman di belakang, judul terbaca, caption sesuai gambar yang dibuka.

## BUG-16 — Kategori Polo Shirt di Home mengarah ke halaman login, produk polo tidak ada | Major
- Sumber: cek ulang 20 Nov 2026. Langkah: klik kartu "Polo Shirt — Lacoste Pique CVC premium, Mulai Rp85.000" di Home.
- Aktual: diarahkan ke /dashboard/pesanan/baru, lalu ke /login. Tidak ada produk polo di katalog dan tidak ada filter kategori Polo.
- Harapan: kartu mengarah ke /katalog atau ke halaman produk polo; kalau produknya memang belum ada, kartu jangan ditampilkan.

## BUG-17 — Harga kategori Home tidak cocok dengan produk mana pun | Minor
- Sumber: cek ulang 20 Nov 2026. Kartu "Hoodie & Jacket — Mulai Rp155.000" di Home.
- Aktual: hoodie harganya Rp125.000 (coret Rp145.000), jaket termurah Rp300.000. Tidak ada produk seharga Rp155.000.
- Harapan: harga "mulai dari" sama dengan harga termurah di kategori itu.

## BUG-18 — Pencarian memakai substring mentah | Minor
- Sumber: cek ulang 20 Nov 2026. Langkah: cari "polo" di katalog.
- Aktual: hasilnya dua produk Kaos Polos, karena "polo" ada di dalam kata "Polos". Tidak ada kaos polo.
- Harapan: pencarian mencocokkan kata utuh atau memberi tahu pengguna tidak ada hasil yang relevan.

## BUG-19 — Gambar produk ilustrasi SVG generik, ada yang salah bentuk | Minor
- Sumber: cek ulang 20 Nov 2026. Gambar Hoodie adalah kaos lengan pendek berkerudung dengan kantong kanguru, bukan hoodie. Kartu Jasa Sablon DTF memakai ulang ilustrasi Sablon Plastisol dari galeri.
- Harapan: gambar produk memakai foto produk asli, atau minimal ilustrasi yang sesuai bentuk produknya.

## BUG-20 — Artikel blog sudah lama, dari 2024 | Cosmetic
- Sumber: cek ulang 20 Nov 2026. Sitemap artikel blog terakhir diubah 5 Agu sampai 15 Nov 2024, sedangkan situs dibuat ulang 20 Nov 2026.
- Harapan: tanggal artikel ditampilkan dan kontennya diperbarui, atau artikel lama ditandai sebagai arsip.

## BUG-21 — Judul halaman katalog dobel | Cosmetic
- Sumber: cek ulang 20 Nov 2026. Title tag halaman /katalog: "Katalog Produk & Apparel | YAJAYA STORE | YAJAYA STORE".
- Harapan: nama toko cukup ditulis sekali.

## BUG-22 — Footer "©2026YAJAYA STORE" tanpa spasi | Cosmetic
- Sumber: cek ulang 20 Nov 2026. Setiap footer tertulis "©2026YAJAYA STORE".
- Harapan: "© 2026 YAJAYA STORE".

## BUG-23 — Iframe peta di /kontak tampil kosong | Minor (kandidat)
- Sumber: cek ulang 20 Nov 2026. Iframe peta di halaman kontak tampil kosong saat dibuka.
- Harapan: peta lokasi toko tampil, atau iframe dihapus kalau memang tidak dipakai. Perlu dicek ulang, bisa jadi masalah jaringan saat pengecekan.
