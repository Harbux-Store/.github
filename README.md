# .github

Repo ini menyimpan **berkas bersama untuk seluruh organisasi Harbux Store**. Tidak ada kode
aplikasi di sini.

GitHub membaca repo bernama `.github` secara khusus: isinya dipakai sebagai bawaan untuk
semua repo lain di organisasi yang belum punya berkasnya sendiri.

## Isi

| Berkas | Guna |
|---|---|
| [`profile/README.md`](profile/README.md) | Halaman depan organisasi — yang tampil di [github.com/Harbux-Store](https://github.com/Harbux-Store) |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | Panduan kontribusi: cara menjalankan, aturan kode, konvensi commit |
| [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) | Kode etik yang berlaku di semua ruang Harbux |
| [`SUPPORT.md`](SUPPORT.md) | Ke mana harus bertanya — pengguna aplikasi, pemasang sendiri, atau laporan keamanan |
| [`.github/ISSUE_TEMPLATE/`](.github/ISSUE_TEMPLATE) | Templat laporan bug dan usulan fitur |
| [`.github/PULL_REQUEST_TEMPLATE.md`](.github/PULL_REQUEST_TEMPLATE.md) | Kerangka deskripsi pull request |

## Cara kerjanya

Kalau sebuah repo Harbux **tidak** punya `CONTRIBUTING.md` sendiri, GitHub memakai yang ada
di sini. Begitu repo itu menambahkan versinya sendiri, versi lokal yang menang. Jadi berkas
di sini adalah **dasar bersama**, bukan aturan yang mengunci.

Pengecualian: `profile/README.md` hanya berlaku di repo ini — tidak diwariskan ke mana pun.

## Mengubah isinya

Perubahan di sini berdampak ke semua repo organisasi sekaligus, jadi kirimkan lewat pull
request dan jelaskan alasannya, jangan langsung dorong ke `main`.
