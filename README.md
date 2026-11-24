# Kantin 88 - Universitas Tarumanagara (React + Bootstrap)

Versi React dari proyek PHP `kantin88`.

## Menjalankan
```bash
npm install
npm run dev      # buka http://localhost:5173
npm run build    # hasil di folder dist/
```

## Struktur
| PHP lama                     | React                                   |
|------------------------------|-----------------------------------------|
| includes/data.php            | src/data/data.js + src/utils/format.js  |
| includes/header.php          | src/components/Header.jsx               |
| includes/navbar.php          | src/components/Navbar.jsx               |
| includes/footer.php          | src/components/Footer.jsx + CartDrawer.jsx |
| assets/js/keranjang.js       | src/context/CartContext.jsx             |
| index.php                    | src/pages/Home.jsx (+ Nota.jsx)         |
| menu.php                     | src/pages/Menu.jsx                      |
| tentang.php / kontak.php     | src/pages/Tentang.jsx / Kontak.jsx      |
| latihan-php.php              | src/pages/Latihan.jsx                   |

Bootstrap dipasang lewat npm (bukan CDN). Routing memakai HashRouter,
jadi bisa dibuka dari hosting statis tanpa konfigurasi server.

## Dashboard Admin
Buka `/#/admin` (atau link "Admin" di footer). PIN bawaan: `88888` (ubah di `adminPin`, `src/data/data.js`).

- **Ringkasan**: pesanan & pendapatan hari ini, antrean, menu habis, menu terlaris.
- **Pesanan**: saring per status, alur Baru → Diproses → Siap diantar → Selesai (atau Batal).
- **Kelola Menu**: tambah/ubah/hapus menu, ubah harga, tandai habis.

Data disimpan di `localStorage` browser (tanpa server), jadi pesanan dari tab pelanggan
muncul di tab admin pada browser yang sama. PIN hanya pengaman demo, bukan keamanan sungguhan;
untuk produksi perlu backend + autentikasi.

| File baru | Fungsi |
|---|---|
| src/context/StoreContext.jsx | state pesanan, menu, login admin |
| src/pages/Admin.jsx | login + kerangka tab |
| src/components/admin/*.jsx | Ringkasan, PesananAdmin, MenuAdmin |

## Pembayaran, Barcode & Grafik (pembaruan)
- **Metode pembayaran**: pelanggan memilih **QRIS** atau **Tunai** di keranjang. Disimpan di pesanan (`bayar`, `lunas`).
  Pesanan lama tanpa metode dianggap QRIS. Admin menandai pesanan **Lunas** secara manual (belum ada gateway pembayaran).
- **Barcode = ID pesanan**: barcode Code 128 sungguhan (`src/utils/code128.js`, tanpa library) menyandikan ID seperti `K88-102`.
  Muncul di nota, layar "Pesanan diterima", dan di tiap baris pesanan admin. Kolom cari di admin bisa menerima hasil scanner barcode.
- **Grafik (Ringkasan admin)**: donat **QRIS vs Tunai** (jumlah transaksi & nominal; pilih Hari ini / 7 hari / Semua)
  dan batang **pendapatan 7 hari terakhir** per metode. Seluruh label berbahasa Indonesia, format Rupiah `id-ID`, zona WIB.
- **Pesanan admin**: cari ID/nama/meja, filter metode bayar, tombol Tandai lunas, **Unduh CSV**.

| File baru | Fungsi |
|---|---|
| src/utils/code128.js | encoder Code 128-B |
| src/components/Barcode.jsx | render barcode SVG dari ID |
| src/components/admin/GrafikBayar.jsx | grafik donat + batang (SVG) |
