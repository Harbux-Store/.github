# Panduan Kontribusi

Terima kasih sudah mau ikut mengerjakan Harbux. Dokumen ini berlaku untuk semua repo di
organisasi Harbux Store.

## Sebelum menulis kode

- **Untuk perbaikan bug kecil** — langsung kerjakan, tidak perlu izin.
- **Untuk fitur baru atau perubahan alur** — buka issue dulu dan jelaskan masalah yang mau
  diselesaikan. Lebih baik berdebat soal arah selama 10 menit di issue daripada membuang
  seharian menulis kode yang arahnya tidak dipakai.

## Menyiapkan lingkungan kerja

Untuk [Harbux Pantau](https://github.com/Harbux-Store/Harbux_Pantau-):

```bash
# Backend (Go) — port 8080
cd api && go build -o cashflow-server . && ./cashflow-server

# Frontend (Next.js) — port 3000
cd web && npm install && npm run dev
```

Tanpa variabel lingkungan apa pun, API memakai SQLite di `api/data/cashflow.db`. Database
kosong membuat halaman login otomatis berubah jadi **Buat Admin Pertama** (sandi minimal
8 karakter) — jadi kamu tidak perlu menyiapkan data awal.

## Aturan yang tidak boleh dilanggar

Ini bukan soal selera, melainkan hal yang membuat data pengguna tetap benar:

- **Pembatasan data per pengguna.** Setiap query akun dan transaksi wajib lewat
  `accScope` / `txScope`. Admin melihat semua, staff hanya miliknya. Query yang melewati
  ini membocorkan data penjual lain.
- **Saldo adalah turunan transaksi.** Stok Robux tidak pernah diubah langsung — ia
  bertambah dari *topup* dan transfer masuk, berkurang dari penjualan dan transfer keluar.
  Biaya dan pengeluaran lain tidak menyentuh Robux.
- **Transaksi *Pending* belum dihitung** ke saldo sampai diubah jadi *Selesai*.
- **Dua dialek database.** API berjalan di SQLite maupun MySQL/MariaDB. Setiap SQL baru
  harus jalan di keduanya — perhatikan `schemaFor`, `hasColumn`, dan pembungkus
  `CAST(... AS SIGNED)` pada agregat.
- **Bahasa Indonesia** untuk seluruh teks antarmuka dan pesan error. Penggunanya penjual,
  bukan developer.

## Tes sebelum mengirim

Ketiganya harus lulus:

```bash
cd api && go test ./...
cd web && npx tsc --noEmit && npx eslint src
```

## Commit

- **Satu commit = satu perubahan logis** — satu fitur, satu perbaikan, atau satu refactor.
  Bukan "update banyak hal".
- Judul singkat bahasa Indonesia, lalu badan commit berisi **apa yang berubah dan kenapa**.
  Alasan lebih berharga daripada daftar berkas — daftar berkas sudah ada di diff.
- Perapian format atau penggantian nama berkas dipisah dari commit fitur, supaya diff
  fiturnya tetap mudah dibaca.

Contoh:

```
Tambah edit transaksi

- Tombol Edit pada tabel transaksi membuka modal yang sama dengan form tambah.
- Saldo akun dihitung ulang setelah perubahan supaya tidak melenceng dari riwayat.
```

## Pull request

1. Kerjakan di branch terpisah, bukan langsung di `main`.
2. Jelaskan di deskripsi PR: masalah apa yang diselesaikan, dan bagaimana kamu mengujinya.
3. Sertakan tangkapan layar bila ada perubahan tampilan.
4. Sebut juga hal yang **sengaja tidak** kamu kerjakan, kalau ada — itu menghemat waktu
   yang mengulas.

## Celah keamanan

Jangan dilaporkan lewat issue atau PR publik. Lihat [SUPPORT.md](SUPPORT.md).
