# Harbux Store

Top-up Robux murah, aman, dan terpercaya — harga bersahabat, proses cepat.

Kami membangun perangkat lunak untuk mengelola usaha jual-beli **Robux dan akun Roblox** —
dari pencatatan stok, transaksi, sampai laporan profit.

Sebelumnya semua dicatat manual di spreadsheet: stok Robux tiap akun mudah salah hitung,
profit hanya bisa ditebak, dan tidak ada pemisahan data antar penjual. Harbux dibuat untuk
menggantikan itu dengan satu aplikasi yang menghitung sendiri.

---

## Project

### [Harbux Pantau](https://github.com/Harbux-Store/Harbux_Pantau-)

Aplikasi web untuk memantau penjualan Robux dan akun-akun Roblox. Catat akun beserta
transaksinya, lalu Harbux menghitung sisa Robux, profit, dan laporan secara otomatis.

**Yang dikerjakan aplikasi ini**

| Bagian | Fungsi |
|---|---|
| **Ringkasan** | Profit hari ini & 30 hari, total stok Robux, harga beli/jual rata-rata, grafik penjualan |
| **Akun** | Daftar akun Roblox dengan stok Robux dan status *Ready / Pending / Borrow* |
| **Transaksi** | Topup, penjualan, biaya lain, dan transfer Robux antar akun |
| **Laporan** | Pendapatan & profit per rentang tanggal, ekspor CSV untuk Excel / Google Sheets |
| **Pengguna** | Admin menambah dan menghapus akun pengguna |

**Keputusan teknis yang jadi dasar pengembangan**

- **Data tiap pengguna terpisah.** Setiap query akun dan transaksi lewat pembatas akses
  (`accScope` / `txScope`) — admin melihat semua, staff hanya miliknya sendiri. Ini aturan
  yang tidak boleh dilewati di fitur baru.
- **Saldo selalu turunan dari transaksi.** Stok Robux tidak pernah diedit langsung; ia
  bertambah dari *topup* dan transfer masuk, berkurang dari penjualan dan transfer keluar.
  Biaya/lain-lain tidak menyentuh Robux. Transaksi berstatus *Pending* belum dihitung.
- **Profit dihitung dengan HPP.** `pendapatan − (robux terjual × harga beli rata-rata) − biaya`,
  bukan sekadar selisih uang masuk-keluar.
- **Satu binary, dua database.** API yang sama berjalan di SQLite (untuk uji coba lokal,
  tanpa konfigurasi apa pun) maupun MySQL/MariaDB (untuk produksi) — cukup diatur lewat
  variabel `HARBUX_DSN`.
- **Tanpa CORS.** Next.js meneruskan `/api/*` dan `/auth/*` ke server API, sehingga browser
  hanya pernah berbicara dengan satu domain dan cookie login tetap aman di domain itu.
- **Bahasa Indonesia** dipakai di seluruh antarmuka dan pesan error — penggunanya penjual,
  bukan developer.

**Teknologi**

- Backend: Go 1.27 (`net/http`), autentikasi JWT via cookie, SQLite / MariaDB
- Frontend: Next.js 16 + React 19, TypeScript, Tailwind CSS 4, mode terang & gelap
- Deploy: web di Vercel, API + database di server Debian sendiri (systemd + Nginx + HTTPS),
  lengkap dengan skrip backup terjadwal

---

## Cara kami bekerja

- **Satu commit = satu perubahan logis.** Judul singkat bahasa Indonesia, badan commit
  menjelaskan apa yang berubah dan kenapa.
- **Tes sebelum push.** `go test ./...`, `npx tsc --noEmit`, dan `npx eslint src` harus lulus.
- **Dokumentasi ikut kode.** Panduan pemakaian ada di README aplikasi, panduan pemasangan
  di `deploy/README.md`, dan konvensi pengembangan di `AGENTS.md`.
- **Keamanan bukan tambahan belakangan.** Pembatasan percobaan login, cookie `Secure` saat
  HTTPS, dan kunci rahasia minimal 32 karakter sudah jadi bagian dari rilis.

---

## Status

Harbux Pantau sudah dipakai untuk operasional sehari-hari. Pengembangan berlanjut pada
pendalaman laporan dan penyempurnaan alur pencatatan.
